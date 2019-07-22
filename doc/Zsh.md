# Zsh


## bindkey

### カーソル移動のキーバインド

カーソルを1文字分戻す

zsh	backward-char		CTRL+B


カーソルを1文字分進める

zsh	forward-char		CTRL+F


行頭にカーソルを移動

zsh	beginning-of-line	CTRL+A


行末にカーソルを移動

zsh	end-of-line		CTRL+E


次のワードに移動

zsh	forward-word		ESC f または M-f


前のワードに移動

zsh	backword-word		ESC b または M-b


### 一文字削除

カーソル前の文字を削除

zsh	backward-delete-char		CTRL+H または DEL


カーソル位置に文字があればそれを削除

なければファイル名補完のリストを表示

zsh	delete-char-or-list		CTRL+D


### 単語削除

前方 CTRL+w

後方 Alt+d


### 削除/復帰-

前のワードを消去

zsh	backward-kill-word		CTRL+W または ESC CTRL+H


ワードを消去

zsh	delete-word			ESC d


入力行全部を消去

zsh	kill-whole-line			CTRL+U


カーソル位置から行末まで消去

zsh	kill-line			CTRL+K


マークセット

zsh	set-mark-command		CTRL+@


カーソルとマークを入れ換える

zsh	exchange-point-and-mark		CTRL+X CTRL+X


消去した文字列をカーソル位置に挿入

zsh	yank				CTRL+Y


領域をコピーするが削除はしない

zsh	copy-region-as-kill		ESC w



### ヒストリ機能

複数行のときは上の行にカーソルを移動する。そうでないときは、前のヒストリを現在の入力行にする。

zsh	up-line-or-history			CTRL+P


複数行のときは下の行にカーソルを移動する。そうでないときは、次のヒストリを現在の入力行にする。

zsh	down-line-or-history			CTRL+N


ヒストリの中を後にサーチする

zsh	history-search-backward			ESC p


ヒストリの中を後にインクリメンタルサーチをする

zsh	history-incremental-search-backward	CTRL+R または CTRL+X r


ヒストリの中を前にサーチする

zsh	history-search-backward			ESC p


ヒストリの中を前にインクリメンタルサーチをする

zsh	history-incremental-search-forward	CTRL+S または CTRL+X s


csh のヒストリを実際に展開する。

zsh	expand-history				ESC ! または ESC SPACE


複数行のときは先頭にカーソルを移動する。そうでないときは、ヒストリの先頭を現在の入力行にする。

zsh	beginning-of-buffer-or-history		ESC <


複数行のときは最後にカーソルを移動する。そうでないときは、ヒストリの最後を現在の入力行にする。

zsh	end-of-buffer-or-history		ESC >


現在のコマンドを実行して、そのコマンドが終了したら、ヒストリの次のコマンドをコマンドラインに出す。ヒストリの次のコマンドがないときは、現在のコマンドも実行されない。

zsh	accept-line-and-down-history		CTRL+O


ヒストリに登録するのを、入力行そのままかシェルによって処理された後の行にするかをきりかえる。

zsh	toggle-literal-history			ESC r または M-r



### 補完機能

補完するか、globbing(ワイルドカード)などを展開する。

zsh	expand-or-complete		CTRL+I


カーソル位置に文字があればそれを削除なければ補完のリストを表示

zsh	delete-char-or-list		CTRL+D


ワードを展開する(globbingや変数など)

zsh	expand-word			CTRL+X *


globbing を展開したリストを表示する

zsh	list-expand			CTRL+X g


補完リストの表示

zsh	list-choices			ESC CTRL+D


## Tips

### zshでcdlsするときに関数とaliasを使わずにchpwdを使う理由

AUTO_CDのときに使えなくなるため

（AUTO_CDとはcdを打たずにディレクトリ名だけで移動できる機能）

あとはpushdとかでも適用されなくなる。


https://superuser.com/questions/601480/always-run-a-command-after-another-command


### 遅いときの調査方法

.zshenv 行頭 に次の処理を追記

zmodload zsh/zprof && zprof

.zshrc 行末 に次の処理を追記

if (which zprof > /dev/null) ;then

zprof | less

fi


### コマンドの一時保存

* Ctrl+Y will paste the last item you cut (with Ctrl+U, Ctrl+K, Ctrl+W, etc.).
* Esc q
* Ctrl+q
* <pre>
Ctrl-A

#

Enter

</pre>

* Esc #
* Alt #

http://unix.stackexchange.com/questions/10825/remember-a-half-typed-command-while-i-check-something

http://unix.stackexchange.com/questions/74184/how-to-save-current-command-on-zsh

http://unix.stackexchange.com/questions/140741/how-to-save-a-command-you-entered-without-executing-it


### コマンド、関数があるか調べる

if whence anyframe-widget-put-history > /dev/nul; then


fi





### lsで絶対パスを取る方法

ls ../test.txt(:a)


### デフォルトで使える色

black

red

green

yellow

blue

magenta

cyan

white


https://wiki.archlinuxjp.org/index.php/Zsh



### プロンプトの設定

これらはデフォルト値なので必要ない。

~~~
PROMPT2='%_> ' # などでコマンドを改行で続けたとき
PROMPT3='?# ' # switch文時
PROMPT4='+%N:%i> ' # シェルスクリプトデバッグ時
SPROMPT="%r is correct? [n,y,a,e]: " # 入力ミスの時に出る
~~~

#### プロンプトをあれこれいじくり回したいときに参照するページ

