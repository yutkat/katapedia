# C/C++

***新しくC/C++を使って書くならRustで書くことを検討したほうがいい***

*理由*

- モダン
- 安全
- 周辺ツールが標準で組み込まれている
- コンパイルエラーが人にやさしい


## 必読

- Effective C++ https://herbsutter.com/elements-of-modern-c-style/

- awesome cpp https://awesomecpp.com/


## 低レイヤー

https://www.sigbus.info/compilerbook

## ヘッダー

### 宣言順

typedef
static定数
static変数
static関数
メンバ変数
コンストラクタ
デフォルトコンストラクタ
コピーコンストラクタ
その他のコンストラクタ
デストラクタ
メンバ関数
内部クラス・構造体


https://www.10106.net/~hoboaki/wiki/index.php?C%2B%2B%2F%E3%82%B3%E3%83%BC%E3%83%87%E3%82%A3%E3%83%B3%E3%82%B0%E8%A6%8F%E5%89%87


## 共通

### [Makefile](Makefile.md)

### [configure](configure.md)

### CMake

#### Configファイルをインストールに含める

`INSTALL(DIRECTORY "config" DESTINATION ${INSTALL_CONFIG} COMPONENT "configurations")`

#### デフォルトのインストール先を変更する

```
if(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
  set(CMAKE_INSTALL_PREFIX "/${PROJECT_PACKAGE_NAME}" CACHE PATH "..." FORCE)
endif()
```

### cscope


cscope -R -b -P `pwd`/


:set csre # -Pのパスを読み込む設定

:cscope add cscope.out

Vimでは、:cscope find search type search string という形式でCscopeの検索コマンドを実行する（cscope findの部分は:cs fで代用可）。search type（検索タイプ）には、以下のものがある。


symbol or s -- すべてのシンボル参照を検索

global or g -- グローバル定義を検索

calls or c -- 指定した関数の呼び出し箇所を検索

called or d -- 指定した関数が呼び出す関数を検索

text or t -- テキスト検索を実行

file or f -- ファイルを開く

include or i -- 指定ファイルをインクルードしているファイルを検索



http://stackoverflow.com/questions/2188405/how-to-let-cscope-use-absolute-path-in-cscope-out-file


### gdb

#### コマンドまとめ

http://www5a.biglobe.ne.jp/~mick/contents/program/gdb.html



#### マクロを参照する

info macro


#### 変数の定義を知りたい

whatis

ptype


structの場合は

ptype struct xxx

とする必要がある。


http://flex.phys.tohoku.ac.jp/texi/gdb-j/gdb-j_46.html


#### TUIモード

入る C-x C-a、<

出る C-x a

トグル C-x A


####  長いメッセージが...になる

set print elements 0


#### Missing separate debuginfos, use: debuginfo-installが出る

sudo yum install -y yum-utils


sudo debuginfo-install --nogpgcheck --enablerepo debug  xxx



http://dqn.sakusakutto.jp/2012/07/php53_imagick%EF%BC%BFsegmentation_fault.html


#### ソースコードが表示できない

show directories

でソースコードの格納ディレクトリがないことを確認する


directory ディレクトリ名

で追加する。

単数形になっているところに注意する。

#### リモートデバッグ

##### 直接ポートが解放できない環境でリモートデバッグする

1. install gdb gdb-gdbserver
2. `gdbserver --multi localhost:9999 <progname>`
3. ローカルフォワードする　`ssh -L 9999:localhost:9999 remote-server`
4. gdb
5. (gdb) target remote localhost:9999

#### デバッグオプションがついているか調べる

objdump --syms

シンボルが表示されるかどうか。No symbolsだとついていない

https://stackoverflow.com/questions/3284112/how-to-check-if-program-was-compiled-with-debug-symbols

ただしgdb上で確認したほうが確実(Readができない場合があるため)
(gdb) info sharedlibrary


### valgrind

~~~
valgrind -v --error-limit=no --leak-check=full --show-reachable=yes --trace-children=yes --track-fds=yes cmdmanageproc -in 0x123 -outxtp 0x456 -outxtc 0x789 -conf /xsat/sat/data_sat/XMC-P1/local/cplanconf.def 2>&1 | tee valgrind.log
~~~

or


~~~
valgrind -v --error-limit=no --leak-check=full  xxx
~~~

#### エラーメッセージの見方

