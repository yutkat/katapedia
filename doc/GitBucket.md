# GitBucket

## フロー

### Pull Request

#### Pull Requestに書くこと

PRの大雑把な目的

issue のリンク

PRの前提条件

PRで実装、変更、改廃した機能

PRの懸念事項

PRでやりたかったこと(目的)とどうしたか、なぜそうしたか、どのように実現したかほかにレビュワーに見て欲しいこと(この書き方ダサくない？もっといいやり方ない？ここどう思う？)


*「受理」タイプ：受け取りました

将来のリリース時に変更が反映される、ということ。やったね！

*「却下」タイプ：受け取れません

残念

却下」された場合でも、キミの変更すべてを否定しているわけではないスレで相談するなりして、皆の意見ももらって、再チャレンジしてみよう

* 「差し戻し」タイプ：このままでは受け取れませんので、修正して欲しいです
この場合、修正して欲しい点を教えてくれる一番多い返答なのでメゲないようにしようムッとくるかもしれないけど、返事をよく読めば、相手が指摘しているのは文章のごく一部のはず。パパっと直そうGitHubのキミのフォルダに戻って、編集して、Commit したら即座に作者に伝わる。編集が落ち着いたら、Pull Requests のところに「もう一度見て欲しい」と書こう


https://gist.github.com/xyzzy-17-638/1962125


#### よいPull Request

10のヒントのリスト


1. 小さくまとめる
1. 1つの事だけやる
1. 行の幅に注意する
1. フォーマットの変更はしない
1. ビルドできるコードをのみを含める
1. 全てのテストをパスさせる
1. テストを追加する
1. 理由を書く
1. 上手に書く
1. スラッシングを避ける

#### 書くべき項目

* 変更の概要
* UIの変更点
* スクリーンショットやアニメーションなどで提示する
* 機能の使い方/バグの再現手順
* テスト項目
* ユニットテストではなく、テスト環境でレビュワーに確認してほしい項目
* 今回保留にしたToDo
* 備考
* 関連URL
* 参考にしたリソース
* チケット

http://post.simplie.jp/posts/37


#### サンプル

~~~
## 変更の概要

- 現状の問題点
- この変更が解決すること
- ...

## UIの変更点

- 変更前のUI
- 変更後のUI

## 機能の使い方/バグの再現手順

1. 変更した機能の使い方
2. バグであればその再現手順
3. ...

## テスト項目

- [ ] テスト環境でレビュワーに確認してほしいこと
- [ ] ...
- [ ] ...

## 今回保留にしたToDo

- [ ] 今回は保留にしたが、今後対応しなければならないこと
- [ ] 必要に応じてチケットやissueを発行する
- [ ] ...

## 備考

- レビュワーに対する注意点など
- ...
- ...

## 関連URL

- 参考にしたリソース
- チケット
- ...
~~~


## Customise

### メール通知のFromを強制的にユーザ名とする

src/main/scala/util/Notifier.scala

~~~
email.setFrom(address, context.loginAccount.get.fullName)
~~~

## プラグイン

### インストール方法

1. System Settings

2. GITBUCKET_HOME を確認

3. そこにpluginsディレクトリをなければ作成

4. プラグイン*.jarをコピーする

5. gitbucket再起動

6. System Settings->Pluginに反映される


## 実行

java ~~jar gitbucket.war ~~ --port=8082 --prefix=/gitbucket


~~~
#! /bin/sh
### BEGIN INIT INFO
# Provides:          gitbucket
# Required-Start:    $remote_fs $syslog
# Required-Stop:     $remote_fs $syslog
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: Gitbucket initscript
# Description:       Starts and stops gitbucket running as a standalone servlet.
### END INIT INFO

# Author: Alex Schneider

# Do NOT "set -e"

# PATH should only include /usr/* if it runs after the mountnfs.sh script
PATH=/sbin:/usr/sbin:/bin:/usr/bin
DESC="Gitbucket git hosting"
USERNAME=git
NAME=git
# Location to java executable
DAEMON="/usr/bin/java"
# Include JVM arguments and all other arguments.
DAEMON_ARGS="-jar /opt/gitbucket/gitbucket.war - --port=8082 --prefix=/gitbucket"
PIDFILE=/var/run/$NAME.pid
SCRIPTNAME=/etc/init.d/$NAME

