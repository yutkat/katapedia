# R言語

## 基本

### 要素へのアクセス

c(2,4,6,8,10)とした後、

x[3]

とすると、3 番目の数値 6 を抽出することが可能です。

x[c(2,4)]

で２番目と 4 番目の数値を抽出でき、また、

x[2:4]

とすれば、2 番目から 4 番目までの数値を抽出することが可能です。





## 仕様

### for文を使わない

R言語ではapplyを使って処理する


http://d.hatena.ne.jp/a_bicky/20120425/1335312593


## Tips

### colSdを使いたい

`colSd <- function (x, na.rm=TRUE) apply(X=x, MARGIN=2, FUN=sd, na.rm=na.rm)`



http://stackoverflow.com/questions/17549762/is-there-such-colsd-in-r




### 複数のファイルをマージする

~~~
# 複数のデータファイルを一括してリストに読み込む
fnames <- dir(pattern=".csv") # とすると、作業ディレクトリ内のcsvファイルのみを選んでくれる
# fnamesp <- choose.files() # ファイルを選ぶ。パスのベクトルが保存される。リストの要素名には使いづらい
csvlist <- lapply(fnames, read.csv)
names(csvlist) <- fnames
csvlist

# 読み込んだcsvをマージする
n <- length(csvlist)
temp <- csvlist[1](1.md)
for (i in 2:n) {
temp <- merge(temp, csvlist[i](i), all=T.md)
}
res <- temp
res

~~~



