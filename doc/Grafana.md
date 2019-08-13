# Grafana

## 設定

### 設定ファイルをGitで管理する方法

https://grafana.com/docs/administration/provisioning/

#### Datasources

```
mkdir -p datasources && curl -s "http://localhost:3000/api/datasources"  -u admin:admin|jq -c -M '.[]'|split -l 1 - datasources/
```

yqなどでyamlに変換して
/etc/grafana/provisioning/datasourcesに格納する

## TroubleShooting

### lokiをクエリして

なぜか現状(2019/08/13)できない模様
https://github.com/grafana/grafana/issues/14576
https://github.com/grafana/grafana/issues/14860

がんばればpromtailをPrometheusに飛ばす方法でできる模様。
https://github.com/grafana/loki/issues/340

