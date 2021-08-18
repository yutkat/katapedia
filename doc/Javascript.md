# Javascript

## npm

--save-dev（package.jsonに書き込む設定）オプションは絶対に付けること！



## Grunt

### 追加プラグイン

"一覧":http://www.riaxdnp.jp/?p=4659


## jQuery

### タグ名、クラス名、ID名による指定方法

ID名:"#"

クラス名:"."


### 人気のjQuery

http://coliss.com/articles/build-websites/operation/javascript/best-jquery-plugins-and-scripts-2015.html


## SCSS

### Tips

.scss(.sass)ファイル名の先頭にアンダースコアを入れると、コンパイルしてもcss ファイルが作成されないという仕様があります。

http://tenderfeel.github.io/SassReference/basic/#partial



## Backbone.js

### FAQ

#### elの指定のない場合

$elに、tagNameやidの指定値を元にDOM要素を作り出して格納します。もし、tagNameの指定が無ければ、勝手にdivタグの要素を作成します。 これは、実際のhtmlと結びついていない状態になりますので、どこかで、この紐付きを書いてあげる必要があります。


http://lxyuma.hatenablog.com/entry/2013/11/17/134546


## 開発環境（yeoman）

http://suigin.jp/blog/?p=9


### FAQ

#### yoの調子がおかしい

yo doctorを実行する


## RequireJS

### 使い方

#### モジュールの作り方

~~~
// define()の第一引数に配列でモジュールIDを指定している
// 'backbone'というモジュールIDはconfigのpathsで定義されている
// 第二引数の関数の引数にあるBackboneというのは、
// 第一引数で指定したモジュールを参照するための変数名
define(['backbone'], function (Backbone) {
console.log(Backbone === window.Backbone);  // trueになる
});
~~~

http://yutapon.hatenablog.com/entry/2014/02/16/003052


### FAQ

#### 特定のプロパティを除外して新しいオブジェクトを作る

Object Rest Destructuring
ComputedPropertyName
https://rikuba.hatenablog.com/entry/20180202/ecmascript_object_omit

#### yeomanで作成したプロジェクトのbuildに失敗する

generator-backboneのgruntファイルに含まれているuserminがrequire.jsのブロックに対応していないので、index.htmlに書かれているような下記の状態だとエラーでbuildできない


buildできない。usePrepareの際にエラーがでる

<!-- build:js scripts/main.js -->

<script data-main="scripts/main" src="bower_components/requirejs/require.js"></script>

<!-- endbuild -->

Running "useminPrepare:html" (useminPrepare) task

Fatal error: require.js blocks are no more supported.

なので、buildのコメントアウトブロックを使用せず、require.jsのcompileを利用する。


Gruntfile.jsのgrunt.initConfigのrequirejsのブロックを下記のように変更


Gruntfile.jsのrequirejs  dist  optionsに

~~~
out: "<%= yeoman.dist %>/scripts/main.js",
include: ['templates','jquery','underscore', 'backbone'],
replaceRequireScript: [
{
files: ['<%= yeoman.dist %>/index.html'],
module: 'main',
modulePath: 'scripts/main'
}
]
~~~
を追加する


## CoffeeScript

### ファイル分割

http://dev.classmethod.jp/client-side/javascript/class-in-coffeescript/


## ライブラリ

http://www.buildinsider.net/web/popularjslib/2015


"wijimo":http://wijmo.com/

"AngularJSHub":http://www.angularjshub.com/


[Javascriptrライブラリ・フレームワーク一覧](Javascriptrライブラリ・フレームワーク一覧.md)


## デバッグコード表示:
console.log(i);



## AngularJS

### ジェネレータ

endpoint：REST APIのエンドポイントを生成するジェネレータ

route：あるURLに対するクライアントサイドのリソース一式を生成するジェネレータ

controller：クライアントサイドのコントローラを生成する。

filter：

directive：AngularJSのdirectiveを生成する。

service：AngularJSのserviceを生成する。

provider：

factory：

decorator：



http://strix01.blogspot.jp/2015/01/yeomanangularjs-full-stack-generator.html



## Node.js

### プロキシ環境下でnpmを実行する

npm config set proxy http://proxy.example.com:8080

