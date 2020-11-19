# Tmux


## Usage

**コマンドのヘルプを表示する**


なんと--helpとかじゃ詳細がでない。なんてこった

tmux list-commandでコマンド一覧が出るが、覚えてられるのか？？


**ペインを新しいウインドウに**

[Prefix] !


**ペインの入れ替え**

[Prefix] {


**セッションデタッチ**

[Prefix] d


**セッション確認**

`tmux list-sessions`

`tmux ls`


**セッションアタッチ**

`tmux attach -t 0`


**コピーバッファから貼り付け**

[Prefix] =


## 設定の意味

bell-action ビープ音を検知するシンボルが@!@になる

visual-bell ビープ音の代わりにステータスバーにメッセージを出す

bell-on-alert アラート発生時でビープ音を出す

monitor-activity モニターに変化があったら発信する。シンボルが@#@になる

visual-activity モニターに変化があったらステータスバーにメッセージを出す


**色関係**

window-status-last-* 移動前のウィンドウの色

window-status-activity-* アラート発生時のウィンドウの色

window-status-bell-* ベル発生時のウィンドウの色


## Tips

#### Prefixにて使用できるキー

controlといっしょに使用できるキーに限られる。

簡易的に確認するためには、Control-V+"確認したいキー"とやって表示されればOK。


http://superuser.com/questions/395233/how-do-i-bind-the-tmux-prefix-key-to-c


#### ステータスバー

~~~
STATUS_LEFT_SIMPLE="#[fg=black,bg=$MYHOSTCOLOR] #h #[fg=brightcyan,bg=black][#S:#I:#P]#[default]"
STATUS_LEFT_DETAIL="#[fg=green,bg=black] #(lsb_release -c --short) #[fg=black,bg=$MYHOSTCOLOR] #h #[default]#[fg=black,bg=yellow]<#(echo $SSH_CONNECTION | cut -d ' '  -f1)#[fg=brightcyan,bg=black][#S:#I:#P]#[fg=blue,bg=yellow] #[default]"
STATUS_RIGHT_SIMPLE="#[default]#[fg=black,bg=white] %h %d(%a)|%H:%M#[default]"
STATUS_RIGHT_DETAIL="#[fg=white,bg=red]?#(cat /var/lib/update-notifier/updates-available | cut -s -d ' ' -f1 | paste -d ' ' -s)!#[fg=yellow,bg=blue]#(cut -d ' ' -f 1-3 /proc/loadavg)#[default]#[fg=black,bg=white] %h %d(%a) %Y|%H:%M#[default]"
~~~


##### ウィンドウサイズによって動的にstatus-left, status-rightを変更する

https://coderwall.com/p/trgyrq/make-your-tmux-status-bar-responsive

#### ローカルインストール

~~~
#!/bin/bash

# Script for installing tmux on systems where you don't have root access.
# tmux will be installed in $HOME/local/bin.
# It's assumed that wget and a C/C++ compiler are installed.

# exit on error
set -e

TMUX_VERSION=1.8

# create our directories
mkdir -p $HOME/local $HOME/tmux_tmp
cd $HOME/tmux_tmp

# download source files for tmux, libevent, and ncurses
wget -O tmux-${TMUX_VERSION}.tar.gz http://sourceforge.net/projects/tmux/files/tmux/tmux-${TMUX_VERSION}/tmux-${TMUX_VERSION}.tar.gz/download
wget https://github.com/downloads/libevent/libevent/libevent-2.0.19-stable.tar.gz
wget ftp://ftp.gnu.org/gnu/ncurses/ncurses-5.9.tar.gz

# extract files, configure, and compile

############
# libevent #
############
tar xvzf libevent-2.0.19-stable.tar.gz
cd libevent-2.0.19-stable
./configure --prefix=$HOME/local --disable-shared
make
make install
cd ..

############
# ncurses  #
############
tar xvzf ncurses-5.9.tar.gz
cd ncurses-5.9
./configure --prefix=$HOME/local
make
make install
cd ..

############
# tmux     #
############
tar xvzf tmux-${TMUX_VERSION}.tar.gz
cd tmux-${TMUX_VERSION}
./configure CFLAGS="-I$HOME/local/include -I$HOME/local/include/ncurses" LDFLAGS="-L$HOME/local/lib -L$HOME/local/include/ncurses -L$HOME/local/include"
CPPFLAGS="-I$HOME/local/include -I$HOME/local/include/ncurses" LDFLAGS="-static -L$HOME/local/include -L$HOME/local/include/ncurses -L$HOME/local/lib" make
cp tmux $HOME/local/bin
cd ..

# cleanup
rm -rf $HOME/tmux_tmp

echo "$HOME/local/bin/tmux is now available. You can optionally add $HOME/local/bin to your PATH."
~~~

## TroubleShooting

### tmuxでshift-spaceやshift-enter,Ctrl-enterが効かない理由

https://unix.stackexchange.com/questions/310532/can-i-use-shift-space-in-a-tmux-keybinding

xterminfoはここに定義されてる
https://invisible-island.net/xterm/terminfo-contents.html

Neovimは
http://www.leonerd.org.uk/code/libtickit/
でやってるからできる


### attachしたときに表示サイズがおかしくなるので再描画したい

`tmux attach -d`で解決する

https://stackoverflow.com/questions/7814612/is-there-any-way-to-redraw-tmux-window-when-switching-smaller-monitor-to-bigger

### 色がおかしい

tmux -2で起動するか、echo $TERMで256colorがついていることを確認する


## FAQ

#### シンボルの意味

~~~
Symbol	Meaning
*	Denotes the current window.
-	Marks the last window (previously selected).
#	Window is monitored and activity has been detected.
!	A bell has occurred in the window.
~	The window has been silent for the monitor-silence interval.
M	The window contains the marked pane.
Z	The window's active pane is zoomed.
~~~

#### tmuxでコマンド補完

<prefix>-:のあとにtabを押して検索候補を出したい。


現状無理っぽい。


http://superuser.com/questions/579545/how-to-tab-completion-when-typing-command-in-tmux



#### setとset-optionとsetwとset-window-optionの違い

set-option        => set

set-window-option => setw


#### 他の環境と重複しないPrefixは

@C-@がよい模様。押しにくいが。。。


http://superuser.com/questions/74492/whats-the-best-prefix-escape-sequence-for-screen-or-tmux


#### No more windowsというメッセージが表示される

bashrcのscreen設定を見直すこと


#### tmux.confが読み込まれない

C-b : source-file で「.tmux.conf」

