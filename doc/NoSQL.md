# NoSQL

## MongoDB

### backup

1. mongod.confのパスを調べる
`ps aux | grep mongod`

1. mongodを停止する
~~~
mongo
> use admin
> db.shutdownServer()
> quit()
~~~
1. バックアップしたデータの格納用に、空ディレクトリを作る
`mkdir ~/mongo_dump`

1. データをバックアップする
`mongodump -v --db [database_name] --out ~/mongo_dump`

1. mongorestoreでリストアする
`service mongod start`

1. データをリストアする
`mongorestore -v --db [database_name] ~/mongo_dump/[database_name]`



### TroubleShooting

#### mongodbの単独起動

`mongod --dbpath .`

