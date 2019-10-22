# Vim


## vimとは

http://postd.cc/vim-galore-1/#what-is-vim


## コマンド一覧と覚え方

http://haya14busa.com/vim-mnemonic/


## よく忘れるコマンド

http://www3.kcn.ne.jp/~okina/vim_blank.html

### surroundで複数行囲む

1. Shift + v
2. :normal yss' ('で囲むとき)

インデントがあるとうまく行かないので一度左に詰めるようにする


### Quickfixの過去履歴を表示

- `chi`
- 戻るとき: `col`

### カーソル下の単語削除

daw	カーソル位置の単語を削除(空白含む) (d + a word)

diw	カーソル位置の単語を削除 (d + inner word)


###  g で始まるコマンドというかキーマップまとめ

http://h-miyako.hatenablog.com/entry/2015/01/31/185620


### 自動的にマークされる

^      最後に挿入モードを抜けた場所

.      最後に変更を加えた場所

' or ` 最後にジャンプした時にいた場所

"      バッファを終了した時にいた場所

0～9   vimを終了する時にいた場所


http://d.hatena.ne.jp/xaxe/20070122


### 戻る系

@:	コマンドの繰り返し

;	移動の繰り返し(右方向)

,	移動の繰り返し(左方向)

&	sコマンドの繰り返し

.	編集の繰り返し

'.	最後の編集位置へ

gv	最後のv選択位置へ

v選択後 o	選択範囲の最初/最後へ

<C-^>	直近開いたバッファへ


### 以前の編集点に戻る