# Read configuration variable file if it is present
[ -r /etc/default/$NAME ] && . /etc/default/$NAME

# Load the VERBOSE setting and other rcS variables
. /lib/init/vars.sh

# Define LSB log_* functions.
# Depend on lsb-base (>= 3.2-14) to ensure that this file is present
# and status_of_proc is working.
. /lib/lsb/init-functions

#
# Function that starts the daemon/service
#
do_start()
{
# Return
#   0 if daemon has been started
#   1 if daemon was already running
#   2 if daemon could not be started
start-stop-daemon --start --quiet --background --chuid $USERNAME --make-pidfile --pidfile $PIDFILE --exec $DAEMON --test > /dev/null         || return 1
start-stop-daemon --start --background --chuid $USERNAME --make-pidfile --quiet --pidfile $PIDFILE --exec $DAEMON --         $DAEMON_ARGS         || return 2
# Add code here, if necessary, that waits for the process to be ready
# to handle requests from services started subsequently which depend
# on this one.  As a last resort, sleep for some time.
}

#
# Function that stops the daemon/service
#
do_stop()
{
# Return
#   0 if daemon has been stopped
#   1 if daemon was already stopped
#   2 if daemon could not be stopped
#   other if a failure occurred
start-stop-daemon --stop --quiet --retry=TERM/30/KILL/5 --pidfile $PIDFILE --name $NAME
RETVAL="$?"
[ "$RETVAL" = 2 ] && return 2
# Wait for children to finish too if this is a daemon that forks
# and if the daemon is only ever run from this initscript.
# If the above conditions are not satisfied then add some other code
# that waits for the process to drop all resources that could be
# needed by services started subsequently.  A last resort is to
# sleep for some time.
start-stop-daemon --stop --quiet --oknodo --retry=0/30/KILL/5 --exec $DAEMON
[ "$?" = 2 ] && return 2
# Many daemons don't delete their pidfiles when they exit.
rm -f $PIDFILE
return "$RETVAL"
}

#
# Function that sends a SIGHUP to the daemon/service
#
do_reload() {
#
# If the daemon can reload its configuration without
# restarting (for example, when it is sent a SIGHUP),
# then implement that here.
#
start-stop-daemon --stop --signal 1 --quiet --pidfile $PIDFILE --name $NAME
return 0
}

case "$1" in
start)
[ "$VERBOSE" != no ] && log_daemon_msg "Starting $DESC" "$NAME"
do_start
case "$?" in
0|1) [ "$VERBOSE" != no ] && log_end_msg 0 ;;
2) [ "$VERBOSE" != no ] && log_end_msg 1 ;;
esac
;;
stop)
[ "$VERBOSE" != no ] && log_daemon_msg "Stopping $DESC" "$NAME"
do_stop
case "$?" in
0|1) [ "$VERBOSE" != no ] && log_end_msg 0 ;;
2) [ "$VERBOSE" != no ] && log_end_msg 1 ;;
esac
;;
status)
status_of_proc "$DAEMON" "$NAME" && exit 0 || exit $?
;;
#reload|force-reload)
#
# If do_reload() is not implemented then leave this commented out
# and leave 'force-reload' as an alias for 'restart'.
#
#log_daemon_msg "Reloading $DESC" "$NAME"
#do_reload
#log_end_msg $?
#;;
restart|force-reload)
#
# If the "reload" option is implemented then remove the
# 'force-reload' alias
#
log_daemon_msg "Restarting $DESC" "$NAME"
do_stop
case "$?" in
0|1)
do_start
case "$?" in
0) log_end_msg 0 ;;
1) log_end_msg 1 ;; # Old process is still running
*) log_end_msg 1 ;; # Failed to start
esac
;;
*)
# Failed to stop
log_end_msg 1
;;
esac
;;
*)
#echo "Usage: $SCRIPTNAME {start|stop|restart|reload|force-reload}" >&2
echo "Usage: $SCRIPTNAME {start|stop|status|restart|force-reload}" >&2
exit 3
;;
esac
~~~

