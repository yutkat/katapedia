# Prometheus


# Loki

Prometheusはアプリケーションやシステムのログを監視できないためこちらを使う。Promtailというアプリでログを収集する

## 設定

### 監視するファイルを追加する

-config.file=/etc/promtail/docker-config.yaml
で指定しているファイルに

``` yaml
scrape_configs:
- job_name: system
  static_configs:
  - targets:
      - localhost
    labels:
      job: varlogs
      __path__: /var/log/*log

- job_name: app
  static_configs:
  - targets:
      - localhost
    labels:
      job: applogs
      __path__: /var/log/app/**/*log
```

### LokiとPromtailを別サーバーにする

-config.file=/etc/promtail/docker-config.yaml
で指定しているファイルに

``` yaml
clients:
  - url: http://192.168.1.100:3100/api/prom/push # loki address
```

