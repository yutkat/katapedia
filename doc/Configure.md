# Configure

## ファイル生成フロー

![](/img/follow.png)

### 配布

autotools を利用したソースコードを配布するときには configure.scan、autoscan.log、configure.ac などの中間生成ファイルは不要。


## FAQ

#### NEWS README AUTHORS ChangeLogがいらない

Makefile.amに

AUTOMAKE_OPTIONS = foreign

を書く


## 参考リンク

"autoreconfの使い方":http://d.hatena.ne.jp/tomohikoseven/20120811/1344650299

"configure.inの各パラメータ説明":http://www.spa.is.uec.ac.jp/~kinuko/slidemaker/autotools/#slide5

http://transitive.info/2012/07/21/autotools-tutorial2-automake/

[GNUビルドのやり方](http://cutter.sourceforge.net/reference/ja/tutorial.html)

"はじめてのautotools":http://transitive.info/2012/07/21/autotools-tutorial2-automake/

