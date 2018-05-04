# sonarqube

## TroubleShooting

### Date of analysis cannot be older than the date of the last known analysis on this project. と表示され

タイムゾーンが一致していないことが原因だった。

1. 環境変数を設定する`TZ=Asia/Tokyo`(データベースサーバが別マシンならそちらにも設定する)
2. sonarqubeを実行する際に`-Duser.timezone=Asia/Tokyo`を設定する
