# Webブラウジング

## 高速Webブラウジングテクニック

### googleのテクニック

googleではショートカットが効く


[検索後]tab : 次の検索項目（矢印がつく）

[検索後]shift+tab : 前の検索項目

何らかの文字を入力（個人的には/が好き） : 検索ボックスへ移動


#### google言語指定検索

&hl=en&gl=US

&hl=ja&gl=JP



### ブラウザ共通

リンクでホイールクリック : 新しいタブで開く

タブでホイールクリック : そのタブを閉じる


### chromeのテクニック

右隣りのタブに移動 : ctrl+tab

左隣りのタブに移動 : shift+ctrl+tab

タブを閉じる : ctrl+w

アドレスバーに移動 : F6,ctrl+l,alt+d

検索ボックスに移動：Esc



## TroubleShooting

### google翻訳でQiitaが見れなくなった

CAPTCHA認証がかかっている可能性あり

ctrl+shift+jでデバッグ



### GoogleChromeでgoogle検索が遅い

たぶんIPV6が原因かと

起動オプションに@--disable-ipv6@を付けて起動して試してみる。


### chromeでfont-familyのフォントが適用されないようにする

エクステンションを使う Stylist

http://assimane.blog.so-net.ne.jp/2014-05-11



↓はだめだった。


xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx


強制的にfont-familyを上書きする

C:Users"ユーザ名"AppDataLocalGoogleChromeUser DataDefaultUser StyleSheets

Custom.css


~~~
*{
font-family: "Noto Sans CJK JP Light";
}
~~~

xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx


## 参考

[GoogleChrome起動オプション](http://chrome.half-moon.org/43.html)

