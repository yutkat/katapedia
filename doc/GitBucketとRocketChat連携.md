# GitBucketとRocketChat連携

GitBucketとRocket.Chatを連携することでPullRequestやIssueのイベントをチャットに流すことができる。


## 手順

1.  [Gitbucket] Test/sampel(サンプル)のプロジェクトのService HooksでWebHook URL、jenkinsのプラグインのURLとしてhttp://192.168.1.10:3001/gitbucket-to-rocketchat/sample を登録する。(URLのsample部分はルームURLと一致していること（http://192.168.1.10:3000/channel/sample）)

