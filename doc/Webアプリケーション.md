# Webアプリケーション

HTML5を使った実際の開発の進め方

http://itpro.nikkeibp.co.jp/atcl/column/14/121100125/021800005/


業務システムの開発におけるWeb技術の変化

https://html5experts.jp/albatrosary/3191/


2015年時点でのフロントエンド周りまとめ

http://blog.mmmcorp.co.jp/blog/2015/05/21/frontrend/



## ブラウザ

### ブラウザの仕組み

https://www.html5rocks.com/ja/tutorials/internals/howbrowserswork/


## Webサーバとアプリケーションサーバ

### Webサーバ

webサーバーはユーザーから送られてきた自サイトへのリクエストを受け取り、なんらかの処理を加えるプログラムです。 そして、場合によってはあなたのRailsアプリケーションにリクエストを投げます。 NginxとApacheは最も有名なwebサーバーです。

CSSやJavaScript、画像など、頻繁に変化しないファイルへのリクエストであれば、Railsアプリケーションはそのリクエストを処理する必要がないかもしれません。 webサーバーはRailsアプリケーションに処理を移譲することなく、そのリクエストを自分で処理できます。 また、そうした方が通常は速く完了します。

webサーバーはSSLリクエストや静的なファイルやアセット、圧縮されたリクエスト等を処理したり、その他大半のwebサイトが必要としそうな数多くの処理をこなしたりすることができます。 そしてもし、あなたのRailsアプリケーションがリクエストを処理しなければならない場合は、webサーバーはリクエストをアプリケーションサーバーにパスします。



### アプリケーションサーバ

アプリケーションサーバーはあなたのRailsアプリケーションを動かしているものです。 アプリケーションサーバーはあなたのコードを読み込み、アプリケーションをメモリに保持します。 アプリケーションサーバーはwebサーバーからリクエストを受け取ると、Railsアプリケーションにそのことを知らせます。 アプリケーションがリクエストを処理すると、アプリケーションサーバーはそのレスポンスをwebサーバーに返します。 （そのレスポンスは最終的にユーザーへ届きます。）

大半のアプリケーションサーバーはwebサーバーを使わずに単体で実行できます。 これはあなたがdevelopmentモードでやっていることです！ しかしproduction環境ではwebサーバーを手前に置くことが多いはずです。 webサーバーは複数のアプリケーションを一度に処理したり、アセットを素早くレンダリングしたり、リクエストごとに発生する多くの処理をさばいたりしてくれます。


### webサーバーとアプリケーションサーバーはどういう関係なのか？

webリクエストはwebサーバーが受け取ります。 そのリクエストがRailsで処理できるものであれば、webサーバーはリクエストに簡単な処理を加えてアプリケーションサーバーに渡します。 アプリケーションサーバーはRackを使ってRailsアプリケーションに話しかけます。 Railsアプリケーションがリクエストの処理を終えると、Railsはレスポンスをアプリケーションサーバーに返します。 そして、webサーバーはあなたのアプリケーションを使っているユーザーにレスポンスを返します。

もっと具体的に言えば、NginxはリクエストをUnicornに渡します。 UnicornはリクエストをRackに渡します。 RackはリクエストをRailsのrouterに渡します。 routerはリクエストを適切なcontrollerに渡します。 そしてレスポンスが逆の順番で返されます。


## 命名規約

### よく見るコンポーネントの名前（クラス名）

http://liginc.co.jp/web/html-css/css/166585


## HTML5周辺のライブラリ、フレームワーク、ツール

|  大分類|  小分類|  概要|  例|
|---|---|---|---|
|HTML|エディタ||Aptana Studio, Adobe Dreamweaver, Adobe Edge|
|HTML|エディタ||Aptana Studio, Adobe Dreamweaver, Adobe Edge|
|HTML|ビューア||各種ブラウザ|
|JavaScript|ライブラリ||underscore, JQuery, Bacon.js, etc|
|JavaScript|MV*フレームワーク||Backbone.js, Ember.js, Angular.js, Knockout.js, React, etc|
|JavaScript|UIフレームワーク||JQuery UI, jQuery Mobile, Twitter Bootstrap, etc|
|JavaScript|テンプレートエンジン||Handlebars, Hogan.js, Mustache, Dust, jsRender, etc|
|JavaScript|モジュール管理機構||RequireJS, CommonJS, etc|
|JavaScript|一元化、難読化||r.js, uglify, etc|
|CSS|||LESS, SCSS, Compass, etc|
|開発環境|ビルドツール||grunt|
|開発環境|パッケージ管理||bower|
|開発環境|開発ツールスタック||Yeoman|
|開発環境|デバッグ||各種ブラウザ|
|テスト|単体テスト||QUnit, Jasmine, Shinon.js, etc|
|テスト|結合テスト||PhantomJS, SimerJS, CasperJS, Selenium,etc|
|AltJS|||TypeScript, CoffeeScript, JSX|


