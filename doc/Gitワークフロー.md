# Gitワークフロー


* 準備
ローカルのmasterに移動する

$ git checkout master

ローカルのmasterをリモートと同期する

$ git pull origin/master

masterから、作業用のブランチを作成する。

$ git checkout -b branchname master

ブランチ名は担当者名と作業名をスラッシュで結合したものとする。

例： taro/featurename, taro/bugname

* コーディング
コードを書く。

コミットする。

$ git status

$ git add filename

$ git commit -m "コメント"

直前のコミットを取り消す場合：

$ git reset --soft HEAD^

コーディングとコミットを繰り返す。

コミットは頻繁に、どのような単位で行なっても良い。

コーディング中の更新履歴は汚くなっても良い。

リモートのmasterに追随する(時々 and 最終テスト前)：

masterの更新を取得する。

$ git fetch origin/master

masterの更新の内容を確認する。

$ git log --oneline--prety=medium -10 origin/master

masterに更新に追随するためにrebaseする。

$ git rebase origin/master

作業を中断して他のブランチに移動する前に：

すべてコミットする。

$ git commit

もしくは、一時保存する。

$ git stash "コメント"

他のブランチから戻ってきたら。

$ git stash list

$ git stash pop

コーディング、テストを終え、他のメンバーに渡せる状態になったら、次の「マージ」に進む。

* マージ
新機能の追加など、チーム内部で機能レビューが必要な場合は、ステージング環境にマージ、デプロイ、機能レビューする。

バグや小さな変更など、機能レビューが不要な場合は、プロダクション環境にマージする。

* ステージング（dev）
devに移動する。

$ git checkout dev

devをリモートと同期する。

$ git pull origin/dev

masterに追随するために、rebaseする。

$ git rebase origin/master

対象のブランチに移動する。

$ git checkout branchname

devに更新に追随するためにrebaseする。

$ git rebase dev

これにより、masterには未適用だがdevに適用されている更新がブランチに適用されるので、ローカルでテストする。 バグ等あれば、ローカルで修正する。

これによる変更がなかった場合は、ステージングへのマージへそのまま進んで良い。

devに移動する。

$ git checkout dev

devにブランチをマージする。

$ git merge --squash branchname

$ git commit -m "コメント"

リモートにdevをpushする。

$ git push

ステージング環境にデプロイする。

ステージング環境でテスト、機能レビューする。

ダクション（master）

masterに移動する。

$ git checkout master

masterをリモートと同期する。

$ git pull origin/master

対象のブランチに移動する。

$ git checkout branchname

masterに更新に追随するためにrebaseする。

$ git rebase master

ステージングで機能テストしていた場合、devのみに適用されている更新が取り除かれるので、ローカルでテストする。 バグ等あれば、ローカルで修正する。

これによる変更がなかった場合は、マージへそのまま進んで良い。

masterに移動する。

$ git checkout master

masterにブランチをマージする。

$ git merge --squash branchname

$ git commit -m "コメント"

リモートにmasterをpushする。

$ git push

ブランチを削除する（しばらく待ったほうが良い？）。

$ git branch -D branchname

* リリース
プロダクションへのデプロイは、責任者が更新内容を確認してから行う。

masterに移動する。

$ git checkout master

masterをリモートと同期する。

$ git pull origin/master

前回のデプロイ移行の、masterの更新履歴を確認する。

$ git log

必要に応じて、前回のデプロイ時との差分を確認する

タグをつける。

$ git tag 2012.01.26

プロダクション環境にデプロイする。

