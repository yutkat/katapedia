# Ruby

## Rails

### 基本理念

1. プログラマの幸福度を最適化
1. 設定より規約を重視する（CoC）
1. メニューは”おまかせ”で
1. パラダイムが1つではない
1. 美しいコードを称える
1. 統合システムを尊重する
1. 安定性より進歩を重視する
1. テントを押し上げる

http://postd.cc/rails-doctrine/



## リンク

"Ruby on Rails入門まとめ":http://techacademy.jp/magazine/5910



## gem

### gemの接続先サイト更新

sudo gem source --add http://rubygems.org/

sudo gem source --remove https://rubygems.org/

sudo gem sources list


### リポジトリにあるバージョン一覧を表示

gem list rhc --remote --all


### gemで取得したファイルすべてをアンインストール

gem uninstall --all


## bundle

### bundleで取得したファイルすべてをアンインストール

bundle cleanup --force


### bundleでNo such file to load — xxとなる

bundleを使うときはgemを使わずに、Gemfileに記載すること。





