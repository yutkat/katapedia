# Chef


### 使い方

1. レポジトリ作成

`knife solo init chef-repo`


2. cookbook作成

`knife cookbook create base -o chef-repo/site-cookbooks/`


3. cookbookダウンロード

`knife cookbook site install yum -o cookbooks`

`berks vendor cookbooks`


4. chef-soloの準備

`knife solo prepare xxx@xxx`


5. chef-soloの実行

`knife solo cook xxx@xxx`


#### knife solo prepare xxxしたらcurl: (7) couldn't connect to hostに

http_proxyが設定されていない。/etc/bashrcでなく、~/.bashrcに記載したらうまくいった。


#### cook中にyum-dump Locking Error!のエラーが表示される

yumのタイムアウトが発生。

/gems/chef-11.4.0/lib/chef/provider/package/yum-dump.py のTIMEOUTの設定値を10秒から変更する。


## chefの注意点

* proxy越しだといろいろ問題があったりする
* sudoをノーパスワードで実行できるユーザが必要
* ssh越しにsudoするためsudo visudo
Defaults    !requiretty

を追加



### proxy環境でのchef

1. sudoersの環境変数を引き継ぐ設定を行う（http_proxy,https_proxy）

### CentOS7で最速インストール方法

~~~
sudo yum install ruby.x86_64 ruby-devel.x86_64 rubygems.noarch rubygems-devel.noarch gcc
~~~


