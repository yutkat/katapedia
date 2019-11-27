# Git

## はじめての

絵があって簡単で面白い

↓↓↓↓↓↓↓↓↓↓↓↓

http://kinokoru.jp/archives/1017



アニメーションで面白い

↓↓↓↓↓↓↓↓↓↓↓↓

http://k.swd.cc/learnGitBranching-ja/


コミット取り消し

http://freak-da.hatenablog.com/entry/20111105/p1


## コマンド

* commitする	バージョンを1つ進める
* stageする	特定の変更内容をindexに登録する(次回commit分に含める)
* index	次回commit分のファイル一覧

|_.コマンド|_.意味|

|git init|カレントディレクトリ以下をgit管理下に置く|

|git add <filename> |<filename>をstageする。<filename>に.(ピリオド)を指定すると変更分全て|

|git commit -m "<message>" | 1行メッセージ付きでcommitする|

|git status | stage(to be commited)と変更分(not updated)の状態を確認する|

|git reset [<target>]| <target>(git logで表示されるハッシュ文字列部分)まで戻る。指定が無ければindexを全てunstageする|

|git log [--format=oneline]|commitの履歴を見る。オプションでformatが幾つか指定出来る|

|git rm|ファイルを削除してgitに削除したことを伝える|

|git clean -fdxn ←確認 危険！必ず確認すること  実行→ git clean -fdx |管理対象外のファイルを削除する|


## 初期設定

~~~
git config --global user.name "your name"
git config --global user.email aaa@gmail.com
git config --global color.ui auto
~~~

## ヘッドの種類

* HEAD
現在作業しているワーキングツリーのコミットを指します。コミットが重なる度に先頭へ移動していきます。また git reset や git merge git rebase などのコマンドでも移動します。

* ORIG_HEAD
git reset や git merge など、通常のコミットとは違う極端な HEAD の移動が発生した時に、移動前の HEAD のコミットを指します。

* MERGE_HEAD
git merge を実行して、コンフリクトなどが発生してマージをしている最中にマージ元のブランチの先頭のコミットを指します。

* CHERRY_PIKC_HEAD
MERGE_HEAD とほぼ同じですが git cherry-pick を実行した場合のものです。

* FETCH_HEAD
git fetch した時、リモートから取得したコミットの先頭のコミットを指します。


http://tkengo.github.io/blog/2014/02/10/how-to-track-git-history/


## 違い

### resetオプションの違い

コミットメッセージだけ修正してコミットし直したい：git reset --soft

変更内容を追加してコミットし直したい：git reset --mixed

コミット自体なかったことにしたい：git reset --hard



## 世代のたどり方

* ^ について
^ を指定することで、親のコミットを取得することができます。よく聞く話かとは思いますが git のコミットには必ず 1 つは親が存在します(一番最初のコミットを除いて)。親というのは要するに直前のコミットのこと。そしてマージが発生すると、そのマージコミットについては 2 つの親を持つことになります

* ~ について
親のコミットを取得できるのは ^ と同じなのですが ~ の方はマージコミットがあっても本流の方だけをたどっていきます。


## Gitの中身

中の挙動が詳しく書いてある

http://koseki.hatenablog.com/entry/2014/04/22/inside-git-1


## コミットメッセージ規約

