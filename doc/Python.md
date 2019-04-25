# Python

## 中級者向けの読み物

https://python.ms/

## awesome python

https://github.com/vinta/awesome-python
https://qiita.com/hatai/items/34c91d4ee0b54bd7cb8b


## ボイラープレート

https://github.com/ikasat/python-boilerplate/tree/v2.0.0
https://techblog.asahi-net.co.jp/entry/2018/11/19/103455
これがまとまっててよい。

setup.pyはsetup.cfgに記載するのでよいと思う。

setup.pyはこれだけでいい

```python
from setuptools import setup

setup()
```


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
| パッケージ(ディレクトリ名)      | 全小文字 なるべく短くアンダースコア非推奨    | tqdm, requests ...  |
| モジュール      | 全小文字 なるべく短くアンダースコア非推奨    | sys, os,...         |
| クラス          | 最初大文字 + 大文字区切り| MyFavoriteClass     |
| 例外            | 最初大文字 + 大文字区切り| MyFuckingException  |
| 型変数         | 最初大文字 + 大文字区切り | MyFavoriteType      |
| メソッド       | 全小文字 + アンダースコア区切り| my_favorite_method  |
| 関数           | 全小文字 + アンダースコア区切り| my_favorite_funcion |
| 変数           | 全小文字 + アンダースコア区切り| my_favorite_instance|
| 定数           | **全大文字** + アンダースコア区切り| MY_FAVORITE_CONST   |

https://qiita.com/naomi7325/items/4eb1d2a40277361e898b

ディレクトリとファイルの名前に複数形は使わない

https://pypyja.readthedocs.io/en/latest/coding-guide.html#id30


### import

つべこべ言わず絶対パスで書くべき


## ロギング

```python
from logging import getLogger, StreamHandler, DEBUG
logger = getLogger(__name__)
logger.debug('hello')
```


https://qiita.com/amedama/items/b856b2f30c2f38665701



## setup.cfg

### 各項目の説明

https://python-packaging-user-guide-ja.readthedocs.io/ja/latest/distributing.html#setup-cfg

### find_packageする

```
[options]
packages = find:
```

## CI

### CIでやるべきこと

```
 - pipenv run vet
 - pipenv run pytest
 - pipenv run pytest -v --cov=operation_reporter
```

## Tips

### 文字コードとasciiの変換

word -> ascii

`hex(ord('r'))`

ascii -> word

`chr(0x72)`




