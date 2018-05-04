# Jenkins

## Jenkinsの環境変数

$JOB_NAME：ビルドのプロジェクト名

$WORKSPACE：ワークスペースのディレクトリパス


https://wiki.jenkins-ci.org/display/JA/Building+a+software+project#Buildingasoftwareproject-Jenkins%E3%81%8C%E8%A8%AD%E5%AE%9A%E3%81%99%E3%82%8B%E7%92%B0%E5%A2%83%E5%A4%89%E6%95%B0


## よく使うシェルコマンド

### sloccount

sloccount --duplicates --wide --details * > $WORKSPACE/sloccount.sc


### cccc

find . -name "*.h" -o -name "*.cc" -o -name ".c" | tr 'n' ' '  | xargs  cccc


### cppcheck

cppcheck --enable=all --suppress=variableScope --xml . 2> cppcheck_result.xml



### doxygen

find . | grep ".c$|.cc$|.h" | xargs -i nkf -w --overwrite {}

nkf -w --overwrite README.md

doxygen Doxyfile


### DRY（pmd（cpd））

find . -type f | egrep '.c$|.cc$|.h$' | xargs -i nkf -w --overwrite {}

/usr/local/bin/pmd/bin/run.sh  cpd --minimum-tokens 100 --files . --language cpp --encoding UTF-8  --format xml >  $WORKSPACE/cpd.xml || true


## 移行

### jenkins-ciのダウンロード

wget -no-proxy http://jenkins_ip:port/jnlpJars/jenkins-cli.jar


### プラグインの移行

java -jar jenkins-cli.jar -s http://jenkins_ip:port/ list-plugins > /tmp/plugins.txt

awk '{print $1}' /tmp/plugins.txt > /tmp/plugins_tripped.txt



cat plugins_tripped.txt | xargs java -jar jenkins-cli.jar -s http://jenkins_ip:port/ install-plugin


## パイプラインプラグイン対応状況

https://github.com/jenkinsci/pipeline-plugin/blob/master/COMPATIBILITY.md


## Troubleshooting

### Jenkins上のMavenのproxy設定

/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/"Jenkins上から入力したMaven名"/conf/settings.xml

にproxyを記述



### ghprbでChecking PR から Created Pull Requestまでの間が遅い

ghprbは複数のジョブを並列で処理できるようになっていないこと原因と思われる。１つのhookから呼び出されるジョブを１つにすればブロックされなくなり速くなる。

https://github.com/jenkinsci/ghprb-plugin/issues/461



### パイプラインでnohup: failed to run command `sh': No such file or directoryとなる

Durable Task Plugin 1.13のバグ。1.12にダウングレードすれば直る

https://wiki.jenkins-ci.org/display/JENKINS/Durable+Task+Plugin


### シェル変数、コマンドが展開される

実行前にevalが入っている模様

バックスラッシュで回避すること



### JenkinsプラグインのProxy設定

~~~
JENKINS_JAVA_OPTIONS="-Djava.awt.headless=true -Dhttp.proxyHost=192.168.1.1 -Dhttp.proxyPort=8080 -Dhttp.nonProxyHosts=localhost"
~~~

### JenkinsでGitがproxy越しに使えない

git->jgitにしたらうまくいった模様。

configure system->git->jgit追加


追加で、以下のコマンドにてプロキシ無効


~~~
sudo git config --system http.proxy "
~~~

## PHP

### スタイルチェック

phpcs --report=checkstyle --report-file="phpcs.xml" PHP || true


### 静的解析

phpmd PHP xml cleancode,codesize,controversial,design,naming,unusedcode --exclude "PHP/test" --reportfile "phpmd.xml" || true


### 重複コード

phpcpd --log-pmd=phpcpd.xml PHP --exclude="PHP/test" || true


### 参考サイト

http://d.hatena.ne.jp/yk5656/20140617/1404310791

http://www.spiceworks.co.jp/blog/?p=4188

http://d.hatena.ne.jp/Kenji_s/20110917/1316216108


## SVN

### コミットトリガーでjenkinsを実行する

svnのpost-commitを使用するしか方法はない。


リポジトリのhooks/post-commitに


~~~
REPOS="$1"
REV="$2"
UUID=`svnlook uuid $REPOS`

wget --no-proxy  http://jenkins_ip:port/jenkins/job/StaticAnalysis_C/build?delay=0sec
~~~


post-commitに実行権限を付けることを忘れないこと！！


## ほしいプラグイン

* 出力したファイルの内容をジョブのトップ画面に表示するプラグイン
-> https://github.com/jenkinsci/description-setter-plugin

* ncmeter用プラグイン

## Tips

### パイプライン使用時はWorkspaceが使えない

https://issues.jenkins-ci.org/browse/JENKINS-33839


### ビルドを一括削除

cd /var/lib/jenkins/jobs/Test/builds

rm -r ビルド番号

ジェンキンス再起動



### プロジェクトページにコンソール出力を表示したい。

出力をファイルにし、

RichTextPublisher

https://wiki.jenkins-ci.org/display/JENKINS/Rich+Text+Publisher+Plugin

HTMLで表示する。


また、正規表現ひっかける

description setter plugin

https://wiki.jenkins-ci.org/display/JENKINS/Description+Setter+Plugin

を使うこともできる



### Jenkinsにパラメータ（引数）を渡す

ビルドのパラメータ化にチェック。

適当な名前をつける

$名前

でパラメータを引ける