1. WhyやHowを書くこと。 WhereやWhatはいらん、diffを見ればわかる
1. 言語は英語にする（最初の１文は必ず英語。そのあと補足で日本語は可）
1. １文の場合にはピリオドを付けない
1. 主語は省き時制は現在の文章形式にする
1. 文頭の英単語を大文字にする
1. ファーストコミットは「Initial commit」 とする
1. issueから発生したコミットは先頭に「[refs #"issue番号"]」を記述する。クローズの場合は[close #"issue番号"]
1. 命令形とする
http://stackoverflow.com/questions/3580013/should-i-use-past-or-present-tense-in-git-commit-messages/8059167#8059167


### [コミットメッセージでよく使う英語](コミットメッセージでよく使う英語.md)


### Gitリポジトリブラウザ比較

* Gitlab
* Gitosis
* Gitolite
* Gitorious
インストールが難しいらしい

* Gitblit
インストール簡単だが、機能がまだそろっていないらしい




### Gitリモートリポジトリでgit init --bare --sharedを付け忘れた場合

~~~
git config core.sharedRepository group
chmod -R g+ws hooks
chmod -R g+ws info
chmod -R g+ws objects
chmod -R g+ws refs
~~~

で--shareと同じ効果


### ブランチの定義

issue-XXX/(大分類名)/ブランチ名で命名

issue-123/chat_timeline/extract-url_linkみたいな感じ


1. developブランチ
開発を行うためのブランチ。開発者は、主にこのブランチ上で作業を行う。次に紹介するfeatureブランチなど、他のブランチで行った作業は、ここにマージされる

1. featureブランチ
主要な機能を実装するためのブランチ。機能の実装やバグフィックスなど、タスクごとにfeatureブランチを作成し、作業を行う

1. releaseブランチ
リリースの準備を行うためのブランチ。プロダクトをリリースする前に、このブランチを作成し、微調整を行う。releaseブランチを作成することで、リリース準備と次のバージョンに向けた開発のコードを分けることができる

1. masterブランチ
リリースしたソースコードを管理するためのブランチ。リリース作業を行うと、releaseブランチはmasterブランチへマージされて、リリースタグが打たれる。開発者は、このブランチへのコミットは行わない

1. hotfixブランチ
リリースされたソフトウェアに緊急の修正を行うためのブランチ。このブランチでの修正内容は、すぐにリリースされるので、hotfixブランチはリリースを管理するmasterブランチへマージされる


## ルール

### 原則

* masterで作業しない。ブランチを作って作業すること
* ブランチでは1機能もしくは1バグのみ作業すること
* メジャーバージョンは常に動くようにしておくこと
* チケットとリンクをとれるようにすること

### [Gitワークフロー](Gitワークフロー.md)




## リポジトリ名規約

* 基本すべて小文字
* "_"でなく"-"の方がよい（使っている人が多いかつかっこいい）

## 便利なコマンドとオプション

### リポジトリ名取得

`basename $(git remote show origin -n | grep "Fetch URL:" | sed 's/.*://;s/.git$//'`


https://stackoverflow.com/questions/15715825/how-do-you-get-git-repos-name-in-some-git-repository


### ブランチ表示ログ

`git log --graph --branches --pretty=format:"%d [%h] "%s""`


## 統計情報

### 変更行数

~~~
git diff <branch name> --shortstat *.cc

git log --author="Taro Tanaka" --oneline --shortstat

git log --numstat --pretty="%H" | awk 'NF==3 {plus+=$1; minus+=$2} END {printf("+%d, -%dn", plus, minus)}'
~~~

### 総追加行数

~~~
git log --since=2013-01-01 --until=2013-06-30 --oneline --numstat --no-merges --pretty=format:"" | cut -f1 | awk 'BEGIN {sum=0} {sum+=$1} END {print sum}'
git log -- <branch name> --oneline --numstat --no-merges --pretty = format: "" | cut -f1 | awk 'BEGIN {sum=0} {sum+=$1} END {print sum}'
git log --since=2015-09-01  --oneline --no-merges --numstat --pretty=""  *.cc *.h  | cut -f1  | awk 'BEGIN {sum=0} {sum+=$1} END {print sum}'
~~~

### 総削除行数

~~~
git log --since = 2013-01-01 --until = 2013-06-30 --oneline --numstat --no-merges --pretty = format: "" | cut -f2 | awk 'BEGIN {sum=0} {sum+=$1} END {print sum}'
~~~


### コントリビューター一覧

git shortlog -se | awk -F't' '{print $2,$3}'


### 総コミット回数

git log --since = 2013-01-01 --until = 2013-06-30 --oneline --no-merges | wc -l


### よくマージしている人

git log --merges --format="%cn" | sort | uniq -c | sort -r | head


### よく編集しているファイル

git log --name-only --pretty="format:" | grep -ve '^$' | sort | uniq -c | sort -r | head


### よくfu*kって書く人

git log --pretty="format:%cn:%s" | grep fu.k | cut -d":" -f1 | sort | uniq -c | sort -r


## submodule

### 一括更新

git submodule foreach --recursive 'git checkout master; git pull'


### ルートリポジトリからsubmoduleを更新してpushしたい

`git submodule foreach --recursive git add -A .`

`git submodule foreach --recursive git commit -m 'submodule commit message'`


`git push --recurse-submodules=on-demand`


孫もいるなら

`git submodule foreach --recursive  git push origin master`

じゃないとだめっぽい


https://ja.stackoverflow.com/questions/17501/git-submodule%E3%82%92%E8%A6%AA%E3%81%AE%E3%83%AA%E3%83%9D%E3%82%B8%E3%83%88%E3%83%AA%E3%81%A8%E5%90%8C%E6%99%82%E3%81%AB%E3%82%B3%E3%83%9F%E3%83%83%E3%83%88-%E3%83%97%E3%83%83%E3%82%B7%E3%83%A5%E3%81%97%E3%81%9F%E3%81%84



2017 Q3にリリースされる予定のgitで孫がいてもpushできるようになる見込み

https://stackoverflow.com/questions/29564257/git-push-recurse-submodules-on-demand-is-not-truly-recursive


### submoduleの削除

~~~
git submodule deinit vendor/pelican-sober
git rm vendor/pelican-sober
rm -rf .git/modules/path/to/submodule
git commit -a
~~~

### 履歴を保持したままリポジトリを統合する

~~~
# Fetch the submodule commits into the main repository
git remote add submodule_origin git://url/to/submodule/origin
git fetch submodule_origin

# Start a fake merge (won't change any files, won't commit anything)
git merge --allow-unrelated-histories -s ours --no-commit submodule_origin/master

# ここでgit log しても表示されない

# Do the same as in the first solution
git rm --cached submodule_path # delete reference to submodule HEAD
git rm .gitmodules             # if you have more than one submodules,
# 他のsubmoduleがある場合はviで対象部分のみ削除
# you need to edit this file instead of deleting!
rm -rf submodule_path/.git     # make sure you have backup!!
git add submodule_path         # will add files instead of commit reference


# Commit and cleanup
git commit -m "removed submodule"
# ここでgit log したら表示される
git remote rm submodule_origin
~~~

https://stackoverflow.com/questions/1759587/un-submodule-a-git-submodule




## Tips

### gitconfigにユーザー情報を入れたくない

GIT_AUTHOR_NAME=""
GIT_AUTHOR_EMAIL=""

を定義しておけばよい

### 一部のファイルのみをstashする

stashしないファイルをgit addする
`git stash -k`
`git reset` # ステージからもとに戻す


### ノンパスワード設定

ssh-keygen -t rsa -C "your.email@example.com" -b 4096

cat ~/.ssh/id_rsa.pub

確認

ssh -T git@example.com


あとは、

https:// を git@github.com:に切り替える



これを使えばhttpsをsshに置き換えてくれる

`git config --global "url.git@github.com:.pushinsteadof" "https://github.com/"`



### push先とfetch先を変える


`git remote set-url --push origin git@github.com:User/forked.git`


`pushurl = http://192.168.1.20/Test/hubot-rocketchat.git`


http://sleepycoders.blogspot.jp/2012/05/different-git-push-pullfetch-urls.html

http://stackoverflow.com/questions/948354/default-behavior-of-git-push-without-a-branch-specified




### 複数のコミットをまとめる

git rebase -i xxx

pick->s


http://iwb.jp/git-commit-rebase-squash/


## Troubleshooting

### パスワードを入力するときに ( gnome-ssh-askpass:11826 ) : Gtk-WARNING **: cannot open display: とか言われる

unset SSH_ASKPASS



### 間違ってgit resetしてしまった

* コミットしているなら
git reflog

git rest --hard xxx


* ステージングしている
git fsck --cache --unreachable $(git for-each-ref --format="%(objectname)")

git show xxx

git fsck --cache --unreachable $(git for-each-ref --format="%(objectname)") xxx

http://stackoverflow.com/questions/7374069/undo-git-reset-hard-with-uncommitted-files-in-the-staging-area


* ステージングしていない
諦める


### EUCでコミットメッセージを書いてしまった

`git filter-branch ~~f --msg-filter 'nkf -w' -~~ --all`


http://hiroom2.jimdo.com/2015/07/09/git-filter-branch%E3%81%A7%E3%82%B3%E3%83%9F%E3%83%83%E3%83%88%E3%82%92%E4%B8%80%E6%8B%AC%E5%A4%89%E6%8F%9B%E3%81%99%E3%82%8B/


### gitignoreでホワイトリスト化できない

階層構造になっていると工夫しないと認識されない


~~~
/*
/.*
/.*/*
/.*/*/*
~~~

という感じに階層分だけ最初に除外する。


ディレクトリがある場合は

/*

!/src/

/src/*

!/src/hello.c

というように上のディレクトリを一回対象外にして入れ直す


http://seesaawiki.jp/aki/d/.gitignore%20%A5%D5%A5%A1%A5%A4%A5%EB%A4%CE%BD%F1%A4%AD%CA%FD


### .gitignoreが反映されない

git rm -r --cached .

git add .

git commit -m "Update .gitignore"


### でかいファイルをコミットしてしまった場合

重いファイルをコミットした場合、ツリーから消さない限りリポジトリが重くなってしまう。


~~~
git filter-branch --force --index-filter  'git rm --cached --ignore-unmatch でかいファイル.tar.gz'  --prune-empty --tag-name-filter cat -- --all
~~~

これでツリー上から完全削除される


http://www.walbrix.com/jp/blog/2013-10-github-large-files.html

"軽量化の仕方":http://techracho.bpsinc.jp/baba/2012_05_22/5594


### でかくなったリポジトリの原因を特定する

これが一番良さそう


~~~
#my problem was that I had there a refs/remotes/origin/master line for a remote repository, delete it, otherwise git won't remove those files
check .git/packed-refs
#(optional)to check for the largest files
git verify-pack -v .git/objects/pack/#{pack-name}.idx | sort -k 3 -n | tail -5
#(optional)to check what are those files
git rev-list --objects --all | grep a0d770a97ff0fac0be1d777b32cc67fe69eb9a98
#to remove a file from all revisions
git filter-branch --index-filter 'git rm --cached --ignore-unmatch file_names' -- --all
#to remove git's backup
rm -rf .git/refs/original/
rm -rf .git/logs/
git reflog expire --expire=now --all
git gc --prune=now
git gc --aggressive --prune=now

# old
#to expire all the loose objects
#git reflog expire --all --expire='0 days'
#to check if there are any loose objects
#git fsck --full --unreachable
#repacking
#git repack -A -d
#to finally remove those objects
#git prune
~~~

リモートリポジトリにあげたら容量がもとに戻ってしまうので、リモートリポジトリを一旦削除して再度pushする必要がある。



http://stackoverflow.com/questions/2164581/remove-file-from-git-repository-history


[gitのリポジトリがでかくなったときの削減の昔のやり方](gitのリポジトリがでかくなったときの削減の昔のやり方.md)


### 一部のファイルだけ違うブランチのものを使う

`git checkout ブランチ名 ファイル名`


### コミットするユーザやメールアドレスを間違えた場合

コミット時のユーザやメールアドレスを変更するとき


git commit --amend --author="sea_mountain <dummy_email_address@example.com>"


### 結構前のコミットするユーザやメールアドレスを間違えた場合

$ git rebase -i <コミット>

1. エディタが開くので以下のように変更して保存
1. (変更後)対象のコミットをeditに変更
$ git commit --amend -m "コミットメッセージ" --author="user.name <user.email>"

$ git log

$ git rebase --continue


### 管理下のファイルの行末のスペース（trailing whitespace）を削除する

`git ls-files | sed -i 's/[ t]*$//'`


### refspecやremoteやrefsやらがわからなくなったとき

以下のサイトにわかりやすく解説されている。

http://d.hatena.ne.jp/hokaccha/20120404/1333507076


公式文書は難解なので。。。



### リモートリポジトリのコミットログを修正する

1. git checkout -b ローカルのブランチ名 origin/リモートのブランチ名 でブランチを切ります。
1. git rebase -iします。
1. git push origin +ローカルのブランチ名:リモートのブランチ名 して強制的にコミットツリーを変更します。
他のリポジトリから変更を取得する際は、git fetchしてgit rebase origin/リモートのブランチ名 としましょう。


### コミット日付をタイムスタンプに復元

~~~
for FILE in `git ls-files`; do
TIME=`git log --pretty=format:%ci -n1 $FILE`
echo $TIME't'$FILE
STAMP=`date -d "$TIME" +"%y%m%d%H%M.%S"`
touch -t $STAMP $FILE
done
~~~

### Gitのルートディレクトリに移動する

cd "$(git rev-parse --show-toplevel)"


### commitの際にオーナー情報を入れていなかった

~~~
git config --global user.name "your name"
git config --global user.email aaa@gmail.com
git commit --amend --reset-author
~~~

OR


.gitconfig


~~~
[user]
name = name
email = mail@gmail.com
~~~

### Gitでプロキシを通すURLと通さないURLを使い分ける

どうやらversion:1.8ではダメのよう


~/.gitconfig


~~~
[http "http://192.168.1.30"]
postBuffer = 524288000
proxy =
[http "http://192.168.1.40"]
postBuffer = 524288000
proxy =
[http "http://192.168.1.50"]
postBuffer = 524288000
proxy =
#[http]
#    postBuffer = 524288000
#    proxy = http://192.168.1.30:10080
#[https]
#    postBuffer = 524288000
#    proxy = http://192.168.1.30:10080
[url "https://"]
insteadOf = git://
~~~

## Trivia

### objectsディレクトリの中

idxファイル：インデックスを保持

pack ファイル：データ実体



### おもしろいinitial commit

This is where it all begins...

Commit committed

Version control is awful

COMMIT ALL THE FILES!

The same thing we do every night, Pinky - try to take over the world!

Lock S-foils in attack position

This commit is a lie

I'll explain when you're older!

Here be Dragons

Reinventing the wheel. Again.

This is not the commit message you are looking for

Batman! (this commit has no parents)


## FAQ

### git@github.com形式でダウンロードできない場合

~/.gitconfig

~~~
[url "http://github.com/"]
insteadOf = git@github.com:
~~~


### [Gitで大量のファイルの中から必要ファイルのみをaddする方法](Gitで大量のファイルの中から必要ファイルのみをaddする方法.md)

### [CentOS6.x系でhttp認証に失敗する](CentOS6.x系でhttp認証に失敗する.md)

### [GitのGUI比較](GitのGUI比較.md)

### 現在のディレクトリでgit clone

`git clone http://xxx .`


※古いバージョンだとできない？v1.8系は無理だった。


### .gitignoreに設定しているファイルがリモートに存在するエラーの解決法

エラー文：

error:The following untracked workding tree files would be overwritten by merge:


解決法：


ようわからんけど、gitでリモートのブランチにローカルを強制一致させたい時



git fetch

git reset --hard FETCH_HEAD

