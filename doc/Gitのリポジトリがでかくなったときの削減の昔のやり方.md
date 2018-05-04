# Gitのリポジトリがでかくなったときの削減の昔のやり方


#### 方法1

`git verify-pack -v .git/objects/pack/pack-*.idx |sort -k 3 -n |tail -100 `

`git rev-list --objects --all | grep xxx`

`git gc --prune=now --aggressive`


http://techracho.bpsinc.jp/baba/2012_05_22/5594

https://git-scm.com/book/ja/v1/Git%E3%81%AE%E5%86%85%E5%81%B4-%E3%83%A1%E3%82%A4%E3%83%B3%E3%83%86%E3%83%8A%E3%83%B3%E3%82%B9%E3%81%A8%E3%83%87%E3%83%BC%E3%82%BF%E3%83%AA%E3%82%AB%E3%83%90%E3%83%AA


#### 方法2

調べる


~~~
git rev-list --all --objects |     sed -n $(git rev-list --objects --all |     cut -f1 -d' ' |     git cat-file --batch-check |     grep blob |     sort -n -k 3 |     tail -n40 |     while read hash type size; do
echo -n "-e s/$hash/$size/p ";
done) |     sort -n -k1
~~~

削除する


~~~
git filter-branch -f  --index-filter 'git rm --force --cached --ignore-unmatch video/parasite-intro.avi'  -- --all
rm -Rf .git/refs/original
rm -Rf .git/logs/
git reflog expire --expire=now --all
git gc --prune=now
git gc --aggressive --prune=now
~~~

確認


~~~
git count-objects -v
~~~


http://stackoverflow.com/questions/1029969/why-is-my-git-repository-so-big

