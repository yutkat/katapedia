# Docker


## Dockerとは

http://apatheia.info/blog/2013/06/17/docker/

http://codezine.jp/article/detail/8513


---


## Dockerインストール

### CentOS

http://saisa.hateblo.jp/entry/2013/12/10/153651

http://programming-10000.hatenadiary.jp/entry/20130803/1375514743


sudo rpm -ivh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm

sudo sed -i.bak 's/enabled=0/enabled=1/' /etc/yum.repos.d/epel-testing.repo

sudo yum install docker-io


### Ubuntu

~~~
sudo sh -c "wget -qO- https://get.docker.io/gpg | apt-key add -"
sudo sh -c "echo deb http://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list"
sudo aptitude    update
sudo aptitude install lxc-docker
~~~



https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-getting-started


### proxy設定

/etc/defaults/docker

export http_proxy='http://user:password@proxy-host:proxy-port'


sudo service docker restart


#### systemdにてdocker daemonにproxyを通す

Environment="HTTP_PROXY=http://ip:port

を書いて


sudo systemctl daemon-reload



---


## 簡易動作

~~~
sudo service docker start
sudo docker run -i -t centos /bin/bash
~~~

---


## ライフサイクル

* "docker run":http://docs.docker.io/en/latest/commandline/cli/#run コンテナを作成する.
* "docker stop":http://docs.docker.io/en/latest/commandline/cli/#stop コンテナを停止する.
* "docker start":http://docs.docker.io/en/latest/commandline/cli/#start コンテナを起動する.
* "docker restart":http://docs.docker.io/en/latest/commandline/cli/#restart コンテナを再起動する.
* "docker attach":http://docs.docker.io/en/latest/commandline/cli/#attach 起動中のコンテナに接続する．
* "docker rm":http://docs.docker.io/en/latest/commandline/cli/#rm コンテナを破棄する(コンテナを停止する必要がある)
* "docker wait":http://docs.docker.io/en/latest/commandline/cli/#wait コンテナが停止するまでブロックする.

コンテナを起動し，それに接続したい場合は，`docker start`してから`docker attach`する．


完全に一時的なコンテナを作りたい場合は，`docker run -rm`で作成すれば停止(`docker stop`)後にコンテナは破棄される．



ホストのディレクトリをコンテナ内にマウントしたい場合は，`docker run -v $HOSTDIR:$DOCKERDIR IMAGE COMMAND`とする．


## 各種情報の表示

* "docker ps":http://docs.docker.io/en/latest/commandline/cli/#ps 起動中のコンテナを表示する．
* "docker inspect":http://docs.docker.io/en/latest/commandline/cli/#inspect コンテナの全情報を表示する（IPを含む）．
* "docker logs":http://docs.docker.io/en/latest/commandline/cli/#logs コンテナのログを表示する．
* "docker events":http://docs.docker.io/en/latest/commandline/cli/#events コンテナ内のイベントを表示する．
* "docker port":http://docs.docker.io/en/latest/commandline/cli/#port コンテナのportを表示する．
* "docker top":http://docs.docker.io/en/latest/commandline/cli/#top コンテナのプロセスを表示する．

---


## Dockerfile

### ONBUILD

ONBUILDを使うと，次のビルドで実行するコマンドをイメージに仕込むことができるようになる． つまり，ベースイメージにONBUILDによるコマンドを仕込み，別のDockerfileでそのベースイメージを読み込みビルドした際に，そのコマンドを実行させるということが可能になる． 要するに， 親DockerfileのDockerfileコマンドを子Dockerfileのビルド時に実行させることができる機能．



## ベースイメージの作成

### CentOS6

cent65.sh


~~~
sudo -i

export http_proxy=http://ip:port
yum install febootstrap xz

#MIRROR_URL="http://ftp.riken.jp/Linux/centos/6.5/os/x86_64/"
#MIRROR_URL_UPDATES="http://ftp.riken.jp/Linux/centos/6.5/updates/x86_64/"

# 64bit
MIRROR_URL="http://vault.centos.org/6.5/os/x86_64/"
MIRROR_URL_UPDATES="http://vault.centos.org/6.5/updates/x86_64/"

