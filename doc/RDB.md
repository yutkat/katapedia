# RDB

## 用語

DDL：Data Definition Language データ定義言語。データを格納するための構造を定義する。

DML：Data Manipulation Language データ操作言語。定義されたデータ構造中の個々のデータを操作する。

DCL：Data Control Language データ制御言語。データへのアクセス権限などを制御する。



## 命名規則

命名規則はスネークケース(小文字で単語を_区切りにしたもの)を採用しているパターンが多い

カラム名もスネークケースを採用したほうが命名規則が統一されて美しい


* テーブル編
複数形

tlm_values


基本的にはRailsの命名規則に従うのがよいだろう


http://railsdoc.com/rails_base


## SQLite

### 使い方

http://so-zou.jp/web-app/tech/database/sqlite/


#### よく使うコマンド

.databases

.table

.dump ?TABLE?

.help

.exit


#### 利用可能な型

http://so-zou.jp/web-app/tech/database/sqlite/data/data-type.htm


## PostgreSQL

### よく使うSQL

#### CSVにする

`COPY (select * from foo) TO '/tmp/aaa.csv' (FORMAT csv);`

#### バイナリをlike検索

`select * from binary where data::varchar like '%010203%';`

#### Byteaを挿入

普通にinsertすればよい

```
INSERT INTO table (data) VALUES (
\x13070809'
);
```

#### Select結果をinsert

```
INSERT INTO table(A, B, C, time)
SELECT
0,
'aaa',
COL_C,
lag(time) OVER (ORDER BY time asc),
FROM
table2
```

#### ページャーを変更

シェルで

`export PAGER=less`

#### ページャーを使わない

\pset pager off


#### キャラクタ型を整数に変換する方法

~~~
ALTER TABLE cps_def ALTER COLUMN no_imaging_flag TYPE integer USING (no_imaging_flag::integer);
~~~

### DB内全消去

`drop schema public cascade; create schema public;`


### Tips

### datetimeをtimeにキャストする方法

cast("start_time" as time)

### 特定のカラムを対象として重複行を削除したい

https://stackoverflow.com/questions/9795660/postgresql-distinct-on-with-different-order-by

#### enumにコメントをつけたい

各項目にはつけられないみたいなのでenum自体に列挙する
`CREATE TYPE my_type_description AS ENUM('foo desc', 'bar desc');`

https://stackoverflow.com/questions/42367917/postgres-add-description-of-an-enum-value

### ひとつ前のレコードの前回値を取得する

https://blog-asnpce.com/technology/1345

### ORDER BYでnull値を除外する

`WHERE target_column IS NOT NULL ORDER BY target_column`

target_columnに別名をつけているの場合はWHEREできないので

`ORDER BY target_column desc NULLS LAST`


## MySQL

### TroubleShooting

#### ActiveRecord::StatementInvalid (Mysql2::Error: Incorrect string value:というエラーが出る

文字コードが原因。

`show table status;`

をして、

latin1_swedish_ci とかを探す。

あったら、

`ALTER TABLE my_table CONVERT TO CHARACTER SET utf8;`

をしてUTF-8にする。

