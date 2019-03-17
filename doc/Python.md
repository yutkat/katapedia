# Python

## awesome python

https://github.com/vinta/awesome-python
https://qiita.com/hatai/items/34c91d4ee0b54bd7cb8b

## お作法

### 先頭行に書くこと

#! /usr/bin/env python

1. ~~*~~ coding: utf-8 ~~*~~



### if __name__ == "__main__":の意味

直接実行されたらという意味

importの時には動作しないようにする

### 命名規則

| 対象              |ルール                                  | 例                   |
|:----------------:|:--------------------------------------:|:--------------------:|
| パッケージ      | 全小文字 なるべく短くアンダースコア非推奨    | tqdm, requests ...  |
| モジュール      | 全小文字 なるべく短くアンダースコア非推奨    | sys, os,...         |
| クラス          | 最初大文字 + 大文字区切り| MyFavoriteClass     |
| 例外            | 最初大文字 + 大文字区切り| MyFuckingException  |
| 型変数         | 最初大文字 + 大文字区切り | MyFavoriteType      |
| メソッド       | 全小文字 + アンダースコア区切り| my_favorite_method  |
| 関数           | 全小文字 + アンダースコア区切り| my_favorite_funcion |
| 変数           | 全小文字 + アンダースコア区切り| my_favorite_instance|
| 定数           | **全大文字** + アンダースコア区切り| MY_FAVORITE_CONST   |

https://qiita.com/naomi7325/items/4eb1d2a40277361e898b

ディレクトリ/モジュール/名前空間はすべて 小文字
ディレクトリとファイルの名前に複数形は使わない

https://pypyja.readthedocs.io/en/latest/coding-guide.html#id30