# 32bit
#MIRROR_URL="http://vault.centos.org/6.5/os/i386/"
#MIRROR_URL_UPDATES="http://vault.centos.org/6.5/updates/i386/"


cd /tmp
febootstrap -i bash             -i coreutils             -i tar             -i bzip2             -i gzip             -i vim-minimal             -i wget             -i patch             -i diffutils             -i iproute             -i yum             centos centos65 $MIRROR_URL -u $MIRROR_URL_UPDATES
tar --numeric-owner -Jcpf centos-6.5_64.tar.xz -C centos65 .
~~~

$ ./cent65.sh

作成したbase imageを登録するには以下のコマンドを実行します。


$ cat centos-6.5_64.tar.xz | sudo docker import - local/centos:6.5_64

試しに以下のようにechoコマンドを実行すると問題なく実行できました。


$ docker run local/centos:6.5 echo 'hello world'


#### 32bitの場合

Dockerfileに以下を書く


ENTRYPOINT ["linux32"]


直接コマンドから実行する場合には

/usr/bin/linux32 /bin/bash

とする


http://stackoverflow.com/questions/26490935/how-to-fake-cpu-architecture-in-docker-container



32bitのイメージを使って別のイメージを作る場合、dockerfileの各RUNにlinux32を記述する必要がある。


#### febootstrapを使用しない方法

centos

~~~
export BASEURL="http://vault.centos.org/6.5/os/i386/Packages/"
sudo mkdir /tmp/sysroot
sudo wget "${BASEURL}/centos-release-6-5.el6.centos.11.1.i686.rpm"
sudo rpm --root /tmp/sysroot --rebuilddb
sudo rpm --root /tmp/sysroot -i centos-release-6-5.el6.centos.11.1.i686.rpm
~~~

ubuntu

https://github.com/docker-32bit/ubuntu/blob/master/build-image.sh



### CentOS7

febootstrapの代わりにsuperminを使うらしい


## Dockerの移行作業

### イメージの移動

**docker save -o <save image to path> <image name>**

~~~
sudo docker images |  awk 'NR>1 {print $1":"$2}' | tr  / _ > aaa  # スラッシュを加工
sudo docker images |  awk 'NR>1 {print $1":"$2}' > bbb
paste -d " " aaa bbb | awk '{print "sudo docker save -o " $0}' > ccc
./ccc
~~~

**docker load -i <path to image tar file>**

ls -1 | xargs -i sudo docker load -i {}


### コンテナの移動

**docker export**


**docker import**


## ssh接続できるDockerコンテナ

~~~
FROM centos

ENV http_proxy http://ip:port ENV https_proxy http://ip:port ENV ftp_proxy http://ip:port

RUN yum update -y
RUN yum install -y git man sudo openssh-server
RUN bash -c "echo root:root" | chpasswd
RUN mkdir -p /var/run/sshd
RUN /usr/bin/ssh-keygen -t rsa -f /etc/ssh/ssh_host_ecdsa_key -C '' -N ''
RUN /usr/bin/ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key -C '' -N ''
RUN /usr/bin/ssh-keygen -t rsa -f /etc/ssh/ssh_host_dsa_key -C '' -N ''
RUN /usr/bin/ssh-keygen -t ed25519 -f /etc/ssh/ssh_host_ed25519_key -N ''
CMD /usr/sbin/sshd -D
EXPOSE 22
~~~

`sudo docker run -d -p 22 xxx/xxx:xxx /usr/sbin/sshd -D`


## 共有ディレクトリ

-v /var/docker/share:/workspace

ホスト：コンテナの順

をdocker run時に追記する


## ワークディレクトリ移動

-w /share

をdocker run時に追記する


## ビルドパラメータ

`sudo docker build -t コンテナグループ/コンテナ名:バージョン Dockerfileのあるディレクトリのパス`



## 実行パラメータ

`sudo docker run  -v /var/docker/share:/share -w /share  -itd -p 22 bpos/l1dev:v0.01 /usr/sbin/sshd -D`


## 各ディストリビューションのdockerfile

### Alpine

#### ユーザー追加

useraddが使えないのでadduserを使う

`RUN addgroup -g 1018 group-name && adduser -u 1016 -G group-name -D user-name&& echo "user-name ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers && echo 'password:password' | chpasswd`