http://www.nri.com/~/media/PDF/jp/opinion/teiki/g_souhatsu/2013/gs201310.pdf


## HTTPステータスコード

### HTTPステータスコードの選択フローチャート

http://postd.cc/choosing-an-http-status-code/


## サーバプッシュ

WebAssembly

PWA

WEBPUSH（HTTP/2）


ここらへんは古いやり方

Comet

XMPP

HTTPロングポーリング

Server Sent Events


https://www.slideshare.net/mawarimichi/push-37869433



## RESTful Hypermedia API

Hypermedia APIは、APIのレスポンスに関連する別のリソースのリンクURIが含まれており、APIの利用者は関連するリソースを事前に知らなくてもリンクURIを辿ることで関連する情報を取得したり操作することができます。

http://dev.classmethod.jp/planning/restful-hypermedia-api-intro/


## REACT.js

http://blog.masuidrive.jp/2015/03/03/react/


## Meteor

http://gihyo.jp/dev/serial/01/meteor/0007


## REST APIリファレンス

"GitHub Developer":https://developer.github.com/v3/

"Google Developer":https://developers.google.com/ad-exchange/buyer-rest/v1.3/?hl=ja

"Twitter":https://dev.twitter.com/rest/public

"Facebook":https://developers.facebook.com/docs/reference/

