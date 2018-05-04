# Java

## システム開発の鉄板構成

https://geechs-magazine.com/tag/tech/20170517

## 技術ノウハウ

NTTデータ
http://terasolunaorg.github.io/

## テストツール

### TestNG

#### テストクラスを無効化する

@Test(enabled=false)

をクラスの頭に設置


~~~
@Test(enabled=false)
public class DBConfigTest {

}
</code>
~~~

## スタイルチェック

### Checkstyle

http://blog-ja.sideci.com/entry/2017/12/27/checkstyle-and-oss

## ビルドツール

### gradle

#### proxy設定

~/.gradle/gradle.properties
か
プロジェクトルート

```
systemProp.http.proxyHost=ip
systemProp.http.proxyPort=port
systemProp.http.nonProxyHosts=192.168.1.*|192.168.2.*|localhost
systemProp.https.proxyHost=ip
systemProp.https.proxyPort=port
systemProp.https.nonProxyHosts=192.168.1.*|192.168.2.*|localhost
```

## 有名なプロジェクト

|プロジェクト名|プロジェクト概要|checkstyle.xmlの有無|ビルドツールへの組込|コーディング規約|
|---|---|---|---|---|---|
|[ReactiveX/RxJava](https://github.com/ReactiveX/RxJava)|非同期プログラミング用API|有|gradle|比較的少数のルールのみ採用|
|[iluwatar/java-design-patterns](https://github.com/iluwatar/java-design-patterns)|デザインパターンのJavaによる実装|有|maven|Google Java Styleベース|
|[elastic/elasticsearch](https://github.com/elastic/elasticsearch)|分散検索エンジン|有|gradle|比較的少数のルールのみ採用|
|[square/retrofit](https://github.com/square/retrofit)|型安全なHTTPクライアントライブラリ|有|maven|Google Java Styleベース|
|[square/okhttp](https://github.com/square/okhttp)|Android向けHTTPクライアントライブラリ|有|maven|Google Java Styleベース|
|[google/guava](https://github.com/google/guava)|Google Core Libraries for Java|-|-|Google Java Style|
|[PhilJay/MPAndroidChart](https://github.com/PhilJay/MPAndroidChart)|Android向けグラフライブラリ|-|-||
|[JetBrains/kotlin](https://github.com/JetBrains/kotlin)|プログラミング言語|-|-||
|[JakeWharton/butterknife](https://github.com/JakeWharton/butterknife)|Android向けView Injectionライブラリ|有|gradle|Google Java Styleベース|
|[bumptech/glide](https://github.com/bumptech/glide)|Android向けメディア管理ライブラリ|有|gradle|独自ルールで多数のチェックを実行|


## SpringFramework

### SpringBoot




## Tips

### Findbugsでエラーになるコード

~~~
/**
* 入力された文字列を標準出力に出力する.
* @param input 入力文字列
* @throws IllegalArgumentException 入力が null の場合
*/
public void printInput(String input) {
if (input == null) {
new IllegalArgumentException("input must not be null");
}
System.out.println("Input is " + input);
}
~~~



