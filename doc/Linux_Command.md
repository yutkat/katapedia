# Linux Command

## GNUの使っているコマンドオプション

https://www.gnu.org/prep/standards/html_node/Option-Table.html#Option-Table

## かっこいいコマンドラインツール

- dstat: システムモニタ。vmstatの豪華版
- slurm: ネットワークモニタ
- multitail: 複数ログ表示
- ttyrec: コマンド履歴録画
- ttygif: コマンド履歴録画をgif化。apt-getできない。githubにある。
- htop: システムモニタ。topの豪華版
- iftop: ネットワークモニタ
- siege: Web系テストツール
- tsung: Web系テストツール。siegeの高機能版
- socat:  proxy ツールである。入力と出力にファイル・標準入出力・コマンド・他のマシンなど、いろいろな種類を割り当てることができる。用途別に紹介しよう。
- netpipes: socatと似たようなもの？
- vifm: viっぽいファイラー
- sloccount: コードのライン数計測。jenkinsのプラグインがある。
- cloc: コードのライン数計測
- ipcalc: ipアドレス、ネットマスク計算
- mtr: 通信経路の継続表示。どこで、どのくらいの品質なのかまでわかる。


`sudo apt-get install dstat slurm multitail ttyrec htop iftop tsung socat vifm sloccount cloc ipcalc`

http://orebibou.com/2014/09/linux%E3%81%A7%E3%83%8D%E3%83%83%E3%83%88%E3%83%AF%E3%83%BC%E3%82%AF%E3%81%AE%E7%9B%A3%E8%A6%96%E3%82%92%E8%A1%8C%E3%81%88%E3%82%8B%E3%83%A2%E3%83%8B%E3%82%BF%E3%83%AA%E3%83%B3%E3%82%B0%E3%82%B3/

## 置き換えるべきコマンド

https://remysharp.com/2018/08/23/cli-improved


## yaourt

### md5sumをスキップする

yaourt --m-arg "--skippgpcheck" -S xxx


## あるプログラムが含まれてるパッケージを探す

sudo yum providers xxx

sudo apt-file search xxx



## OSに近いコマンド

http://postd.cc/adventures-in-usr-bin/


## curl

環境変数をデータとして送る方法



```
curl -u <my-api-token>:   -X POST https://api.pushbullet.com/v2/pushes   --header 'Content-Type: application/json'   --data-binary '{"type": "note", "title": "'"$TR_TORRENT_NAME"'",   "body": "'"$TR_TORRENT_NAME completed"'."}'
```
http://superuser.com/questions/835587/how-to-include-environment-variable-in-bash-line-curl


### wgetみたいに使う

`curl -LOJ http://xxx`


## xargs

### 2つのコマンドに引数を渡す

`find . -name *.cc | xargs -I % sh -c "echo -n '% '; nkf -g %"`

@{}@だとだめなので注意


http://stackoverflow.com/questions/6958689/xargs-with-multiple-commands-as-argument


### 変数を渡す

TEST=hallo2

echo "hallo" | xargs sh -c 'echo passed=$1 test='$TEST sh


### 変数に格納せずに`{}`で得られた値を変更する

`find . -name "*.txt" | xargs -I{} sh -c 'mv "$1" "foo/$(basename ${1%.*}).new.${1##*.}"' -- {}`

## sudo

-H：$HOMEを変身先のユーザの設定にする

-i：変身先のユーザの設定を反映する

-E：変身元のユーザの設定を反映する


## diff

### 同一ファイルでもリポートする

-s オプションを付ける


## grep

### 単語区切りで最短マッチ

`grep -P -o --color -h -I "bu._.+?b" {} *`


e.g.


`find . ( -name *.c -o -name *.cc  ) -print | xargs -i grep -P -o --color -h -I "bu._.+?b" {}  | sort | uniq`


### 単語単位で検索

`-w`


### バイナリファイルを除外

`-I`


### 改行も含めて検索

aaabbbccc

dddeeefff

ggghhhiii


`grep -Pzo 'bbb[sS]*?ddd' aaaaaa`


## tar

### ディレクトリを指定した際に配下のファイル、ディレクトリを格納しない

@--no-recursion@を付ける


## systemd

### 設定が反映されない

`systemctl daemon-reload`

を実行する


## scriptの結果をきれいに

`col -bp typescript | less -R`

`cat -v typescript | sed -e "s/x1b[.{1,5}m//g"`

`cat typescript | perl -pe 's/e([^[]]|[.*?[a-zA-Z]|].*?a)//g' | col -b > typescript-processed`


col: 無効または不完全なマルチバイトまたはワイド文字ですが出力される場合

nkf -w をかませる


## 覚えておきたいpingの使い方

http://orebibou.com/2015/04/linux%E3%81%AEping%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%E3%81%A7%E8%A6%9A%E3%81%88%E3%81%A6%E3%81%8A%E3%81%8D%E3%81%9F%E3%81%84%E4%BD%BF%E3%81%84%E6%96%B916%E5%80%8B/


## 標準出力のdiff

`diff <(command A) <(command B)`

## stdbuf

バッファリングの調整

## shuf

cutの変形版。同じ行が別々に出力される。

## findの区切り文字を切り替える

`find . type -f -print0 | xargs -0 rm`

## 圧縮・解凍

tar.gz: tar -zcvf

実行速度は速いが、圧縮率はいまいち


tar.bz2: tar -jcvf

実行速度はまあまあ、圧縮率もまあまあ

Cent4から使える


tar.xz: tar -cvf --lzma

実行速度は遅い、圧縮率は高い

Cent5から使える


## システムモニター

* メモリ監視
~~~
ps alx --sort -rss
~~~

* ディレクトリの全パーミション・所有者確認
~~~
find . -printf "%U %G %m %pn"
~~~

* ディレクトリ構成をツリー表示

teratermでも文字化けしないように


`tree -F --charset=x`



## ターミナルでユーザをグループに追加

~~~
vi /etc/group　　←　/etc/groupの編集

git:x:1001:xxx　　　 　←　管理ユーザをsvnグループへ追加
vi /etc/gshadow　←　/etc/gshadowの編集

git:!::xxx　　　　　　 ←　管理ユーザをsvnグループへ追加
再度確認。
sudo grpck　←　グループ確認
~~~

新規ログイン後有効になる


## awk

http://www.bigegg.net/post/17548414073/cat-foo-txt-awk-print


`gawk 'BEGIN{FS=",";OFS="t"} {gsub(""",""); $1=$1; print $6,$4,$1,$1$2}' test.csv`


## sloccount

### FAQ

#### ライン数が表示されない

ファイルヘッダーのコメントに`Do not edit`があると計測されない。
`--autogen`をつけると自動生成も計測してくれる


## jq

### すべてのstringの値を抜き出したい

`jq -r 'recurse(.[]?) | strings'`


## よくわからなくなるコマンド

### eval

evalは引数の変換を一度に行って実行を行う。たとえば，変数NAME1に「FUKUDA」，NAME2に「NAME1」が入っていた場合とする。この場合で，「NAME2」の変数を利用してNAME1の内容を閲覧したい場合は，使用例のようにevalを利用する。


### nohup




nohupでコマンドを実行した場合は，ログアウトしてもプログラムを実行し続ける。




