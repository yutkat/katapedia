# Makefile

## 書き方

http://minus9d.hatenablog.com/entry/20140203/1391436293


## 特殊変数の取り扱い

$@ : ターゲットファイル名

$% : ターゲットがアーカイブメンバだったときのターゲットメンバ名

$< : 最初の依存するファイルの名前

$? : ターゲットより新しいすべての依存するファイル名

$^ : すべての依存するファイルの名前

$+ : Makefileと同じ順番の依存するファイルの名前

$* : サフィックスを除いたターゲットの名前


http://www.jsk.t.u-tokyo.ac.jp/~k-okada/makefile/


## イコールの違い

|記号|意味|
|---|---|
|=|右辺の内容を憶えておき実際に使う時に展開される。代入には「=」を使う|
|:=|代入行がMakefileから読みこまれるとすぐに右辺を評価する。|
|?=|値がセットされていないときのみ変数に値を代入|
|+=|変数の最後にテキストを追加する|

http://minus9d.hatenablog.com/entry/20140204/1391527870


### すべてのC,C++をコンパイル

$(patsubst %.cpp,%.o,$(wildcard *.cpp)) $(patsubst %.c,%.o,$(wildcard *.c))


## 使い方

### LIBSやCFLAGSなどのシンボル

|  シンボル|  説明|  default|
|---|---|---|
|CC|Cコンパイルコマンド|cc|
|CC|Cコンパイルコマンド|cc|
|CXX|C++コンパイルコマンド|g++|
|CFLAGS|Cコンパイルオプション|無し|
|CXXFLAGS|C++コンパイルオプション|無し|
|CPPFLAGS|C++プリプロセッサ用オプション|無し|
|LDFLAGS|ldというリンクを呼び出すコマンドのリンクオプション|無し|
|INCLUDES|includeするheaderのディレクトリを指定する|無し|
|LIBS|利用するライブラリを指定する|無し|
|TARGET|生成するターゲットファイル名|無し|
|SRCS|ターゲットファイルを生成するために利用するソースコード|無し|
|OBJS|ターゲットファイルを生成するために利用するオブジェクトファイル|無し|
|RM|ファイル削除コマンド|rm -f|

http://yut.hatenablog.com/entry/20120702/1341185909


### CFLAGS,LDFLAGSのオプションがどれか調べる

gccのリンカはldを呼んでいるだけなので、ldのオプションがLDFLAGSになる。


http://www.asahi-net.or.jp/~wg5k-ickw/html/online/gcc-2.95.2/gcc_2.html


### セキュアなCFLAGS,LDFLAGS

ライブラリ

CFLAGS=-fstack-protector-all -O2 -fno-strict-aliasing -D_FORTIFY_SOURCE=2

LDFLAGS=-Wl,-z,now,-z,relro


実行ファイルは追加で

CFLAGS=-fPIE

LDFLAGS=-pie


### 多段Make

http://exlight.net/devel/make/child_process.html


## べからず集

### suffix ruleについて

suffix ruleは古い書き方で、見た目にも分かりにくい（依存関係の順番がpattern ruleと逆になっている）。GNU makeを使う限りにおいては、suffix ruleではなくpattern ruleを使う方がよいらしい。


### PHONY

.PHONY: all

.PHONY: clean

は必ず書くこと。


targetがallやcleanでも誤動作しないようにするため


### イコールの使い方

=は動作が直感的でないため:=を使用すること


### ヘッダファイルの依存関係を解決すること

http://d.hatena.ne.jp/wagavulin/20120405/1333629926


## FAQ

#### make で同じターゲット名を定義

二重コロンでターゲットを定義すると重複を許容できる


~~~
==> 1.mk <==
all ::
echo 1

==> 2.mk <==
all ::
echo 2

==> 3.mk <==
all ::
echo 3
~~~

http://uyota.asablo.jp/blog/2010/09/04/5329742


#### includeするときの ディレクトリの場所が移動できない

Makefileから別のMakefileを呼ぶ場合、カレントディレクトリから移動できない

次の変数を定義して使用すればよい。


~~~
TOP := $(dir $(lastword $(MAKEFILE_LIST)))
or
ROOT_DIR:=$(shell dirname $(realpath $(lastword $(MAKEFILE_LIST))))
or
mkfile_path := $(abspath $(lastword $(MAKEFILE_LIST)))
current_dir := $(notdir $(patsubst %/,%,$(dir $(mkfile_path))))
~~~

http://stackoverflow.com/questions/18136918/how-to-get-current-directory-of-your-makefile

