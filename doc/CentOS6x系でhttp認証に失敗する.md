# CentOS6x系でhttp認証に失敗する

centos6.xのgitバージョンが古すぎてhttp認証でpushできない


~~~
error: Cannot access URL gitserver, return code 22
fatal: git-http-push failed
~~~

解決方法は２つ


1. .netrcに以下を追加


~~~
machine gito.example.org
login hogehoge
password fugafuga
~~~

http://d.hatena.ne.jp/torutk/20130127/p1



2. .git/configのURLにログインユーザ名（hogehoge@ipadr）を追加

