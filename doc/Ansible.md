# Ansible

https://galaxy.ansible.com/


## 基礎

http://tdoc.info/blog/2013/04/20/ansible.html


## 始め方

http://yteraoka.github.io/ansible-tutorial/


### 導通確認

1. echo -e "[server-grouping-name]ntarget_server_ip:port" > hosts
1. ansible 127.0.0.1:2222 -m ping -i hosts -u root -k -s
1. ansible -i hosts server-grouping-name -a 'uname -r'

### 構文確認

1. ansible-playbook -i hosts simple-playbook.yml --syntax-check
1. ansible-playbook -i hosts simple-playbook.yml --list-tasks
1. ansible-playbook -i hosts simple-playbook.yml --check

http://yteraoka.github.io/ansible-tutorial/


### 実行

一回ログインしないと

`fatal: : FAILED! => {"failed": true, "msg": "Using a SSH password instead of a key is not possible because Host Key checking is enabled and sshpass does not support this.  Please add this host's fingerprint to your known_hosts file to manage this host."}`

というエラーになる

`ssh vagrant@xxx -p xxx`



`ansible-playbook -i hosts site.yml -k`

`ansible-playbook -i hosts site.yml -k --ask-become-pass` ssh先でsudoする場合



-kを入れないとパスワード入力ができずエラーになる


### 途中から実行

ansible-playbook site.yml -l redmine --start-at="redmine : install plugins in github"


## Playbookの記述

まずはメインのplaybookを作る


site.yml


~~~
---
- include: webservers.yml
- include: dbservers.yml
~~~


そしてsite.ymlから読み込むグループごとのプレイブックを同じくトップレベルのディレクトリに作成します。


webservers.yml


~~~
---
- hosts: webservers
roles:
- common
~~~

dbservers.yml


~~~
---
- hosts: dbservers
roles:
- common
~~~


http://knowledge.sakura.ad.jp/tech/3084/


## ディレクトリ構成

~~~
production # inventory file for プロダクション
staging # inventory file for ステージ
group_vars/
group1 # グループごとの変数をまとめておく
host_vars/
hostname1 # ホスト固有の値を設定する
site.yml # 全ての起点のplaybook
webservers.yml # playbook for webサーバ
dbservers.yml # playbook for dbサーバ
roles/
nginx/ # ロールごとに作成(Chefでいうクックブック単位)
tasks/ # 実行したい処理
main.yml # nginxのインストール処理
handlers/ # main.yml # notifyで呼ばれるハンドラ
templates/
nginx.conf.j2 # nginxのコンフィグファイル
files/ bar.txt # 変数不要で配備したいファイル
vars/
main.yml # このロールの変数を設定
~~~

`mkdir -p files filter_plugins group_vars handlers host_vars library production roles staging tasks templates vars`



http://docs.ansible.com/ansible/playbooks_best_practices.html


## モジュール

http://www.infiniteloop.co.jp/blog/2013/08/ansible/


### apt,yum

~~~
tasks:
- apt: name=nginx
notify: restart nginx
handlers:
- name:
restart nginx
service:
name=nginx
state=restarted
~~~

~~~
tasks:
- apt:
name=nginx
state=latest
when: ansible_os_family == "Debian"
~~~

### file

~~~
- file:
path=/etc/foo.conf
owner=foo
group=foo
mode=0644
~~~
~~~
- file: path=/etc/some_directory
state=directory
mode=0755
~~~


### copy

~~~
- copy:
src=/srv/myfiles/foo.conf
dest=/etc/foo.conf
owner=foo
group=foo
mode=0644
~~~

#### 複数ファイルをコピーする

~~~
- name: setup configuration
copy:
src: "{{ item.src }}"
dest: "{{ item.dest }}"
with_items:
- { src: './files/sysctl.conf', dest: '/etc/sysctl.conf' }
- { src: './files/i18n', dest: '/etc/sysconfig/i18n' }
~~~

https://stackoverflow.com/questions/36696952/copy-multiple-files-with-ansible


### template

~~~
- template:
src=/mytemplates/foo.j2
dest=/etc/file.conf
owner=bin
group=wheel
mode=0644
erb: <%= hoge %>
jinja2: {{ hoge }}
~~~

### スクリプト

~~~
tasks:
# 冪等性無し
- script: my_command.sh
# 冪等性有り
- script: my_command.sh creates=/tmp/done.txt
~~~

### get_url

~~~
tasks:
- name: download foo.conf
get_url: url=http://example.com/path/file.conf dest=/etc/foo.conf
mode=0440
~~~

## ユースケース別タスクの書き方

### 再起動させたい

~~~
- name: reboot!
command: shutdown -r now

- name: wait for SSH port up
wait_for: host={{ inventory_hostname }} port={{ ansible_port }} state=started delay=30 timeout=60
delegate_to: 127.0.0.1
become: no
~~~


## Tips

### `FAILED! => {"msg": "to use the 'ssh' connection type with passwords, you must install the sshpass program"}`

sshpassをインストールする必要がある

`sudo apt-get install sshpass`

コマンドオプションで
`-c paramiko`
でもうまく行くみたい

http://d.hatena.ne.jp/yk5656/20141016/1415403630


### scriptとcommandとshellの違い

shell:シェルコマンドを実行。これを使っておけば無難

script：シェルスクリプトを実行するときに使う

command:環境変数を読めなくてよい、リダイレクトやパイプをしないときに使う


## FAQ

### 初回にでるフィンガープリントを無視する Are you sure you want to continue connecting

ansible.cfg

```
[defaults]
host_key_checking = False
```

### fatal: : FAILED! => {"changed": false, "failed": true, "module_stderr": "sudo: パスワードが必要ですn", "module_stdout": "", "msg": "MODULE FAILURE", "rc": 1}

become: yesがあるためローカルのコマンドもsudoで打とうとする

そのタスクだけbecome: noにすると大丈夫



### Aborting, target uses selinux but python bindings (libselinux-python) aren't installed! というエラーがでる

libselinux-pythonがないことが原因

インストールすること

