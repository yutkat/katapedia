# Haroopad

### クソみたいなフォントを変更する方法

**エディタ**


ファイル > 設定 > エディタ > defaultをEdit


~~~
editor {
font-family: 'Lucida Grande','Hiragino Kaku Gothic ProN', 'ヒラギノ角ゴ ProN W3',Meiryo,メイリオ,sans-serif;
}
~~~

**ビューア**


ファイル > 設定 > ビューワ > defaultをEdit


~~~
* {
font-family: 'Lucida Grande','Hiragino Kaku Gothic ProN', 'ヒラギノ角ゴ ProN W3',Meiryo,メイリオ,sans-serif;
}

code {
font-family: 'Bitstream Vera Sans Mono', Courier, monospace;
font-weight: normal;
}
~~~


http://www.hrkd.net/2014/04/23/id_1458/



注）ビューアの方に.userStyle.cssがあるとうまく変更されない

