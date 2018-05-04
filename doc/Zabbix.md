# Zabbix


### 初期構築参考ページ

http://shobon.hatenablog.com/entry/2012/07/09/223100


## バックアップ・リストア

### バックアップ

`sudo docker exec -ti zabbix_zabbix-db_1 /zabbix-backup/zabbix-mariadb-dump -u zabbix -p my_password -o /backups`


### リストア

`sudo docker exec -it zabbix_zabbix-db_1 /bin/bash`

`mysql  -u zabbix -p`

`drop database zabbix;`

`create database zabbix;`

`grant all privileges on zabbix.* to zabbix@localhost ;`

`q;`

`mysql  -u zabbix -p my_password zabbix < /backups/20171011-1356_db-3.0.0.sql`