`. カーソル位置へジャンプする

g;  編集位置をさかのぼる


### コマンドモードの履歴を検索

ctrl-f



### 画面操作

H	画面の一番上の行へ移動

L	画面の一番下の行へ移動

Ctrl-e	1 行下にスクロール

Ctrl-y	1 行上にスクロール

Ctrl-f	1 画面下にスクロール

Ctrl-b	1 画面上にスクロール

Ctrl-d	半画面下にスクロール

Ctrl-u	半画面上にスクロール


### 補完コマンド

|  補完機能|  キーマップ|
|---|---|
|1. 行全体|CTRL-X CTRL-L|
|1. 行全体|CTRL-X CTRL-L|
|2. 現在のファイルのキーワード|CTRL-X CTRL-N|
|3. 'dictionary'のキーワード|CTRL-X CTRL-K|
|4. 'thesaurus'のキーワード, thesaurus-style|CTRL-X CTRL-T|
|5. 編集中と外部参照しているファイルのキーワード|CTRL-X CTRL-I|
|6. タグ|CTRL-X CTRL-]|
|7. ファイル名|CTRL-X CTRL-F|
|8. 定義もしくはマクロ|CTRL-X CTRL-D|
|9. Vimのコマンドライン|CTRL-X CTRL-V|
|10. ユーザ定義補完|CTRL-X CTRL-U|
|11. オムニ補完|CTRL-X CTRL-O|
|12. スペリング補完|CTRL-X s|
|13. 'complete'のキーワード|CTRL-N|

補完を中止したいときは「CTRL-E」を押すといいらしい。


### 最後に実行したコマンドを実行する

@:


### 最後に実行した編集を実行する

&



### カーソルの移動位置を戻す

元いた場所に戻る: <C-o>

元いた場所に進む: <C-i>



### 矩形選択での上書き

ctrl+vで領域選択後

c	矩形を置換

***rでなくてc***



### バッファの移動

バッファ番号 CTRL+^

もしくは

:buffer バッファ番号

:b バッファ番号


http://blog.livedoor.jp/nakamura_tech/archives/51363029.html



### 直前に開いていたファイルを開く

C-^ ... 一つ前に開いていたファイルに移動


### ヘッダファイルを開く

「Ctrl-w gf」タブでファイルを開く。

タブとして開くので、ノーマルモードで「gt」で移動



### ウィンドウ操作

Ctrl W + R - To rotate windows up/left.

Ctrl W + r - To rotate windows down/right.

Ctrl W + L - Move the current window to the "far right"

Ctrl W + H - Move the current window to the "far left"

Ctrl W + J - Move the current window to the "very bottom"

Ctrl W + K - Move the current window to the "very top"

Ctrl W + x OR Ctrl W + Ctrl x - Rotates the current focused window with

" 横分割 -> 縦分割への切り替え

CTRL-W t CTRL-W H

" 縦分割 -> 横分割への切り替え

CTRL-W t CTRL-W K

" カレントウィンドウをタブとして移動する

CTRL-W T


:vertical {cmd}

垂直分割をするようにします。例えば、:vertical split は :vsplit のような動作になります。

:leftabove {cmd} :aboveleft {cmd}

水平分割なら上に、垂直分割なら左に新しいウィンドウを作ります。

:rightbelow {cmd} :belowright {cmd}

水平分割なら下に、垂直分割なら右に新しいウィンドウを作ります。

:topleft {cmd}

水平分割なら画面の一番上に、垂直分割なら画面の一番左に、それぞれ幅、高さが最大になるように新しいウィンドウを作ります。

:botright {cmd}

水平分割なら画面の一番下に、垂直分割なら画面の一番右に、それぞれ幅、高さが最大になるように新しいウィンドウを作ります。


### コマンドモード中にペースト

<C-R>" ：コマンドモード中にヤンクからペーストを行う

Rはレジスタと覚える

デフォルトレジスタ「"」


<C-R>/：コマンドモード中にコマンド欄で検索した文字をペースト


Ctrl-W x：ウィンドウを入れ替える


### バッファ

<C-o>：元いた場所にも取る

<C-i>：元いた場所に進む


#### バッファの設定

http://leafcage.hateblo.jp/entry/2013/11/21/083830


### マーク

ma：現在のカーソル位置をマーク名 a に保存

'a：マーク名 a の位置に移動


### 折りたたみ

zo：カーソル下にある折りたたみをひとつ開く

zO：カーソル下にある折りたたみを全て開く

zc：カーソル下にある折りたたみをひとつ閉じる

zC：カーソル下にある折りたたみを全て閉じる

zf ： 選択した範囲が折り畳み


### 表示更新

Ctrl+l


### 矩形貼り付け

1. Ctrl+vで矩形選択モードにする

2. 範囲選択したあとにCtrl+Iで入力モードにする

3. Ctrl+rを押して"入力でヤンクした内容をペースト

4. ESCで抜けて複数行に反映


### 辞書に追加

]s	次のスペルミスの箇所へ移動

[s	前のスペルミスの箇所へ移動

z=	正しいスペルの候補を表示し、選択した単語でスペルミスを修正

zg	カーソル下の単語を正しいスペルとして辞書登録

zw	カーソル下の単語を誤ったスペルとして辞書登録


## vimの基礎

### リストいろいろ

:history

:registers

:marks

:ls (:buffers)

:args

:jumps

:changes

:tags


### マップ

|  コマンド|  ノーマルモード|  挿入モード|  コマンドラインモード|  ビジュアルモード|
|---|---|---|---|---|
|map/noremap|○|-|-|○|
|map/noremap|○|-|-|○|
|nmap/nnoremap|○|-|-|-|
|imap/inoremap|-|○|-|-|
|cmap/cnoremap|-|-|○|-|
|vmap/vnoremap|-|-|-|○|
|map!/noremap!|-|○|○|-|

http://vim-jp.org/vimdoc-ja/map.html


***キーマッピングのオプション***


<silent>：コマンドラインへの出力を抑制します。キーマッピングからコマンドを実行する場合などに指定します。

<unique>：すでにマッピングが存在する場合、エラーにします。通常は上書きされます。

<buffer>：バッファローカルなキーマッピングを定義します。<buffer>を付けるとVim起動時に最初に表示されるバッファのみでしか定義されず他のバッファで使えなくなる。

<expr>：マップ先の文字列を Vim の式とみなして、評価した結果の文字列をマップ先とします。


### ファイルタイプ

http://ftp.vim.org/pub/vim/runtime/indent/


### ファイルパスのデリミタ設定

~~~
function! AddPath(pathlist) abort " {{{
let pathlist = split($PATH, s:delimiter)
for path in map(filter(a:pathlist, 'v:val'), 'expand(v:val)')
if isdirectory(path) && index(pathlist, path) == -1
call insert(pathlist, path, 0)
endif
endfor
let $PATH = join(pathlist, s:delimiter)
endfunction " }}}
call AddPath([
 '/usr/local/texlive/2013/bin/x86_64-linux',
 '/usr/local/texlive/2013/bin/x86_64-darwin',
 '~/.pyenv/bin',
 '~/.plenv/bin',
 '~/.rbenv/bin',
 '~/.ndenv/bin',
 '~/.pyenv/shims',
 '~/.plenv/shims',
 '~/.rbenv/shims',
 '~/.ndenv/shims',
 '~/.anyenv/envs/pyenv/bin',
 '~/.anyenv/envs/plenv/bin',
 '~/.anyenv/envs/rbenv/bin',
 '~/.anyenv/envs/ndenv/bin',
 '~/.anyenv/envs/pyenv/shims',
 '~/.anyenv/envs/plenv/shims',
 '~/.anyenv/envs/rbenv/shims',
 '~/.anyenv/envs/ndenv/shims',
 '~/.cabal/bin',
 '~/.vim/bundle/vim-themis/bin',
])
~~~

## 変数の値を参照

let はecho $

set は path?


キーバインドは :help C-o


## 外部プログラムの実行

makeprg xxx

make


eg.


function! Cppcheck_1()

setlocal makeprg=cppcheck --enable=all %

" earlier it was: " setlocal errorformat=[%f:%l]:%m

" fixed by an advise by Mr. Ingo Karkat

setlocal errorformat+=[%f:%l] -> %m,[%f:%l]:%m

let curr_dir = expand('%:h')

if curr_dir == ''

let curr_dir = '.'

endif

echo curr_dir

execute 'lcd ' . curr_dir

execute 'make'

execute 'lcd -'

exe   ":botright cwindow"

:copen

endfunction


## 設定

### complete

.: The current buffer

w: Buffers in other windows

b: Other loaded buffers

u: Unloaded buffers

t: Tags

i: Included files


## プラグイン

### ctrlp

実行中に <c-r> をタイプすると部分一致検索モードから正規表現検索モードにスイッチします。入力した文字を正規表現パターンとしてファイルを検索出来ます。

もう一度タイプすると通常モードに戻ります。また <c-d> でフルパスモードからファイル名モードへ、<c-f> でMRUファイル検索、<c-b> でバッファ検索へとスイッチ出来ます。


### CCTree

cscopeかccglueをインストールしておく


~~~
cscope -R -b
CCTreeLoadDB $HOME/cscope.out
CCTreeTraceForward
~~~




### Unite

#### コマンド出力をUniteにする

:Unite output<CR>

map



### vim-clang

.clangにパスを書く -I/usr/local/include

の形式

.lvimrcとかだとあとから読まれるため反映されない。


pathに追加してもいけるが、これも.vimrcに記述する必要がある。


### quickrun

~~~
let g:quickrun_config = {
   "cpp" :{
       "type" : "my_cpp"
   },
   "my_cpp" : {
       "command"   : "LANG=C make",
       "exec" : "LANG=C make -f Makefile64.lin86 test",
       "cmdopt" : "-f Makefile64.lin86",
   },
}
~~~

#### 色を付ける

~/.vim/syntax/quickrun.vim


~~~
let g:quickrun_config = {}

augroup QrunRSpec
autocmd!
autocmd BufWinEnter,BufNewFile *_spec.rb set filetype=ruby.rspec
augroup END

let rspec_outputter = quickrun#outputter#buffer#new()
function! rspec_outputter.init(session)
call call(quickrun#outputter#buffer#new().init,  [a:session],  self)
endfunction

function! rspec_outputter.finish(session)
highlight default RSpecGreen ctermfg = Green cterm = none
highlight default RSpecRed    ctermfg = Red   cterm = none
highlight default RSpecComment ctermfg = Cyan  cterm = none
highlight default RSpecNormal  ctermfg = White cterm = none
call matchadd("RSpecGreen", "^[.F]*.[.F]*$")
call matchadd("RSpecGreen", "^.*, 0 failures$")
call matchadd("RSpecRed", "F")
call matchadd("RSpecRed", "^.*, [1-9]* failures.*$")
call matchadd("RSpecRed", "^.*, 1 failure.*$")
call matchadd("RSpecRed", "^ *(.*$")
call matchadd("RSpecRed", "^ *expected.*$")
call matchadd("RSpecRed", "^ *got.*$")
call matchadd("RSpecRed", "^ *Failure/Error:.*$")
call matchadd("RSpecRed", "^.*(FAILED - [0-9]*)$")
call matchadd("RSpecRed", "^rspec .*:.*$")
call matchadd("RSpecComment", " # .*$")
call matchadd("RSpecNormal", "^Failures:")
call matchadd("RSpecNormal", "^Finished")
call matchadd("RSpecNormal", "^Failed")

call call(quickrun#outputter#buffer#new().finish,  [a:session], self)
endfunction

call quickrun#register_outputter("rspec_outputter", rspec_outputter)
let g:quickrun_config['ruby.rspec'] = {
 'command': 'rspec',
 'outputter': 'rspec_outputter',
 }
~~~

#### 出力後最終行に行くようにする

vim-quickrun/autoload/quickrun/outputter/buffer.vim

~~~
64 "  silent normal! zt
65   silent normal! G
~~~


### syntastic

~~~
let g:syntastic_cpp_include_dirs = ['$HOME/Workspace/include',
 '$HOME/Workspace/include2']
~~~

### The NERD Commenter

コメントアウト省力化


<leader>c<space>


nmap <Leader>/ <Plug>NERDCommenterToggle

vmap <Leader>/ <Plug>NERDCommenterToggle


### fugitive.vim

過去ソースを参照しつつ修正


:Gdiff

:Gblame


### surround.vim

クオートとかを便利に入力


|  コマンド|  動作|
|---|---|
|ds'	|文を囲んでいる ' を消す|
|ds'	|文を囲んでいる ' を消す|
|di'	|' で囲まれた部分を消す|
|cs'"	|' を " に変更|
|ci'	|' で囲まれた部分を消して, インサートモードに入る|
|S'	|ビジュアルモードで選択した部分を ' で囲む|
|yss'|	文を ' で囲む|
|ysiw'|	カーソルがある単語を ' で囲む|


ds ･･･ d(delete)s(surround)

di ･･･ d(delete)i(inside)

cs ･･･ c(change)s(surround)

ci ･･･ c(change)i(inside)

yss ･･･y(yank)s(surround)s(sentence)

ysiw ･･･ y(yank)s(surrond)iw(inner word)


## プラグイン検索

http://vimawesome.com/


## ほしいプラグイン

* yank時に内容を一時表示するプラグイン
* 置換時にundoできるプラグイン
%s:/xxx/yyy/gcで確認しながら置換する際にNで戻ることができない。

これを実現する。

* 検索時に一周した場合画面にわかりやすく大きく表示するプラグイン
* デフォルトのコマンド出力をMoreよりもわかりやすくするプラグイン。検索できたりとか
* tagsの結果をunite ->unite-tag があるがデフォルトの方が使いやすい。。。
* cscopeの結果をunite-cscopeがあるがデフォルトの方が使いやすい。。。
* tabで重複してファイルを開くとすでにあるタブが開かれる
* 検索時のハイライトをカレントウィンドウだけにする
* insert modeの入力をrepeat(".")に加えないようにする
* コマンドhistoryでwとかqとかを表示しないようにするプラグイン
これが近いが、q:で表示されてしまう。viminfoにいれないようにしたい

http://stackoverflow.com/questions/17310432/history-ignores-in-vim


## 使ってみたいプラグイン

https://github.com/tyru/operator-camelize.vim

https://github.com/vim-scripts/CCTree



kana/vim-altr.git

kana/vim-filetype-haskell.git

kana/vim-niceblock.git

kana/vim-operator-replace.git

kana/vim-operator-user.git

kana/vim-submode.git

kana/vim-textobj-entire.git

kana/vim-textobj-fold.git

kana/vim-textobj-indent.git

kana/vim-textobj-line.git

kana/vim-textobj-syntax.git

kana/vim-textobj-user.git


### [プラグイン調査](プラグイン調査.md)

## [clipboard|クリップボード連携](clipboard|クリップボード連携.md)

## リンク

### カラースキーマ

http://cocopon.me/app/vim-color-gallery/

[デフォルトでインストールされている](http://nanasi.jp/colorscheme/default_install.html)

http://vimcolorschemetest.googlecode.com/svn/html/index-c.html

わかってる人 http://cocopon.me/blog/?p=841


## 小ネタ

### 起動速度改善

`vim -X --startuptime /tmp/speedcheck.txt`


最小構成

~~~
touch vimrc
vim -u vimrc --noplugin --startuptime  /tmp/speedcheck_org.txt
~~~


sort -k3  /tmp/speedcheck.txt


### message（more-prompt）で検索できるようにする

~~~
:redir @a
:imap
:redir END
:new
:put a
~~~

http://stackoverflow.com/questions/18817614/how-do-i-change-vims-internal-pager-to-something-else


### タブのスペースを2に

~~~
:set ts=2
:set shiftwidth=2
~~~

### vim srcexpl でtagsファイルを隠しファイルにする

~~~
set tags=".tags"
"tags"->".tags"
set "tags = tags" -> set "tags = .tags"
~~~

### 画面のみ移動

~~~
^y ^e
~~~

### ハット（hat）の入力

~~~
ctrl + v + ~
~~~

### viバイナリモード

~~~
vi -b
のあと
:%!xxd
~~~


### ESC入力で日本語IME解除

viの設定ではなくmozc(google日本語入力)で対応する。

keymapのカスタマイズで新しいエントリとして「直接入力、入力文字なし」->キャンセル後IME無効化

で設定できる。


## 全部入りVimのインストール

プラグイン利用時に必要になる(+lua等)が使えるvimをインストールする


ubunut


~~~

sudo apt-get remove --purge vim vim-runtime vim-gnome vim-tiny vim-common vim-gui-common
sudo apt-get install liblua5.1-dev luajit libluajit-5.1 python-dev ruby-dev libperl-dev mercurial libncurses5-dev libgnome2-dev libgnomeui-dev libgtk2.0-dev libatk1.0-dev libbonoboui2-dev libcairo2-dev libx11-dev libxpm-dev libxt-dev

sudo mkdir /usr/include/lua5.1/include
sudo ln -s /usr/include/luajit-2.0 /usr/include/lua5.1/include

cd ~
hg clone https://code.google.com/p/vim/
cd vim/src
make distclean
./configure --with-features=huge             --enable-rubyinterp             --enable-largefile             --disable-netbeans             --enable-pythoninterp             --with-python-config-dir=/usr/lib/python2.7/config-x86_64-linux-gnu             --enable-perlinterp             --enable-luainterp             --with-luajit             --enable-gui=auto             --enable-fail-if-missing             --with-lua-prefix=/usr/include/lua5.1             --enable-cscope
make

sudo make install
~~~

Centos

perlは有効にできなかった

~~~
sudo yum remove vim-common vim-X11 vim-enhanced
sudo yum install lua-devel ruby ruby-devel python python-devel
./configure --with-features=huge --enable-rubyinterp --enable-largefile --disable-netbeans --enable-pythoninterp --enable-luainterp --enable-gui=auto --enable-fail-if-missing --enable-cscope  --with-lua-prefix=/usr  --enable-multibyte
~~~

centos7 http://www.yuuan.net/item/937

centos6.5 http://www.yuuan.net/item/821


## VimScript

|  接頭辞|  意味|
|---|---|
|g:varname|指定の変数をグローバル変数として指定|
|g:varname|指定の変数をグローバル変数として指定|
|s:varname|指定の変数をカレント・スクリプト・ファイルのローカル変数として指定|
|w:varname|指定の変数をカレント・エディター・ウィンドウのローカル変数として指定|
|t:varname|指定の変数をカレント・エディター・タブのローカル変数として指定|
|b:varname|指定の変数をカレント・エディター・バッファーのローカル変数として指定|
|l:varname|指定の変数を現行関数のローカル変数として指定|
|a:varname|指定の変数を現行関数のパラメーターとして指定|
|v:varname|指定の変数を Vim 事前定義変数として指定|


|  接頭辞|  意味|
|---|---|
|&varname|Vim オプション (定義がある場合はローカル・オプション。そうでなければグローバル・オプション)|
|&varname|Vim オプション (定義がある場合はローカル・オプション。そうでなければグローバル・オプション)|
|&l:varname|ローカル Vim オプション|
|&g:varname|グローバル Vim オプション|
|@varname|Vim レジスター|
|$varname|環境変数|

### メッセージを抑制する

silent



### リストから重複を削除する

let unduplist=filter(copy(list), 'index(list, v:val, v:key+1)==-1')


### リストから空値を削除する

let validlist=filter(copy(list), "v:val !~ '^$'")


## vimdiff

|コマンド|意味|
|---|---|
|[ c|次の違いがある場所にジャンプ diff put|
|] c|前の違いがある場所にジャンプ  diff obtain|
|do|今開いてるバッファに他のバッファの差分を取り込む :diffget と同じ|
|dp|もう一つのバッファに今開いてるバッファの差分を入れる。:diffput と同じ|

## Tips

### 関数が存在しているか判定する

`exists('*xxx')`

### オプション(set xxx)が存在しているか判定する

`echo exists('&xxx')`

シングルクオートが必要なので注意

### 16進表示にする

`00 01 02 03 04`
->
`0x00 0x01 0x02 0x03 0x04`

単語置換
`:%s/\(\w\w\)/0x\1/g`


### 3つの波括弧の意味

三重の中括弧

折りたたみのデフォルトマーカー

http://ac206223.ppp.asahi-net.or.jp/adiary/memo/adiary.cgi/hirosugu/vim%E3%81%A7%E7%95%B3%E3%81%BF%E8%BE%BC%E3%81%BF%E3%82%92%E4%BD%BF%E3%81%86


### プラグインの更新だけを行いたいエラーを確認したい

CIのときなどに使用

echo "" | v -c "q"  |& grep "Press ENTER" | wc -l


### 改行無視して検索したい

最長一致

`_,`

最短一致

`_.{-}`


http://psycho-cafe.blogspot.jp/2014/09/vim.html


### エラーメッセージを目立たせたい

hi WarningMsg ctermfg=white ctermbg=red guifg=White guibg=Red gui=None


## FAQ

### batch mode（コマンドライン）でexit code 0以外で終了させる

:cq

### 最短一致

~~~
``で囲まれた範囲最短一致
v`.{-}`

vでvery magicと扱い、{-}をしたほうがバックスラッシュで色々かわさなくていいので楽
よくある*?ではない
~~~

### 途中からのパスをgfで開けない

pathを設定する

先頭のスラッシュを除去する


https://hail2u.net/blog/software/support-slash-started-relative-url-in-vim-gf.html



### 言語ごとの基本的なvimrcがほしい

http://vim-bootstrap.com/



### 今のファイル名をインサートしたい

:put %

:r! basename %

:<C-R>=file


### 特殊キーのマッピングがうまくいかない

以下のものは同一視される


<C-i> == <Tab>

<C-m> == <Enter>

<C-[> == <ESC>



http://rbtnn.hateblo.jp/entry/2014/11/30/174749


### ウィンドウとバッファの違い

ウィンドウ：:今見えている領域。splitを含む（splitすると１つのバッファに2つのウィンドウが表示される）。qで閉じる部分。

バッファ：基本的には1つのバッファ=1つのファイルです。:buffersで確認できる。




### 置換処理中に前の単語に戻りたい

基本的にはできない。工夫するしかない。プラグインも思わしくない

http://stackoverflow.com/questions/31956485/how-can-i-switch-from-forward-to-backward-in-interactive-search-replace-regex-in


### シンタックスを再読み込みしたい

:syntax on



### E21: 'modifiable' がオフなので，変更できません

vimでQuickFixListのような特殊なウィンドウをいじろうとしたら出た


:set modifiable

:set write


### noremapとmap

noremapは他でmapを変更していても素のvimの設定をマッピングする（再割当てしない）。

mapは他で変更していた場合は変更されたあとの設定となる（再割当てする）。


素のvimのマッピングを変更したいときはnoremapを使おう！

<Plug>があるときはmapを使おう！


http://cocopon.me/blog/?p=3871

### mapによくある<C-u>
 
 範囲選択<,'>を消しているらしい
 
 https://vim-jp.org/vimdoc-ja/map.html#omap-info


### vim-clangでウィンドウが勝手に閉じる

"  au BufUnload <buffer> call <SID>DiagnosticsPreviewWindowCloseWhenLeave()

"  au BufWinLeave <buffer> call <SID>DiagnosticsPreviewWindowCloseWhenLeave()

au BufDelete <buffer> call <SID>DiagnosticsPreviewWindowCloseWhenLeave()


BufWinLeave でもBufDelete でもいけるっぽい


### デバッグログを出す

vim -V9logfile.log




### コマンドを履歴検索して実行

bashのCtrl-rみたいなことをやりたい場合


You can use q/ just like q: to view the search history.



### 補完時をキャンセルしたい

Ctrl+eを押す


恒久的には

noremap pumvisible() ? "<esc>a" : "<cr>"




### ubuntuでluaを再コンパイルせずに入れたい

sudo apt-get install vim-gnome

を入れるとCLI版も更新される


## TroubleShooting

### neovimでmarkをdelmarkしても開き直したら復活する

`wshada!`
で直る模様。。。

https://github.com/neovim/neovim/issues/4288#issuecomment-186076052


### quickrunのパッチ

~~~
91     execute sp
~~~


### quickfixstatus.vimのパッチ

~~~
execute 'echo b:qfstatus_list[ln][:' . &co . '-13]'
~~~

## リンク

[vimの使い方](http://mba-hack.blogspot.jp/2013/02/vim.html)

