# GitBucketとJenkins連携

連携することでPullRequest時にビルドが自動で走るようにすることができる


## 手順

1. [Gitbucket] jenkinsbotユーザーをjenkinsから実行したい対象プロジェクトに追加する。

2. [Gitbucket] Test/sample(サンプル)のプロジェクトのService HooksでWebHook URL、jenkinsのプラグインのURLとして http://192.168.1.20:8081/ghprbhook/ を登録する。

3. [Jenkins]任意の新規ジョブを作成し、 GitHub projectにgitbucketのURLに http://192.168.1.20:8082/Test/sample を設定。

4. [Jenkins]ソースコード管理でGitを選択し、以下のようにそれぞれ設定。

* Repository URL - http://192.168.1.20:8082/git/Test/sample.git
* Credentials - 指定無し
* Refspec(高度な設定から遷移) - +refs/pull/*:refs/remotes/origin/pr/*
* Branches to build - ${sha1}

5. [Jenkins]ビルド・トリガの設定。 GitHub Pull Request Builderをチェックし、以下の様に設定。

* GitHub API credentials - http://192.168.1.20:8082/api/v3
* Admin list - jenkinsbot