## docker-compose

### インストール

sudo apt-get install python-dev libffi-dev libssl-dev

sudo easy_install pip

sudo pip install -U docker-compose


### 使い方

docker-compose up -d


## TroubleShooting

### docker execするとターミナルのサイズがおかしい

表示されるターミナルサイズが小さくなってしまう。ウィンドウリサイズで直るがリサイズがめんどくさいときに

`docker exec -it -e COLUMNS=`tput cols` -e LINES=`tput lines xxx /bin/bash` 

docker v18.06では直ってるらしい

https://github.com/moby/moby/issues/33794


### 共有ディレクトリでPermission Deniedになる

SELinuxが原因

`-v host:guest:Z`

で直る

https://stackoverflow.com/questions/24288616/permission-denied-on-accessing-host-directory-in-docker

### Cannot start containers: port is already allocated  failed: port is already allocated

ps aux | grep docker-proxy

sudo netstat -tulpen | grep docker

すると存在しないコンテナのdocker proxyがポートを抑えている



docker停止

sudo rm /var/lib/docker/network/files/local-kv.db

で直った


https://github.com/docker/docker/issues/20486


## Tips

### Postgresとかをentrypointでpsqlを叩かないで実行する方法

docker-composeにentrypointを設定

```
  psql-executor:
    container_name: postgres
    image: postgres
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres_pass
      - POSTGRES_DB=test
      - POSTGRES_HOST=test-db
      - POSTGRES_PORT=5432
    entrypoint: /bin/sh -c "PGPASSWORD=$$POSTGRES_PASSWORD psql -h $$POSTGRES_HOST -p $$POSTGRES_PORT -U $$POSTGRES_USER $$POSTGRES_DB -t -A"
```

```
docker-compose run psql-executor < <(echo "$sql_command")
```

変数が展開されたりするのでテクニックが必要


### daemonみたいにコンテナを起動しっぱなしにしたい

exec -itで入って作業するときとかに

`docker run -td xxx /bin/bash`
`docker exec -it yyy /bin/bash`


https://stackoverflow.com/questions/25775266/how-to-keep-docker-container-running-after-starting-services


### コンテナ内のプロセスで使っている設定ファイルを一時的に変更したい

1. `docker-compose xxx-service /bin/bash`
2. `vim /path/to/config`
3. `docker-compose restart xxx-service`
4. `docker-compose logs xxx-service`


### ログインした先がdockerかどうか調べる

一番はやいのは `/.dockerenv`の有無を調べること
LXCとかも対応したいなら `/proc/1/cgroups` の中身を確認する

https://stackoverflow.com/questions/20010199/determining-if-a-process-runs-inside-lxc-docker

### root以外で実行する

root以外のユーザで実行した場合

Cannot connect to the Docker daemon. Is the docker daemon running on this host?

とかが出る場合


ls -l /var/run/docker.sockで権限確認

sudo usermod -aG docker jenkins

等で権限追加


https://unix.stackexchange.com/questions/252684/why-am-i-getting-cannot-connect-to-the-docker-daemon-when-the-daemon-is-runnin


### docker-composeイメージ更新

docker-compose rm -f

docker-compose pull

docker-compose up --build -d


http://stackoverflow.com/questions/32612650/how-to-get-docker-compose-to-always-start-fresh-images



### noneのイメージを削除

@ sudo docker rmi $(docker images | awk '/^<none>/ { print $3 }')@


### docker buildの途中からやり直す方法

step指定はできないみたいなのでv1.9以上は

`docker build --build-arg commit=3e1670c59af1`

とかでやる。


v1.9未満は

Dockerfileの指定の部分に

echo "restart" && command

と書いて、

ビルドし直す

とかやって強引に実行する



### コンテナにてcore fileを出す

ホストの/procをマウントしているためホスト側でコアダンプする設定にする必要がある。


`echo '/tmp/core.%e.%p.%t' > /proc/sys/kernel/core_pattern`



### build時にDNSを設定する

mdesales@ubuntu ~ $ cat /etc/default/docker

DOCKER_OPTS="--dns 172.18.20.13 --dns 172.20.100.29 --dns 8.8.8.8"



sudo service docker restart


http://stackoverflow.com/questions/25130536/dockerfile-docker-build-cant-download-packages-centos-yum-debian-ubuntu-ap



### docker build時に環境変数を渡す

`sudo docker build --build-arg  http_proxy=http://ip:port`