http://stackoverflow.com/questions/322936/common-gnu-makefile-directory-path


#### コマンドラインから引数を入力されても上書きさせず、追加させる方法

override CFLAGS +=

とする

http://stackoverflow.com/questions/2129391/append-to-gnu-make-variables-via-command-line



#### エラーで止まらないようにするには

コマンドの先頭に-をつければよい。


#### ヘッダーファイルの依存関係を解決する方法

http://nu-pan.hatenablog.com/entry/20121205/1354693969

にやり方が書いてある


gcc -MMDを使う方法と-include を使う方法


## 参考

http://postd.cc/7-things-you-should-know-about-make/


### サンプル

これがなかなかいいサンプル

http://urin.github.io/posts/2013/simple-makefile-for-clang/



~~~
PROGNAME  := $(shell basename `readlink -f ..`)
PRODIR    := bin
OBJDIR    := lib
INCDIR    := include
SRCS      := $(wildcard *.c)
OBJS      := $(SRCS:%.c=%.o)
OBJSPATH  := $(addprefix $(OBJDIR)/, $(OBJS))
DEPENDS   := $(SRCS:%.c=%.d)
DEPENDSPATH   := $(addprefix $(OBJDIR)/, $(DEPENDS))

INCL      := -I$(INCDIR)/cspice/             -I$(INCDIR)/etc

LIBS      := -ansi -O2 -fPIC -DNON_UNIX_STDIO -lm cspice.a csupport.a

CC        := gcc
override CFLAGS    +=
LD        := gcc
override LDFLAGS   +=

.PHONY: all
all:$(PROGNAME)

-include $(DEPENDSPATH)

$(PROGNAME): $(OBJSPATH)
$(LD) $(LDFLAGS) $^ $(LIBS) -o $(PRODIR)/$(PROGNAME)

$(OBJDIR)/%.o: %.c
$(CC) -c $(CFLAGS) $(INCL) $< -o $@

.PHONY: clean
clean:
rm -f $(OBJSPATH)
rm -f $(DEPENDSPATH)
rm -f $(PRODIR)/$(PROGNAME)

# google test
TEST_PACKAGE  = $(PROGNAME)_test
TEST_SRC      = $(PROGNAME)_test.cc
TEST_PRODIR   = ../test/bin
TEST_OBJDIR   = ../test/lib
TEST_SUDIR    = ../test
TEST_DATADIR  = ../testdata
TEST_OBJ      = $(TEST_SRC:.cc=.o)
TEST_EXE      = $(TEST_SRC:.cc=)
TEST_EXEPATH  = $(addprefix $(TEST_PRODIR)/,$(TEST_EXE))
TEST_OBJPATH  = $(addprefix $(TEST_OBJDIR)/,$(TEST_OBJ))

GTEST_DIR     = /usr/local/gtest
GTEST_LIBS    = $(GTEST_DIR)/lib/libgtest_main.a $(GTEST_DIR)/lib/libgtest.a
GTEST_HEADERS = $(GTEST_DIR)/include
GCPPFLAGS     = -I../src -I$(GTEST_HEADERS)
GLDFLAGS      = -lpthread

MAKESHELL     = $(addsuffix ;, $(TEST_EXEPATH))

### google test ###

test: $(TEST_EXEPATH)

$(TEST_PRODIR)/%: $(TEST_OBJDIR)/%.o
g++ $(CFLAGS) -o $@ $^ $(LIBS) $(GTEST_LIBS) $(INCL) $(GLDFLAGS)

$(TEST_OBJDIR)/%.o: $(TEST_SUDIR)/%.cc

~~~

こっちもいい

http://postd.cc/makefile-c-projects/



~~~
TARGET ?= a.out
SRC_DIRS ?= ./src

SRCS := $(shell find $(SRC_DIRS) -name *.cpp -or -name *.c -or -name *.s)
OBJS := $(addsuffix .o,$(basename $(SRCS)))
DEPS := $(OBJS:.o=.d)

INC_DIRS := $(shell find $(SRC_DIRS) -type d)
INC_FLAGS := $(addprefix -I,$(INC_DIRS))

CPPFLAGS ?= $(INC_FLAGS) -MMD -MP

$(TARGET): $(OBJS)
$(CC) $(LDFLAGS) $(OBJS) -o $@ $(LOADLIBES) $(LDLIBS)

.PHONY: clean
clean:
$(RM) $(TARGET) $(OBJS) $(DEPS)

-include $(DEPS)
~~~

