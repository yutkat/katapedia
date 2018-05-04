# Assert


~~~
#ifdef DEBUG
#define ASSERT(C) DoAssert_((C),__FILE__,__LINE__)
#else
#define ASSERT(C)
#endif


#ifdef DEBUG
void DoAssert_( bool bCond, const char *pcFileName, int nLine )
{
if ( bCond ) {
;
}
else {
cout << "Assertion failed at " << pcFileName
<< "(" << nLine << ")" << endl;
ABORT();
}
}
#endif
~~~