### docker run -ditした際の標準出力を確認したい

`docker logs <id>`

にて確認できる




### sudoユーザの追加

~~~
RUN echo "docker    ALL=(ALL)       ALL" >> /etc/sudoers.d/docker
~~~


### DockerfileのFROMのイメージ内にproxyを使用する場合

ONBUILDという方法を取れば良い模様。


ベースとなるDockerイメージ（FROMで指定）と作成するDockerイメージ（docker build -tで指定）の両方に同じ名前を指定しています。


docker-http-proxy

#!/bin/sh


if [ $# != 1 ]; then

echo "Usage: $0 DOCKER_IMAGE"

exit 1

fi


cat <<EOF | docker build ~~t $1 ~~

FROM $1

ONBUILD ENV http_proxy $http_proxy

ONBUILD ENV https_proxy $https_proxy

ONBUILD ENV no_proxy $no_proxy

EOF


$ docker-http-proxy fedora:21

Sending build context to Docker daemon 2.048 kB

Sending build context to Docker daemon

Step 0 : FROM fedora:21

---> 834629358fe2

Step 1 : ONBUILD env http_proxy http://proxy.example.com:8080

---> Running in 7293472d9793

---> 35244c2f0c76

Removing intermediate container 7293472d9793

Step 2 : ONBUILD env https_proxy http://proxy.example.com:8080

---> Running in 4a52cac11df4

---> 5190135a8d40

Removing intermediate container 4a52cac11df4

Step 3 : ONBUILD env no_proxy 127.0.0.1,localhost

---> Running in fedf6baa373a

---> eda7740be6ee

Removing intermediate container fedf6baa373a

Successfully built eda7740be6ee

これでプロキシ設定済みのfedora:21イメージが完成しました。


最初の例に挙げたDockerfileを再びビルドしてみます。


$ docker build -t httpd .

Sending build context to Docker daemon 2.048 kB

Sending build context to Docker daemon

Step 0 : FROM fedora:21

1. Executing 3 build triggers
Trigger 0, ENV http_proxy http://proxy.example.com:8080

Step 0 : ENV http_proxy http://proxy.example.com:8080

---> Running in 1d862437b875

Trigger 1, ENV https_proxy http://proxy.example.com:8080

Step 0 : ENV https_proxy http://proxy.example.com:8080

---> Running in 2483039ae324

Trigger 2, ENV no_proxy 127.0.0.1,localhost

Step 0 : ENV no_proxy 127.0.0.1,localhost

---> Running in 6cf89a748f98

---> c00437a577bd

Removing intermediate container 1d862437b875

Removing intermediate container 2483039ae324

Removing intermediate container 6cf89a748f98

Step 1 : RUN yum update -y && yum install -y httpd && yum clean all

---> Running in 3b5ba7989288


（中略）


Complete!

Cleaning repos: fedora updates

Cleaning up everything

---> 2c8f2b5806be

Removing intermediate container 3b5ba7989288

Successfully built 2c8f2b5806be

今度はONBUILDで埋め込んでおいたプロキシ設定が読み込まれ、正常にビルドが終了しました。



http://qiita.com/speg03/items/4b8573686e7bbf218b61



### あとからポートを公開する

iptables -t nat -A DOCKER ! -i docker0 -p tcp -m tcp --dport 9292 -j DNAT --to-destination 172.17.0.49:80

iptables -A FORWARD -d 172.17.0.49/32 ! -i docker0 -o docker0 -p tcp -m tcp --dport 80 -j ACCEPT


確認は、

sudo iptables -t nat -L


## プライベートなDocker Hubを作る

プライベートレジストリを起動すればよい


docker run -d -p 5000:5000 --restart=always --name registry registry:2


https://docs.docker.com/registry/deploying/


## バグ？

### npm installが失敗するときがある

何度がやればなぜか通る


~~~
npm ERR! Linux 3.13.0-63-generic
npm ERR! argv "node" "/usr/local/bin/npm" "install" "hubot" "hubot-scripts" "hubot-diagnostics" "hubot-help" "hubot-heroku-keepalive" "hubot-google-images" "hubot-google-translate" "hubot-pugme" "hubot-maps" "hubot-redis-brain" "hubot-rules" "hubot-shipit" "--save"
npm ERR! node v0.12.4
npm ERR! npm  v2.11.1
npm ERR! code ENOTFOUND
npm ERR! errno ENOTFOUND
npm ERR! syscall getaddrinfo

npm ERR! network getaddrinfo ENOTFOUND registry.npmjs.org
npm ERR! network This is most likely not a problem with npm itself
npm ERR! network and is related to network connectivity.
npm ERR! network In most cases you are behind a proxy or have bad network settings.
npm ERR! network
npm ERR! network If you are behind a proxy, please make sure that the
npm ERR! network 'proxy' config is set properly.  See: 'npm help config'

npm ERR! Please include the following file with any support request:
npm ERR!     /home/hubot/npm-debug.log
~~~

## FAQ

### gdbで warning: Error disabling address space randomizationとなる

docker run時に

`-security-opt seccomp=unconfined`
を追加すればよい


### Docker上でCtrl+pが２回押さないと効かない


デタッチのショートカットと衝突しているため



~/.docker/config.json

~~~
{
"detachKeys" : ""
}
~~~



### Docker上でtmuxが起動できない

正確にはdocker execのときに起動できなくなる模様。


これでできるようになる

`docker exec -it my-container env TERM=xterm script -q -c "/bin/bash" /dev/nul`


17.06バージョンからは上をやらなくてもできるようになる見込み


https://github.com/moby/moby/issues/8755



### CentOS7にてhttpでdocker pushしたい

/etc/sysconfig/dockerに

INSECURE_REGISTRY='--insecure-registry ip:port'

を追加



### ホストPC起動時にコンテナを起動したい

dockerのsystemdに追加する

https://docs.docker.com/engine/admin/host_integration/


### Dockerで他PCと同一セグメント（サブネット）に参加したい

Pipeworkを使ってブリッジ接続するのが良い模様


http://blog.takanabe.tokyo/2014/05/30/1012/

http://www.agilegroup.co.jp/technote/docker-network-in-bridge.html


### Dockerコンテナ上でsystemdを使いたい

ホスト側のcgroupsとの兼ね合いでゲスト・ホスト間の疎結合性が崩れるからインストールされていない。

パッケージコマンド管理による依存関係をだまくらかす意味でfakesystemdが入れられている。


強引に使いたい場合は以下を参照のこと。


http://fight-tsk.blogspot.jp/2015/02/dockercentossystemd.html


### docker runにてproxyを通す

docker run -e http_proxy=xxx:yyy


### dockerでsudo docker run -i -t centos /bin/bashがPulling repository cent connection timed outとなったときの対処法

~~~
sudo HTTP_PROXY=http://localhost:3128/ docker -d &
~~~

/etc/init.d/dockerに書き込むとなお良い


### CentOS7で（要はsystemd）自動起動

sudo vi /etc/sysconfig/docker

に以下を記述

http_proxy=http://ip:port

https_proxy=http://ip:port

HTTP_PROXY=http://ip:port

HTTPS_PROXY=http://ip:port

no_proxy=no_proxy_ip:port


### EXPOSEの意味

EXPOSEは書いただけで何か動作が変わるといったものではなく、他の機能から参照されて意味を持つ。

dockerに入門したてだとEXPOSEを指定しないとコンテナにネットワーク接続できないと勘違いしてしまったりするがそんなことはない。

EXPOSEしなくてもdocker run -pオプションでポートを外部公開できるし、dockerネットワークのIPを使ってコンテナ間もしくはホスト・コンテナ間で通信可能だ。


### CMDとENTRYPOINT

ENTRYPOINTはrun時の引数がENTRYPOINTの引数になる。CMDのときはrun時の引数がそのまま置き換わる。

要するにENTRYPOINTってのは絶対なわけだ。runする人が変えられない。Dockerfile作者が絶対に実行したいコマンドってこと。

CMDは、runする人しだいで置き換えられるもの。

