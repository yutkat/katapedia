# GitHub

## GitHubの人気ランキング

https://octoverse.github.com/

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

### ラベル

- `(╯°□°)╯︵ ┻━┻` https://github.com/ryanoasis/vim-devicons/labels/%28%E2%95%AF%C2%B0%E2%96%A1%C2%B0%29%E2%95%AF%EF%B8%B5%20%E2%94%BB%E2%94%81%E2%94%BB


## Pull Request(プルリクエスト)

### 書くべき内容

- 目的
- 達成条件
- 実装の概要
- レビューして欲しいところ
    - ここはこうしたけどこの点で問題はないだろうか
    - このあたりいじってるけど特に悪いことはないか
 - 不安に思っていること
    - 何をレビューしてほしいかを書こう
    - 設計や企画のレビュー
- 保留してること
    - このプルリクでしないこと
    - レビューの範囲を絞れる
- スケジュール
    - マージすべき日、リリースすべき日の指定があれば書く
- 関連
    - 関係するプルリクエスト

https://qiita.com/ikuwow/items/fb52a54c086398eb5b92


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

