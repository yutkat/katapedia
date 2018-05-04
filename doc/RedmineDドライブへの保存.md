# RedmineDドライブへの保存


以下も同様ですが、ファイルを編集するエディタは管理者権限で実行して変更してください。(私は普通にファイルを編集し、設定が変わらず、かなりあせりました。)


### ■ 添付ファイルの保存先の変更

レッドマインでアップロードされるファイルは、以下のフォルダに保存されています。apps/redmine/htdoc/files

このfilesフォルダを任意の場所に移動します。


そして、移動先の情報を変更します。

apps/redmine/htdocs/config/configuration.yml

(configuration.yml.exampleをconfiguration.ymlにリネームします。)


「   attachments_storage_path: D:/ICF_AutoCapsule_disabled/RedmineData/files」



file : configuration.yml

~~~
# Absolute path to the directory where attachments are stored.
# The default is the 'files' directory in your Redmine instance.
# Your Redmine instance needs to have write permission on this
# directory.
# Examples:
# attachments_storage_path: /var/redmine/files
# attachments_storage_path: D:/redmine/files
attachments_storage_path: ←この行を上の例を参考に任意のフォルダに変更します。
~~~
この項目以外の、その他の変数は、特に削除しなくても問題なさそうです。



### ■ MySQLの保存フォルダの変更

インストールしたフォルダに作成されるmysqlフォルダ内にあるdataフォルダを、任意の場所に移動させます。


そして、移動先の情報をmysqlフォルダ内にあるmy.iniを編集して変更します。


file : my.ini

~~~
# set basedir to your installation path
basedir=C:/PROGRA~1/BITNAM~1/mysql
# set datadir to the location of your data directory
datadir=C:/PROGRA~1/BITNAM~1/mysqldata ←ここを任意の場所に変更します。
~~~

「datadir=D:/ICF_AutoCapsule_disabled/RedmineData/mysqldata」



### ■ ベースの設定変更

インストールフォルダに作成されるproperties.iniを変更します。


file : properties.ini

~~~
[MySQL]
mysql_port=3306
mysql_root_directory=C:Program FilesBitNami Redmine Stack/mysql
mysql_binary_directory=C:/Program Files/BitNami Redmine Stack/mysql/bin
mysql_data_directory=C:Program FilesBitNami Redmine Stack/mysql/data
↑この変数をMySQLのデータ保存先に設定したフォルダのパスに変更します。
mysql_arguments=-u root
mysql_unique_service_name=redmineMySQL

mysql_data_directory=C:ICF_AutoCapsule_disabledProgramRedmine/mysql/data
↓
「mysql_data_directory=D:/ICF_AutoCapsule_disabled/RedmineData/mysqldata」
~~~

※※※※※※※※※※※※※※※※※※※※※※※※※※

データベースはインポートした場合、パスワードが違うため、

ローカルでインポートしたDBを持ってくること

※※※※※※※※※※※※※※※※※※※※※※※※※※


PCを再起動すれば、保存先が新しいものに変更されます。

またバックアップする場合は、dataフォルダとfilesフォルダをそのまま保存するだけなので、簡単です。



