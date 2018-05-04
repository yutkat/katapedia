# GitLab

## インストール方法

### 設定


sudo vi /etc/gitlab/gitlab.rb


external_url 'http://ip:port'

gitlab_rails['gitlab_email_enabled'] = true

gitlab_rails['gitlab_email_from'] = 'mail@address'

gitlab_rails['gitlab_email_display_name'] = 'GitLab'

gitlab_rails['gitlab_email_reply_to'] = 'mail@address'

gitlab_rails['smtp_enable'] = true

gitlab_rails['smtp_address'] = "smtp_ipaddress"

gitlab_rails['smtp_port'] = 25

gitlab_rails['smtp_user_name'] = "mail@address"

gitlab_rails['smtp_password'] = "n@si2n@si2"

gitlab_rails['smtp_domain'] = "smtp_domain_ipaddress"

gitlab_rails['smtp_authentication'] = "login"


sudo gitlab-ctl reconfigure


## GitLabCI

### 使い方

https://kore1server.com/351/GitLab+CI%E3%81%AE%E5%9F%BA%E6%9C%AC%E3%83%81%E3%83%A5%E3%83%BC%E3%83%88%E3%83%AA%E3%82%A2%E3%83%AB%EF%BC%88%E7%BF%BB%E8%A8%B3%EF%BC%89
https://kore1server.com/353/GitLab+CI%E3%81%A8%E3%83%A9%E3%83%B3%E3%83%8A%E3%83%BC%E3%81%A7%E3%83%87%E3%83%97%E3%83%AD%E3%82%A4%E3%81%99%E3%82%8B

### 基本

#### runnerの設定ファイル

/etc/gitlab-runner/config.toml

### インストール方法

https://docs.gitlab.com/runner/install/


### 登録方法

#### docker-ssh

`sudo gitlab-runner register --url "http://127.0.0.1:8000" --registration-token "xbbKbDaymsBhfHv3enpM" --executor "docker-ssh" --docker-image "aaa/package:latest"`


#### docker

`sudo gitlab-runner register --url "http://192.168.1.100:8000" --registration-token "7Pszg9Muytx5LUnyd16r" --executor "docker" --docker-image "aaa/build:latest" --docker-privileged`

### 登録解除

`sudo gitlab-ci-multi-runner list`
`sudo gitlab-ci-multi-runner unregister --name xxx`


### 起動

~~~
QA testing:
stage: deploy
script:
- ls -l
~~~

dockerコンテナは先に起動させておく必要がある模様

### TroubleShooting

#### This job is stuck, because you don't have any active runners online with any of these tags assignedで動かない

runnerのタグと.gitlab-ci.ymlのタグが一致していないことが原因


---

## Wiki

### リンクの貼り方

https://gitlab.com/gitlab-org/gitlab-ce/issues/30204

## [過去バージョンの情報](./old/GitLab.md)