http://zsh.sourceforge.net/Doc/Release/Zsh-Line-Editor.html

#### 動的文字を挿入したい

zle -R を使う


### oh-my-zsh

インストール

`wget http://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh ~~O ~~ | sh`


## TroubleShooting

### コマンド履歴ファイルが読み込まれない

どのタイミングで発生しているかは不明

`fc -R`



### マニュアルインストールしたzshでfpathが正しく定義されない

`./configure --enable-additional-fpath=/usr/local/share/zsh/site-functions,/usr/share/zsh/vendor-functions,/usr/share/zsh/vendor-completions,/usr/share/zsh/functions/Calendar,/usr/share/zsh/functions/Chpwd,/usr/share/zsh/functions/Completion,/usr/share/zsh/functions/Completion/AIX,/usr/share/zsh/functions/Completion/BSD,/usr/share/zsh/functions/Completion/Base,/usr/share/zsh/functions/Completion/Cygwin,/usr/share/zsh/functions/Completion/Darwin,/usr/share/zsh/functions/Completion/Debian,/usr/share/zsh/functions/Completion/Linux,/usr/share/zsh/functions/Completion/Mandriva,/usr/share/zsh/functions/Completion/Redhat,/usr/share/zsh/functions/Completion/Solaris,/usr/share/zsh/functions/Completion/Unix,/usr/share/zsh/functions/Completion/X,/usr/share/zsh/functions/Completion/Zsh,/usr/share/zsh/functions/Completion/openSUSE,/usr/share/zsh/functions/Exceptions,/usr/share/zsh/functions/MIME,/usr/share/zsh/functions/Misc,/usr/share/zsh/functions/Newuser,/usr/share/zsh/functions/Prompts,/usr/share/zsh/functions/TCP,/usr/share/zsh/functions/VCS_Info,/usr/share/zsh/functions/VCS_Info/Backends,/usr/share/zsh/functions/Zftp,/usr/share/zsh/functions/Zle`


をする



### _unsetopt: function definition file not found

rm ~/.zcompdump


### PATHが相対パスだとパスが通らない

bashでは相対パスでもうまくいくのにzshではダメな模様

readlink -fして絶対パスに直すこと


## プラグイン

* auto-fu.zsh
* antigen
クソ便利！


良さそうなやつ


* zsh-syntax-highlight
* cd-gitroot
* cd-bookmark
* cdd
* knu/z
* zsh-bd
* incr
* zsh-autosugestions
* zsh-history-substring-search
* zsh-git
* zaw
* prezto
http://dev.classmethod.jp/tool/zsh-prezto/


## FAQ


#### PATHに記載されている(N-/)とは

(N-/): 存在しないディレクトリは登録しない。


#### chpwdを一時的に無効にしたい

cd -qで実行する


#### 補完時にGet http:///var/run/docker.sock/v1.13/containers/json: dial unix /var/run/docker.sock: permission deniedとか表示される

usermod -G docker -a your_user_name.

再起動でエラーが表示されなくなる。（dockerサービス再起動ではうまくいかなかった）


https://github.com/docker/docker/issues/7258


#### 新しいコマンドが補完されない

補完のインデックスが更新されていないため


rehash or hash -rf.をうつ


または、


zstyle ":completion:*:commands" rehash 1

を.zshrcに書く。

パフォーマンスが悪くなるが。。。


#### autoload -Uzの意味

http://stackoverflow.com/questions/12570749/zsh-completion-difference

http://linux.die.net/man/1/zshbuiltins


#### compinit -u の意味

セキュリティのために、compinitは以下のことをチェックします。

・補完システムが使用するファイルの所有者がrootか現在のユーザ以外であるか

・ファイルが入っているディレクトリ

・パーミッション：誰でも書き込み可能もしくはグループ書き込み可能か

・rootもしくは現在のユーザ以外が所有しているか


-u : 上記のテストを避けて、すべての発見したファイルを警告なしに使用する


http://d.hatena.ne.jp/ywatase/20071103


## バグ


#### ctrl+backspace

ctrl+backspaceがgnome-terminalのバグでbackspaceと同じkeycode ^?となってしまう。

teratermからは可能だが、しばらくはctrl+wで我慢するしかない。。。


https://github.com/Guake/guake/issues/51

https://bugs.launchpad.net/ubuntu/+source/gnome-terminal/+bug/1341667


#### 5.0.2の履歴（ヒストリー）バグ

これが

~~~
sudo estcmd gather -il -ja -cm -fx ".pdf" "H@/usr/share/hyperestraier/filter/estfxpdftohtml" /opt/redmine/files/dmsf_index -lt -1
~~~

こうなる

~~~
sudo estcmd gather -il ja cm fx ".pdf" "H@/usr/share/hyperraier/filter/stfxpdftohtml" /opt/redmine/flles/dmsf_index /opt/rede/filllles/dmsf pc ut--8 lf -
~~~

~~5.0.5をソースインストールしたら改善されていた。~~


auto-fuのバグが原因。auto-fuをアップデートしたらOK


#### 5.0.2の履歴（ヒストリー）バグ2

setopt hist_reduce_blanksが有効だと


vi -u .vimrc --startuptime aaa  .vimrc

が

vi -u .vimrc --startuptime aaa .vmmrc

こうなる


## リンク

"oh-my-zshテーマ":https://github.com/robbyrussell/oh-my-zsh/wiki/Themes

"oh-my-zshプラグイン":https://github.com/robbyrussell/oh-my-zsh/tree/master/plugins