http://d.hatena.ne.jp/taiyakisun/20150902/1441214819


## ツール

### プロファイラ

* EProfiler
* GProf
最適化オフ（-O0）らないと正しく表示されない

* OProfile
* Valgrind（callgrind）
最適化オフ（-O0）らないと正しく表示されない

* Very Sleepy
* gperftools
* perf
~~~
$ make
$ perf record ./a.out
$ perf report
~~~

### Kcahegrind

関数の呼出しグラフを可視化


http://blog.cybozu.io/entry/2016/09/15/100000


### AddressSanitizer（ASan）

メモリ関連のチェックツール



### clang-format

`clang-format -i -style="{BasedOnStyle: Google, IndentWidth: 4, Standard: C++11}" source-file`

ディレクトリ以下をFormat

`find . -regex '.*\.\(cpp\|hpp\|cc\|cxx\)' -exec clang-format -style=file -i {} \;`

オプション設定

http://algo13.net/clang/clang-format-style-oputions.html



### cccc

#### Complexity 複雑度（McCabeのサイクロマチック数）

10 以下であればよい構造

30 を越える場合，構造に疑問

50 を越える場合，テストが不可能

75 を越える場合，いかなる変更も誤修正を生む原因を作る


####  cccc 実行コマンド

`cccc $(find . -name "*.c" -o -name "*.h" -o -name "*.*pp")`


## C++高速化

https://heavywatal.github.io/cxx/speed.html


## カバレッジ

### CMakeでカバレッジ

コンパイルオプションを変更

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -O0 -Wall --coverage")

プロジェクトルートにファイルを持ってこないとうまくいかなかった

`find build -type f \( -iname \*.gcno -or -iname \*.gcda \) -exec cp {} . \;`
`gcovr -r .`


## Tips

### 戻り値をshared_ptr<Foo>で返すかそのままFooを返すか

コストを気にするならshared_ptrで返したほうがいいが、moveセマンティクスもあるためそのまま返したほうが読みやすいのではと思っている

参考：
https://stackoverflow.com/questions/45806526/move-semantic-vs-returning-a-shared-ptr
https://stackoverflow.com/questions/7977141/return-vectorfoo-or-shared-ptrvectorfoo

### static関数 vs namespace

JavaやC#と違ってnamespaceを使うのが一般的な模様。

https://stackoverflow.com/questions/14361408/using-a-namespace-in-place-of-a-static-class-in-c
https://softwareengineering.stackexchange.com/questions/134448/where-should-i-put-functions-that-are-not-related-to-a-class
https://stackoverflow.com/questions/9321/how-do-you-create-a-static-class-in-c/9328#9328


### C++で型を変数にしてcastしたい

できない

https://stackoverflow.com/questions/4972795/how-do-i-typecast-with-type-info

### 型の情報を取得する

typeidを使う

http://program.station.ez-net.jp/special/handbook/cpp/syntax/typeinfo.asp

### google testでprotected関数をテストする

#### その1

https://stackoverflow.com/questions/26337123/testing-protected-member-with-googletest

#### その2


``` c
class B : public A {
 public:
  using A::target_variable_or_function;
};
```


### gccバージョンごとの対応C++機能

各gccのバージョンでのstd=c++の対応状況

https://gcc.gnu.org/projects/cxx-status.html

#### 共有ライブラリから別の共有ライブラリをリンクすり

http://masahir0y.blogspot.jp/2013/01/shared-object.html


#### メンバ変数をコールバックで呼び替える

~~~
class Y {
public:
void callback(int a, int b) {
std::cout << "callback: " << (a+b) << std::endl;
}
};

void cbf(std::function<void(int,int)> f)
{
f(1,2);
}

int main()
{
Y o;
cbf(std::bind(&Y::callback, std::ref(o), std::placeholders::_1, std::placeholders::_2));

return 0;
}
~~~

http://ncnl.blog.so-net.ne.jp/2016-03-30



#### 引数argc, argvを作る

~~~
char *argv[] = {"program name", "arg1", "arg2", NULL};
int argc = sizeof(argv) / sizeof(char*) - 1;
~~~


#### 共有ライブラリのパスを取得する

gcc --print-search-dirs | ruby -ne 'puts $_.split(/:|=/).select{|e| e =~/^//}.map{|e| File.expand_path(e) }' | sort


