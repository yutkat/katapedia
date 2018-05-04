# Googletest

* 例外
<notextile>

{ASSERT|EXPECT}_THROW(statement,expected_exception)     statementがexpected_exceptionをthrowする

{ASSERT|EXPECT}_NO_THROW(statement)     statementはいかなる例外もthrowしない

{ASSERT|EXPECT}_ANY_THROW(statement)    statementは何らかの例外をthrowする

</notextile>

* 整数の比較
<notextile>

{ASSERT|EXPECT}_EQ(expected,actual)     expected == actual

{ASSERT|EXPECT}_NE(v1,v2)       v1 == v2

{ASSERT|EXPECT}_LT(v1,v2)       v1 < v2

{ASSERT|EXPECT}_LE(v1,v2)       v1 <= v2

{ASSERT|EXPECT}_GT(v1,v2)       v1 > v2

{ASSERT|EXPECT}_GE(v1,v2)       v1 >= v2

</notextile>

* 実数の比較
<notextile>

{ASSERT|EXPECT}_FLOAT_EQ(f1,f2) f1 == f2（floatの比較）

{ASSERT|EXPECT}_DOUBLE_EQ(d1,d2)        d1 == d2（doubleの比較）

{ASSERT|EXPECT}_NEAR(r1,r2,err) |r1 - r2| < err

</notextile>

* 文字列の比較
<notextile>

{ASSERT|EXPECT}_STREQ(s1,s2)    s1 と s2 は等しい

{ASSERT|EXPECT}_STRNE(s1,s2)    s1 と s2 は等しくない

{ASSERT|EXPECT}_STRCASEEQ(s1,s2)        s1 と s2 は等しい（大文字／小文字を区別しない）

{ASSERT|EXPECT}_STRCASENE(s1,s2)        s1 と s2 は等しくない（大文字／小文字を区別しない）

</notextile>


## 中級

### 共通処理

テストの前処理などを書きたいときに


~~~
class TestFix : public ::testing::Test
{
protected:
std::vector<int> data;

virtual void SetUp()
{
printf("TestFix::SetUpn");
data.push_back(0);
data.push_back(1);
data.push_back(2);
}
virtual void TearDown()
{
printf("TestFix::TearDownn");
}
}:
TEST_F(TestFix, Test1)
{
data.push_back(3);  // TestFix のメンバーにアクセス可能
TEST_ASSERT_EQ(3, data.size());
}
TEST_F(TestFix, Test2)
{
TEST_ASSERT_EQ(3, data.size());
}
~~~

http://srz-zumix.blogspot.jp/2012/01/google-test.html



### パターン化

// template 引数にはパラメータの型を渡す

class TestP : public ::testing::TestWithParam<int> {};


~~~
TEST_P(TestP, TestA)
{
int param = GetParam(); // パラメータは GetParam 関数で取得できる。
printf("%dn", param);
}
~~~

~~~
INSTANTIATE_TEST_CASE_P(InstantiationName, TestP
, ::testing::Values(1, 3, 0)); // ここがパラメータ
~~~

http://srz-zumix.blogspot.jp/2012/01/google-test.html



### インストール

yum install libstdc++-devel

