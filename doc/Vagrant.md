# Vagrant

## ・仮想マシンを動かす手順


仮想マシンの雛形を用意する → vagrant box add [NAME] [URL]

仮想マシンを作成する → ディレクトリを作成しそこに移動して vagrant init し、作成された Vagrantfile を適宜編集

仮想マシンを起動する → vagrant up


例:

~~~
$ vagrant box add lucid32 http://files.vagrantup.com/lucid32.box
$ mkdir foo
$ cd foo
$ vagrant init lucid32
$ vagrant up
~~~


その他のよく行うと思われる操作:


ssh でログインする → `vagrant ssh`

シャットダウンする → `vagrant halt`

再起動する → `vagrant reload`

ステータスを確認する → `vagrant status`

一時停止する → `vagrant suspend`

一時停止から復帰する → `vagrant resume`

box作成 → `vagrant package --base xxx --output aaa.box`

xxxはVBoxManage list vmsで調べられる

http://mizzy.org/blog/2013/03/11/1/


---


## ・イメージ置き場

http://www.vagrantbox.es/


---


## ・.vagrant.dのディレクトリ場所変更

VAGRANT_HOME D:ICF_AutoCapsule_disabledVagrant.vagrant.d


## 自作ベースイメージの作成方法

公式

https://www.vagrantup.com/docs/boxes/base.html


CentOS6.6

http://te2u.hatenablog.jp/entry/2015/05/11/012225



sudo yum install openssh-clients man git vim wget curl


作り終わったあと最適化したほうがいい

yum clean all

sudo dd if=/dev/zero of=/EMPTY bs=1M

sudo rm -f /EMPTY


## 自前リポジトリサーバの構築


プライベートなVagrantイメージ配布サーバを構築する手順


https://github.com/hollodotme/Helpers/blob/master/Tutorials/vagrant/self-hosted-vagrant-boxes-with-versioning.md


$ #ポートフォワーディングの設定とか共有ディスクの設定とかをオフにする

$ vagrant package --base virtualbox-name --output xxx.box

$ vagrant box add http://xxx/xxx.box

$ vagrant init xxx

$ vagrant up


## vagrantのBoxアップデート

$ vagrant box list

$ vagrant box remove [BOX名]

$ vagrant destroy

$ vagrant up


---


## Tips

### ローカルISOを追加する

http://snowlong.hatenablog.com/entry/2015/11/04/201843


---


## FAQ


### default: Warning: Authentication failure. Retrying...となりvagrant upに失敗する

Timed out while waiting for the machine to boot. This means that

Vagrant was unable to communicate with the guest machine within

the configured ("config.vm.boot_timeout" value) time period.



# SSH ディレクトリの作成と設定

$ mkdir /home/vagrant/.ssh

$ chmod 700 /home/vagrant/.ssh


# vagrant で公開されている Insecure Keypair(安全でないキーペア) のダウンロード

# 今回は開発用のため、こちらのキーペアを利用します。

# -k：SSL証明書の警告を無視、-L：リダイレクト先に再接続、-o：ファイル名を指定して保存

$ cd /home/vagrant/.ssh

$ curl -k -L -o authorized_keys 'https://raw.github.com/mitchellh/vagrant/master/keys/vagrant.pub'


# ダウンロードした公開鍵の権限設定

$ chmod 600 /home/vagrant/.ssh/authorized_keys

$ chown -R vagrant:wheel /home/vagrant/.ssh



### proxy設定

$ http_proxy=http://ip:port vagrant plugin install vagrant-proxyconf



Vagrantfile

~~~
config.proxy.http     = "http://ip:port"
config.proxy.https    = "http://ip:port"
config.proxy.no_proxy = "localhost,127.0.0.1"
~~~


### Vagrant で VirtualBox 上の CentOS 7 へ固定 IP を設定
vagrant upした時のエラー内容

~~~
ARPCHECK=no /sbin/ifup eth1 2> /dev/null
Stdout from the command:
ERROR    : [/etc/sysconfig/network-scripts/ifup-eth] Device eth1 does not seem to be present, delaying initialization.
~~~
解決策

: http://fits.hatenablog.com/entry/2014/08/26/000432