#### デフォルトのインクルードパスを調べる

gcc ~~xc -E -v ~~


#### コピーコンストラクタとoperator=の重複コードを減らす

http://d.hatena.ne.jp/gintenlabo/20100707/1278532276

http://stackoverflow.com/questions/1477145/reducing-code-duplication-between-operator-and-the-copy-constructor

http://stackoverflow.com/questions/3279543/what-is-the-copy-and-swap-idiom


#### -gオプションが付いているか確認する

`readelf --debug-dump=line XXX`

なにか出力される

`objdump -W XXX`

Contents of the .debug_aranges section:

Contents of the .debug_pubnames section:

Contents of the .debug_info section:

がある

`nm -a XXX`

< 0000000000000000 N .debug_abbrev

< 0000000000000000 N .debug_aranges

< 0000000000000000 N .debug_info

< 0000000000000000 N .debug_line

< 0000000000000000 N .debug_loc

< 0000000000000000 N .debug_pubnames

< 0000000000000000 N .debug_pubtypes

< 0000000000000000 N .debug_st


#### メモリ使用量を計測する

GNUのtimeコマンドを利用する

***フルパスで***

`/usr/bin/time -f "%M KB" ls`

すべて計測

`/usr/bin/time -f "%Uuser %Ssystem %Eelapsed %PCPU (%Xtext+%Ddata %Mmax)k %Iinputs+%Ooutputs (%Fmajor+%Rminor)pagefaults %Wswaps"`


http://d.hatena.ne.jp/N_Nao/20111226/1324926173


#### ライブラリが32bitか64bitか調べる

共有ライブラリ

file libhoge.so


スタティックライブラリ

objdump -f libfoo.a | grep ^architecture


i386 ->32bit

i386:x86-64 ->64bit


#### gccで不要な関数をリンク時に自動的に除外

-Wl,--gc-sections


#### gccでlibがプレフィックスになっていない静的ライブラリにリンクする方法

cspice.aなどのスタティックライブラリにリンクする


gcc main.o -L/path/to/foo -l:foo.a


### 使っていないincludeを検出するツール

https://code.google.com/p/cppclean/

https://github.com/include-what-you-use/include-what-you-use


http://stackoverflow.com/questions/614794/c-c-detecting-superfluous-includes


## C

### 前提

2016年、C言語はどう書くべきか

http://postd.cc/how-to-c-in-2016-1/


### 知識DB

* 巨大な配列を使うには
1.static宣言する

2.ヒープ領域を用いる配列

