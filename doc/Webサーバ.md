# Webサーバ

## リバースプロキシを立てる理由

・ワンドメイン配下に複数のサービスが出来る

・リリース切り替えが容易になる

http://www.rickynews.com/blog/2014/08/25/why-you-need-to-use-reverse-proxy/



## nginx

### メンテナンス画面に飛ばす

~~~
location / {
root   html;
index  index.html index.htm;
if (-f "/tmp/maintenance" ) {
rewrite ^(.*)$ http://172.17.0.3:8000/maintenance.html? permanent;
}
}
~~~

### 設定

#### proxy_passとredirect

~~~
location  /foo {
rewrite /foo/(.*) /$1  break;
proxy_pass         http://localhost:3200;
proxy_redirect     off;
proxy_set_header   Host $host;
}

or

location /gitbucket {
rewrite ^/gitbucket/(.*) /$1 break;
rewrite ^/gitbucket$ /$1 break;
proxy_pass http://localhost:8082;
proxy_redirect   off;
proxy_set_header Host                   $host;
proxy_set_header X-Real-IP              $remote_addr;
proxy_set_header X-Forwarded-Proto      http;
proxy_set_header X-Forwarded-Host       $host;
proxy_set_header X-Forwarded-Server     $host;
proxy_set_header X-Forwarded-For        $proxy_add_x_forwarded_for;
client_max_body_size 500M;
}

~~~

~~~
location  /foo {
rewrite /foo/(.*) /$1  break;
proxy_pass         http://localhost:3200;
proxy_redirect     default;
}

or

location /gitbucket {
rewrite ^/gitbucket/(.*) /$1 break;
rewrite ^/gitbucket$ /$1 break;
proxy_pass http://localhost:8082;
proxy_redirect   default;
proxy_set_header Host                   $host;
proxy_set_header X-Real-IP              $remote_addr;
proxy_set_header X-Forwarded-Proto      http;
proxy_set_header X-Forwarded-Host       $host;
proxy_set_header X-Forwarded-Server     $host;
proxy_set_header X-Forwarded-For        $proxy_add_x_forwarded_for;
client_max_body_size 500M;
}
~~~

http://anopara.matrix.jp/2015/03/13/nginx%E3%81%A7%E3%83%AA%E3%83%90%E3%83%BC%E3%82%B9%E3%83%97%E3%83%AD%E3%82%AD%E3%82%B7%E3%81%99%E3%82%8B%E3%83%A1%E3%83%A2/


前者の方がいい模様


location  /foo {

rewrite ^/images/(.*)$ http://images.example.com/$1 redirect;

rewrite /foo/(.*) http://localhost:3200/$1  redirect; # permanent

}


恒久的な場合はこっち




#### proxy_redirect

バックエンドサーバがリダイレクトした時の Location ヘッダ。 on なら proxy_pass をホスト名としてリダイレクト。 off ならサーバの指示通りにリダイレクト。


#### rewriteのオプション

last

rewriteは完了で、以降のマッチするURI や locationの処理は継続します。

break

rewriteは完了です。

redirect

一時的なリダイレクトの意味で使います。このとき、HTTP ステータス番号 302 を返して、リダイレクトを実行します。

permanent

恒久的なリダイレクトの意味で使います。このとき、HTTP ステータス番号 301 を返して、リダイレクトを実行します


http://server-setting.info/centos/apache-nginx-4-setting-redirect.html


#### proxy_set_header

リバースプロキシを使う場合はproxy_passは先に定義する必要がある

※Unicornなどのソケット通信の場合はproxy_passを後ろに書いても問題ないっぽい。

