# Omniauthによるアカウント統合

Redmine,jenkins,gitlabをアカウント統合する。


詳しくは↓


http://d.hatena.ne.jp/suer/20130530/redmine_oauth


こっち↓のソフトはバグってる。


http://d.hatena.ne.jp/suer/20130706/omniauth_redmine



### ハマったところ

* redmine-oauth-pluginがインストールできない。
-> marvenが必要。

~~~
sudo aptitude install maven2 openjdk-7-jdk
sudo -u jenkins mvn install
~~~
* proxy設定
$JENKINS_HOME/.m2/settings.xmlを作成

~~~
<settings>
<proxies>
<proxy>
<active>true</active>
<protocol>http</protocol>
<host>ip_address</host>
<port>port</port>
<username></username>
<password></password>
<nonProxyHosts></nonProxyHosts>
</proxy>
</proxies>
</settings>
</code>
~~~