~~~
int **p;
p= ((int **)malloc(sizeof(int *)*N)
for (i=0; i<N; i++){
p[i]=(int *)malloc(sizeof(int)*M)
}

...

for (i=0; i<N; i++){
free(p[i]);
}
free(p);
</code>
~~~


* void ポインタ
void ポインタはキャストしないのがK&Rに記載されている言語使用である。（voidポインタは定義された際に暗黙でキャストされるため）

gccのバージョン向上によりvoid*をchar*として扱うようになってしまった。そのため,(int*)malloc(sizeof(int))とするような記述が増えてきている。

実際には(int*)は不要である。



* return exitの違い
atexitの動作が異なってくる

returnの場合mainが終了しているので、static変数も開放される。exitの場合mainが終了していないため変数にアクセスできる


## C++

### C++の神器

|ツール名|説明|
|---|---|
|cccc|メトリクス計測|
|cppcheck|静的解析|
| [GoogleTest](GoogleTest.md) |ユニットテストツール|
|gprof|プロファイリング|
|doxygen|ドキュメントツール|


### コーディング規約

基本的には"Google C++スタイル":http://www.textdrop.net/google-styleguide-ja/cppguide.xml を守る


以下の部分利便性の観点からgoogle C++スタイルと異なるようにする


* 定数：すべて大文字_つなぎ
　→Cの命名規則と似ていて、目立つため

* thisを必ずつけること
→補完が効きかつ、名前の衝突が発生しないため。可読性が下がるが慣れれば大丈夫

* staticメンバ変数の場合はクラス名をつける。
* static const(final)の場合は付けなくてもよい。
* log4jのstatic変数loggerも例外とする。
* メンバ関数名：小文字で処理名のちキャメル
　→Qtで見やすかったから

* コメントはDoxygenQtスタイルで
* static変数は必ず定義時に初期化する
* 複数値が入るものは複数形にすること
* 以下のようなスペースのスタイルとする
~~~
void□write(int□type,□int□mode)□{
if□(type□==□TYPE_NORMAL)□{
git.insert("help").delete("/")
.trim();
}
else□{

}
}
~~~

### コード

#### スタイル

* [assert|ASSERTコード](assert|ASSERTコード.md)
* strnlen
~~~
int i_len=strlen(szp_del);
if( i_len > l_max ){
i_len = l_max;
}
~~~

#### コメント

~~~
// xxx
/* xxx
*
*/
</code>
~~~
のようにスペースを入れること


テンポラリなコメントは逆にワンスペースを入れないこと。また、テンポラリにコードをコメントアウトするときは行の先頭を"//"でコメントアウトすること


~~~


//
void getCurrentTime(
char *szp_time,
int   i_option
){
time_t timer=time(NULL);  // 暦変換
struct tm * st_lct=localtime(&timer); // 地方時変換

// 引数チェック
if(szp_time==NULL){ errlog; return; }

//    strftime(szp_time,D_TIME_LENGTH,"%Y%m%d%H%M%S",st_lct);  # <- 一時的なコメントアウトの例
//あとで変更すること！ # <- テンポラリなコメント
strftime(szp_time,D_TIME_LENGTH,"%Y%m%d",st_lct);
return;
}

</code>
~~~



#### ポインタ類のアスタリスクの位置

**Definition：**

`char *s;`

`char* s;`

どちらでもいいようだが、

`char* s;`

とする。


**Pros:**

`char* s;`

は定義と、値として使用する際の@*s@を区別できる。

定義時の@*@の意味とポインタ先を参照する@*@は定義上違うが、言語仕様上同じとなっており混乱を招くため。

また、関数定義の際も@get(char*, char&)@ のようが @get(char *, char &)@よりも見やすく感じる。

本来@*@は型定義の一貫なので変数名につけるのが妥当。


**Cons:**

Cでは

`char *s;`

が多い。

`char* s, t;`

としても

`char *s;`

`char t;`

と宣言される。

よって、*は変数定義でなく変数名にかかっている。


**Decision:**

どちらも捨てがたいがとりあえず、今のところ@char* s;@としておく。


### ツール

* [googletest](googletest.md)
* [リバースエンジニアリングツール](リバースエンジニアリングツール.md)
* バグ予測アルゴリズム bugspots

### コンパイルエラー解決集

* return closeなどでコケる（segmentation fault）
変数が見つからなくなってしまったか、解放する際に失敗してしまった場合が多い。

* [自作template class でundefined reference to](自作template class でundefined reference to.md)

## 知識DB

#### 自作共有ライブラリから別の共有ライブラリをリンクするときのgccのオプション

http://masahir0y.blogspot.jp/2013/01/shared-object.html



#### 演算子のオーバーロードは同一のものも行う

|  カテゴリ|  同類演算子|
|---|---|
|単項+と−|@単項+ 単項−@|
|単項+と−|@単項+ 単項−@|
|加減算|@前置++ 後置++ 前置−− 後置−− + − += −=@|
|乗除算|@* / % *= /= %=@|
|ビットシフト|@<< >> <<= >>=@|
|関係演算|@< > <= >=@|
|等価演算|@== !=@|
|ビット演算|<pre>単項～ & ˆ | &= ˆ= |= </pre>|


#### プリミティブ型の引数の渡し方

~~~
const int & MyClass::getFoo() { return m_foo; }
void MyClass::setFoo(const int & foo) { m_foo = foo; }
~~~

~~~
int MyClass::getFoo() { return m_foo; }  // Removed 'const' and '&'
void MyClass::setFoo(const int foo) { m_foo = foo; }  // Removed '&'
~~~

後者の方が良い模様


http://stackoverflow.com/questions/672700/what-is-the-use-of-passing-const-references-to-primitive-types

http://stackoverflow.com/questions/3009543/passing-integers-as-constant-references-versus-copying


#### doubleのイコール

~~~
if (fabs(a - b) < DBL_EPSILON) {
}
~~~
http://koze.hatenablog.jp/entry/2015/06/25/230000


####  static_castでvoidを返してのキャスト

~~~
#include <iostream>

int main()
{
int a = 10;
double* b = static_cast<double*>(static_cast<void*>(&a));

std::cout << *b << std::endl;
}
~~~

#### const: constを利用することで一時変数を参照が終了するまで保持することが可能らしい。。。

#### std::map: std::mapにはソートのためoperator<が使用されている。 よって、オリジナルclassを使用する際には、<をオーバーライドしてあげなければならない。

####  C++配列の大きさ計測

http://www.bohyoh.com/CandCPP/FAQ/index.html

２次元配列の要素数（ここでは、行数・列数と呼びます）は、以下のように求めることができます。

~~~
int x[7][6];
int n1 = sizeof(x) / sizeof(x[0]);          /* 行数すなわち7が得られる */
int n2 = sizeof(x[0]) / sizeof(x[0][0]);    /* 列数すなわち6が得られる */
~~~

####  C++ グローバル演算子とメンバ演算子

* グローバルのメリット
1+T,T+1両方に対応できる

* グローバルのデメリット
スコープがグローバル

* メンバ演算子のメリット
スコープが絞られる

* メンバ演算子のデメリット
クラスのサイズが大きくなる

1+Tができない

通常、operator<<以外はメンバに持つとする。ただし、1+Tをしたい場合にはもちろんグローバル定義をする


#### [例外処理](例外処理.md)

####  [staticまとめ](staticまとめ.md)

#### enum 継承

enumを継承させることは無理のようだ


#### includeが循環した際の対処方法

ヘッダファイルで前方宣言のみをする

class Test;

これによりヘッダ内で名前のみがわかる（インスタンスを生成する必要がないためインクルードの必要はなし）

ソースファイル内でincludeをして使用する


#### 複数の翻訳単位に渡る定義の方法

通常、プログラム中に定義はひとつしか書けませんが、いくつかの場合だけ、複数の翻訳単位に定義を書くことが許されています。

クラステンプレートのstaticデータメンバーも、その一つです。

他にも、クラス、enum、関数テンプレート、クラステンプレート、inline関数なども、複数の翻訳単位で定義できます。


#### C++ 演算子と記述方法(operator)

* 代入演算（「=」）
Test& Test::operator=(const Test& t);

* 自身に対する和差積商（「+=」「-=」「*=」「/=」）
Test& Test::operator+=(const Test& t);

* 和差積商（「+」「-」「*」「/」）
Test Test::operator+(const Test& t) const;

* 比較演算（「==」「!=」「<」「>」）
bool Test::operator==(const Test& t) const;

* 配列アクセス（「[ ]」）
int& Test::operator [ ] (int index) const

* 比較演算
~~~
bool MyClass::operator<(const MyClass &rhs)
{
return a < rhs.a;
}

std::sort(vec.begin(), vec.end());
Second option:

bool CompareMyClass(const MyClass &lhs, const MyClass &rhs)
{
return lhs.a < rhs.a; // this function will need to be declared friend if a is private
}

std::sort(vec.begin(), vec.end(), CompareMyClass);
Third option:

struct MyFunctor
{
bool operator()(const MyClass &lhs, const MyClass &rhs) const
{
return lhs.a < rhs.a;
}
};
~~~


std::sort(vec.begin(), vec.end(), MyFunctor());

http://ppp-lab.sakura.ne.jp/ProgrammingPlacePlus/cpp/language/019.html


### デフォルト生成関数

デフォルトコンストラクター：X()

コピーコンストラクター：X(const X &)

コピー代入：X &operator=(const X &)

ムーブコンストラクター：X(X &&)

ムーブ代入：X &operator=(X &&)

デストラクター：~X()

プログラマーがこれらの関数を宣言すると、コンパイラーは次の規則に基づいて生成しない。


* プログラマーがコンストラクターを一つでも宣言すれば、コンパイラーはデフォルトコンストラクターを生成しない。
* プログラマーがコピー関数、ムーブ関数、デストラクターの何れか一つでも宣言すれば、コンパイラーはコピー関数、ムーブ関数、デストラクターの何れも生成しない。

http://pegacorn.hatenablog.jp/entry/2015/04/26/111004


### 現代的C++

http://www.infoq.com/jp/news/2014/10/modern-cpp-essentials?utm_campaign=infoq_content&utm_source=infoq&utm_medium=feed&utm_term=global


### C++のいけてないところ

* 連想配列がない（マップかenumで代用）
*  コンストラクタでエラー処理しづらい
*  STLの頭が弱い
*  初期化がコンストラクタのみ
*  enumにアクセス指定子が指定できない（C11からOK）
*  POD以外をstatic変数にできない（開放処理が不定をなるため）
* STLが直感的でない
* mapの要素にアクセスするときにconstでとれない。（C11からOK）
* vectorの初期化が{0,1}とかじゃできない（C11からはOK）
* 同名、違戻り値の関数を作るのが大変
* メンバ関数をコールバック関数にできない（staticならできる。C11からはstd::function,std::bindでできる）

## TroubleShooting

### error: invalid conversion from 'char**' to 'const char**'になる

const char* const*にすると大丈夫な模様。

一つ目のポインタの値の書き換えはガードされるけど、ポインタのポインタの値の書き換えがガードできないからエラーと理解。

http://stackoverflow.com/questions/18273610/c-char-to-const-char-conversion/18273885

https://isocpp.org/wiki/faq/const-correctness#constptrptr-conversion


### clangを使ってboostのregexをstatic linkしようとするとエラーになる

`/usr/bin/ld: /usr/local/lib/libboost_regex.a(regex.o): relocation R_X86_64_PC32 against undefined hidden symbol `_ZTCN5boost10wrapexceptISt13runtime_errorEE0_NS_16exception_detail10clone_implINS3_19error_info_injectorIS1_EEEE' can not be used when making a shared object`

clangが悪いみたい。2020/03/10現在まだ解決してなさげ。
https://bugs.llvm.org/show_bug.cgi?id=40484

boostもclangでコンパイルするとうまくリンクできた

## FAQ

### コンフィグファイルをどこにインストールすべきか

/etcか/usr/local/etc

https://refspecs.linuxfoundation.org/FHS_3.0/fhs/ch04s09.html
https://unix.stackexchange.com/questions/15473/what-is-the-difference-between-etc-and-usr-local-etc


### 継承する際に継承元のアクセススコープを狭い方向に変更したい（public->protectedなど）

`class Car : private Engine`

https://www.bogotobogo.com/cplusplus/private_inheritance.php


### クラスと構造体の選択

https://docs.microsoft.com/ja-jp/dotnet/standard/design-guidelines/choosing-between-class-and-struct


### std::hexを使って16進表示にしたあとも16進表示のままになる

``` cpp
void printHex(std::ostream& x) {
   ios::fmtflags f(x.flags());
   x << std::hex << 123 << "\n";
   x.flags(f);
}

int main() {
    std::cout << 100 << "\n"; // prints 100 base 10
    printHex(std::cout);      // prints 123 in hex
    std::cout << 73 << "\n";  // problem! prints 73 in hex..
}
```

https://stackoverflow.com/questions/2273330/restore-the-state-of-stdcout-after-manipulating-it

### テンプレートを使って実装とヘッダを分けた場合にリンクエラーとなる

テンプレートメンバ関数をpublicで作った際に起こりがち

undefined reference to


解決するには、

1. 実装をヘッダに書く

2. ソースににインスタンス生成だけする

`template int func <int> (int);`


http://d.hatena.ne.jp/pknight/20090826/1251303641

http://qiita.com/i153/items/38f9688a9c80b2cb7da7




### 共有メモリのsoname等

http://wagavulin.hatenablog.com/entry/20091026/1256577635


### staticなメンバ関数にconstを付ける

付けられない。仕様書で決まっている。

http://stackoverflow.com/questions/12997967/c-why-cant-static-functions-be-declared-as-const-or-volatile-or-const-volati


### tagsに重複キーがある

ctags -R --links=no xxx



### ソースファイルでネームスペースを定義するときusingを使う？namespaceを使う？

namespaceがよい。メソッド名でコンフリクトしないため。

From a code readability standpoint, it is probably better in my opinion to use the #2 method for this reason:


You can be using multiple namespaces at a time, and any object or function written below that line can belong to any of those namespaces (barring naming conflicts). Wrapping the whole file in a namespace block is more explicit, and allows you to declare new functions and variables that belong to that namespace within the .cpp file as well


http://stackoverflow.com/questions/4122607/c-using-namespace-in-source-files

http://stackoverflow.com/questions/10816600/c-namespaces-how-to-use-in-header-and-source-files-correctly

http://stackoverflow.com/questions/8210935/creating-a-c-namespace-in-header-and-source-cpp