[amazon aws](http://docs.aws.amazon.com/AmazonS3/latest/API/APIRest.html)


[yahoo developer network](https://developer.yahoo.com/social/rest_api_guide/ysp_api_book.html)


## React

* reactのサンプル集
http://react.rocks/


## 種類

### サーバ

* nginx
* apache httpd
* jboss


### IDE

* Eclipse
* Subline text
* atom


### 標準API

* Java SE
* Java EE

### Webアプリケーションフレームワーク

* Spring
狭義の意味では、DI,AOP、MVC、ORMなどを含んだフルスタックフレームワーク

* Play Framework
CoCやホットリローディング、エラーのブラウザ上への表示といった方針により、開発者の生産性を上げることを目的としているMVCフレームワーク

* Dropwizard
YammerのバックエンドWebサービスを提供するために作られた、近年注目されている組み込みWebサーバを利用するタイプのフレームワーク。

* Seasar2
機能追加が停止されている。

* JSF2.0
* JAX-RS
* WebSocket attachment:WebSocket.pdf

#### JAX-RS vs JSF

JAX-RS

* フロント側はJavascriptMV*系フレームワークで構造化する前提で、RESTで作る。
* どれだけのフロントエンジニアの調達
* 教育ができるかが成否を分ける。

JSF

* フロント側はほどんど何もしない。フロント側はJSFが提供する機能のみで作る。
* フロントエンジニア不在でもある程度リッチなWebシステムが構築可能だが、限界はある。
* その他言いたかったことをつらつら・・・

フロントエンジニアとしては、JSX−RSを使って行きたいのですが、Struts+JSP主体の業務系エンジニアが、すぐフロントエンジニアにスキルチェンジできる訳ではないので、今後の業務系システムではJSFが主に使われていくのかなと考えています。

とはいえ、Struts＋JSPからJSFストレートに移行できるというような単純なものではなく、JSF独自のライフサイクルやお作法があるため、扱えるようになるための教育コストは掛かると思っておいた方がいいでしょう。



### DOM操作

* jQuery


### テンプレートエンジン

* Thymeleaf
XML/XHTML/HTML5テンプレートエンジンで、一般的なアプリケーションを通して表示するだけでなく、拡張子に.htmlを採用しているため、コンパイル無しでもブラウザ表示できる。

* Mustache.java
Java以外にRuby、JavaScript、Python、PHP、GO,Scala、node.js他多数の言語で使用可能なテンプレートエンジン

* Apache Velocity
HTMLだけでなくXMLやSQL文などのテキストファイルならどのようなものにでも適用できる高い汎用性を持っているテンプレートエンジン。

* Hogan
* EJS
* Handlebars
* Underscore.js（Template機能）
* JsRender

### UIフレームワーク

* Bootstrap
* jQueryUI
* YUI


### ドキュメント生成

* JSDoc3

### データベース

* Hibernate
データベースアクセスを直接高レベルなオブジェクト操作機能に置換することで、ドメインモデルを関連データベースにマッピングするフレームワーク。

* MybBatis
XMLまたはアノテーションを用いてSQLをオブジェクトとひもづけるフレームワーク。他のORMと異なりDBとオブジェクトのマッピングではなく、SQL文とオブジェクトのマッピングを行うため、レガシーな環境や非正規化されたDBでも有用。

* H2 Database
Javaプラットフォーム上で動き、組み込み、クライアント・サーバモードでも動作する、高速・軽量なデータベース。

* Flyway
複数人でのアプリケーション開発時のDBマイグレーション作業を素早く手軽に行うことができるDBマイグレーションフレームワーク。

* Commons DbUnit
関係データベース（JDBC）へのアクセスを容易にするライブラリ。



### オブジェクトマッピング

* Jackson
JavaオブジェクトとJSONの相互変換を行う高機能なライブラリ。

* Gson
JavaオブジェクトとJSONの相互変換をシンプルに行うライブラリ。

* Xstream
JavaオブジェクトとXMLの相互変換を行うライブラリ。

* dom4j
XMLをDOMツリーとして使うためのライブラリ。

* OrangeSignal CSV
CSV入出力操作を簡易化させる高機能なライブラリ。



### テスト

* JUnit
* testNG


### ログ

* Logback
log4jの開発者が作った後継的な実装ライブラリ。

* log4j2
log4jの最新バージョン。



### 各種ライブラリ

* Guava
Googleによって開発された、ユーテリティライブラリ。

* Lombok
AST変換によりJava特有の冗長なコードを簡潔に記述するためのライブラリ。

* Guice
アノテーションを使用した依存性の注入ライブラリ。

* RxJava
JavaでReactiveProgrammingを可能にするためのライブラリ。

* Jersey
RESTアーキテクチャにのっとってWebアプリケーションを記述するためのライブラリ。

* Jsoup
JQuery風HTMLを解析・操作するライブラリ。

* Ehcache
高速・シンプル・軽量なキャッシュライブラリ。

* ModelMapper
高機能でカスタマイズ性も高いオブジェクトマッピングライブラリ。

* Quartz
指定時間や特定の周期で実行したいジョブをcronのようにスケジュール・コントロールする機能を提供するライブラリ。



### ビルドツール

* Gradle
AntとMavenのいい部分を組み合わせたGroovyで記述するビルドツール

* Maven

### その他ツール

＊ JaCoCo

カバレッジ測定ツール

* FindBugs
* Checkstyle


### JVM言語

* Scala
* Groovy
スクリプト言語



### サーバサイドJavascript

* node.js


### CSSフレームワーク

* SCSS
* LESS
* Sass（Syntactically Awesome Style Sheets）
* Stylus
* Gumby


### テストモック作成ライブラリ

* Sinon.JS


### Javascriptソースコード静的解析

* QUnit
* Jasmine
* Mocha
* ESLint
* JSLint
* JSHint


### JSソースコード最適化

* Closure Compiler
* UglifyJS



### タスクランナー

* Grunt
* gulp.js


### パッケージマネージャ

* Bower


### プロジェクトのひな型生成ツール

* Yo


### 開発ワークフロー

* Yeoman
Grunt,Bower,Yo



### レイアウトフレームワーク

* Bootstrap
* Foundation
* Web Starter Kit
http://websae.net/framework-20141119/



### モックアップ作成ツール

* Adobe Dreamweaver
* Infragistics Indigo Studio
* Sencha Architect

## ブラウザpush通知

### 実装しているサイト

https://www.asahi.com/tool/webpush/


##  参考リンク

JavaScript http://www.buildinsider.net/web/popularjslib/2015

Javaフレームワークの歴史 http://javatechnology.net/diary/java-framework/

脱Struts時代 http://www.atmarkit.co.jp/ait/articles/1408/26/news040.html

ノンプログラマーがWebサイトを作るまで https://ferret-plus.com/2383

