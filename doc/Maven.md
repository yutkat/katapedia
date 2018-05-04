# Maven

### プロキシ設定

~/.m2/settings.xml


~~~
<settings>
<proxies>
<proxy>
<active>true</active>
<protocol>http</protocol>
<host>ip</host>
<port>port</port>
<nonProxyHosts>192.168.1.*</nonProxyHosts>
</proxy>
</proxies>
</settings>
~~~


### プロジェクトの作成

`mvn archetype:create -DgroupId=com.aaa.projectA -DartifactId=prototype`

