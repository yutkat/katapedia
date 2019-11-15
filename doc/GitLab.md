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


---

## GitLab CI

### 汎用化したと思われるSonarqubeとGitLabCIの連携

```
image: docker:latest

variables:
  BUILD_ENV_VERSION: $CI_COMMIT_SHA
  DOCKER_HOST: tcp://docker:2375/
  DOCKER_DRIVER: overlay2
  CONTAINER_IMAGE: xxx.registry.gitlab.com/$CI_PROJECT_PATH
  PROGRAM_NAME: $CI_PROJECT_NAME

services:
  - docker:dind

cache:
  # key: "$CI_COMMIT_REF_NAME"
  paths:
    - build/

stages:
  - prepare
  - build
  - test
  - release

before_script:
  - echo ${CI_JOB_ID}
  - echo ${CI_JOB_NAME}
  - echo ${CI_COMMIT_REF_NAME}
  - echo ${CI_PROJECT_PATH}
  - echo ${CI_COMMIT_SHA}
  - echo ${CI_MERGE_REQUEST_IID}
  - docker info
  - export CURRENT_DIR=`pwd`
  - ls -l
  - ls -l *
  - docker login -u gitlab-ci-token -p $CI_JOB_TOKEN xxx.registry.gitlab.com

prepare-dev-image:
  stage: prepare
  script:
    - docker pull $CONTAINER_IMAGE:develop || true
    - docker build -f ./tools/docker/environment/Dockerfile --cache-from $CONTAINER_IMAGE:develop --tag $CONTAINER_IMAGE:develop .
    - docker push $CONTAINER_IMAGE:develop
  tags:
    - docker-in-docker

build:
  stage: build
  script:
    - apk add git bash
    - docker run -td --name built $CONTAINER_IMAGE:develop bash
    - docker cp ${CURRENT_DIR} built:/
    - docker exec --workdir /${PROGRAM_NAME} built bash -c "mkdir build || true && cd build && cmake -DCMAKE_BUILD_TYPE=Debug .. && make -j 20 | tee ../compile_warn.txt"
    - docker commit built $CONTAINER_IMAGE:built
    - docker push $CONTAINER_IMAGE:built
    - docker rm -f built
  artifacts:
    paths:
      - compile_warn.txt
  except:
    - tags
  tags:
    - docker-in-docker

unit_test:
  stage: test
  script:
    - docker run --name test --workdir /${PROGRAM_NAME} -e CI_PROJECT_PATH=$CI_PROJECT_PATH -e CI_COMMIT_SHA=$CI_COMMIT_SHA -e CI_COMMIT_REF_NAME=$CI_COMMIT_REF_NAME -e GITLAB_TOKEN=$GITLAB_TOKEN $CONTAINER_IMAGE:built bash -c "(cd ./bin && ./${PROGRAM_NAME}.test) && gcovr -r . -e test/ && gcovr -r . -e test/ -x -o coverage.xml && cppcheck -v --enable=all --xml -I include src 2> cpp_check_report.xml && /usr/local/bin/sonar-scanner-4.0.0.1744-linux/bin/sonar-scanner -Dsonar.projectKey=xxx -Dsonar.sources=src/ -Dsonar.cxx.includeDirectories=include/ -Dsonar.host.url=xxx -Dsonar.login=xxx -Dsonar.cxx.cppcheck.reportPath=./cpp_check_report.xml -Dsonar.cxx.coverage.reportPath=./coverage.xml -Dsonar.cxx.gcc.reportPath=./compile_warn.txt -Dsonar.gitlab.api_version=v4 -Dsonar.gitlab.project_id=$CI_PROJECT_PATH -Dsonar.gitlab.commit_sha=$CI_COMMIT_SHA -Dsonar.gitlab.ref_name=$CI_COMMIT_REF_NAME -Dsonar.gitlab.user_token=$GITLAB_TOKEN -Dsonar.gitlab.url=xxx -Dsonar.gitlab.quality_gate_fail_mode=none -Dsonar.gitlab.max_blocker_issues_gate=-1 -Dsonar.gitlab.max_critical_issues_gate=-1 -Dsonar.gitlab.merge_request_discussion=true"
    - docker cp test:/${PROGRAM_NAME}/coverage.xml .
    - docker cp test:/${PROGRAM_NAME}/cpp_check_report.xml .
    - docker rm -f test
  artifacts:
    paths:
      - coverage.xml
      - cpp_check_report.xml
  except:
    - tags
  tags:
    - docker-in-docker

release-image:
  stage: release
  script:
    - apk add git bash
    - docker run --name tmp-dev -v "${CURRENT_DIR}:/tmp/${PROGRAM_NAME}:z" --workdir /tmp/${PROGRAM_NAME} $CONTAINER_IMAGE:develop bash -c "mkdir build-release || true && cd build-release && cmake -DCMAKE_INSTALL_PREFIX=/tmp/${PROGRAM_NAME}/install .. && make -j 20 && make install"
    - docker build -t $CONTAINER_IMAGE:latest -f tools/docker/deploy/Dockerfile .
    - docker push $CONTAINER_IMAGE:latest
  only:
    - tags
  tags:
    - docker-in-docker


```





