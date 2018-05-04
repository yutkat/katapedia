# Staticまとめ


==========source==========

~~~
#include --

namespace test{

const int Test::aaa = 0;  // 実体がソース側にある定数
int Test::ccc = 0;           // 初期化

const int ddd = 2;          // externで参照できる変数
static const int eee = 3; // externで参照できない(そのファイルからしか呼び出せない変数)

int hhh = 4;


Test::Test(): constVar(0) // fffを初期化
{
}


}
~~~


==========header==========

~~~
#include ---


namespace test{

static int ggg = 5; // (オブジェクトファイル単位で同じアドレスとなる)
extern int hhh;     // プロセス内で一意

class Test

public:


private:
static const int aaa;　// 全インスタンスで同じ値になる(プロセス内で同じアドレスとなる)

const int bbb = 1;     // 実体がヘッダー側にある定数
static int ccc;          // クラスのメンバ変数で、値をオブジェクトごとに持つのではなく、すべてのオブジェクトで同じ変数を参照したい(プロセス内で同じアドレスとなる)
const int fff'            // ソースに実体を持たせる。コンストラクタで初期化

}
~~~

