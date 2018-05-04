# GitHub

## 人気のある・参考になるリポジトリ

https://github.com/docker/docker

https://github.com/takezoe/gitbucket

https://github.com/rails/rails

https://github.com/jquery/jquery

https://github.com/h5bp/html5-boilerplate

https://github.com/atom/atom

https://github.com/robbyrussell/oh-my-zsh

https://github.com/adobe

https://github.com/google


GitHubの人気のあるリポジトリ検索

http://otatin.com/widget/hub25/


## Issue

### デフォルト設定

duplicate 重複,複製

bug バグ

enhancement 拡張

invalid 無効

question 質問

wontfix "won't fix"(報告された内容は認識しているが)修正するつもりは無い 修正しない。前向きな理由

backlog 保留。何かあったらさらう。

rejection 拒否

worksforme 報告者の勘違いなどで登録された


https://github.com/vim-jp/issues/wiki/Member


## Milestones

### デフォルト設定

next

priority

future


## バッジ

Travis CI：CIサービス。オープンソース向けには無料で提供。.travis.ymlを書いてGitHubにpushすれば自動でテストを実行してくれる。もう便利すぎてわけがわかりません。

Coveralls：カバレッジ計測

Code Climate：コードの品質を計測してくれるサービス

gitter:チャット

David:モジュールの依存関係

LICENSE：ライセンス情報

coderwall:活動に応じてバッジがもらえる

http://shields.io/:オリジナルバッジ


## Tips

### Pull Requestをfetchする

.git/configに

`fetch = +refs/pull/*/head:refs/remotes/origin/pr/*`

を記述する

