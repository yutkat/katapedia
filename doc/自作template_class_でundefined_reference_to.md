# 自作template class でundefined reference to



.cppにて使用されｔる型を明示するか、.hに直接記述するかすること




> “Undefined reference to” template class constructor

> up vote

> 12

> down vote

> favorite

> 11

> I have no idea why this is happenning, since I think I have everything properly declared and defined.

>

> I have the following program, designed with templates. It's a simple implementation of a queue, with the member functions "add", "substract" and "print".

>

> I have defined the node for the queue in the fine "nodo_colaypila.h":

> <pre><code class="cpp">

#ifndef NODO_COLAYPILA_H

#define NODO_COLAYPILA_H


#include <iostream>


template <class T> class cola;


template <class T> class nodo_colaypila

{

T elem;

nodo_colaypila<T>* sig;

friend class cola<T>;

public:

nodo_colaypila(T, nodo_colaypila<T>*);


};

</code></pre>

> Then the implementation in "nodo_colaypila.cpp"


> <pre><code class="cpp">

#include "nodo_colaypila.h"

#include <iostream>


template <class T> nodo_colaypila<T>::nodo_colaypila(T a, nodo_colaypila<T>* siguiente = NULL)

{

elem = a;

sig = siguiente;//ctor

}

</code></pre>

> Afterwards, the definition and declaration of the queue template class and its functions:


> "cola.h":


> <pre><code class="cpp">

#ifndef COLA_H

#define COLA_H


#include "nodo_colaypila.h"


template <class T> class cola

{

nodo_colaypila<T>* ult, pri;

public:

cola<T>();

void anade(T&);

T saca();

void print() const;

virtual ~cola();


};



#endif // COLA_H

</code></pre>


> "cola.cpp":

> <pre><code class="cpp">

#include "cola.h"

#include "nodo_colaypila.h"


#include <iostream>


using namespace std;


template <class T> cola<T>::cola()

{

pri = NULL;

ult = NULL;//ctor

}


template <class T> void cola<T>::anade(T& valor)

{

nodo_colaypila <T> * nuevo;


if (ult)

{

nuevo = new nodo_colaypila<T> (valor);

ult->sig = nuevo;

ult = nuevo;

}

if (!pri)

{

pri = nuevo;

}

}


template <class T> T cola<T>::saca()

{

nodo_colaypila <T> * aux;

T valor;


aux = pri;

if (!aux)

{

return 0;

}

pri = aux->sig;

valor = aux->elem;

delete aux;

if(!pri)

{

ult = NULL;

}

return valor;

}


template <class T> cola<T>::~cola()

{

while(pri)

{

saca();

}//dtor

}


template <class T> void cola<T>::print() const

{

nodo_colaypila <T> * aux;

aux = pri;

while(aux)

{

cout << aux->elem << endl;

aux = aux->sig;

}

}

</code></pre>


> Then, I have a program to test these functions as follows:


> "main.cpp"


> <pre><code class="cpp">

#include <iostream>

#include "cola.h"

#include "nodo_colaypila.h"


using namespace std;


int main()

{

float a, b, c;

string d, e, f;

cola<float> flo;

cola<string> str;


a = 3.14;

b = 2.71;

c = 6.02;

flo.anade(a);

flo.anade(b);

flo.anade(c);

flo.print();

cout << endl;


d = "John";

e = "Mark";

f = "Matthew";

str.anade(d);

str.anade(e);

str.anade(f);

cout << endl;


c = flo.saca();

cout << "First In First Out Float: " << c << endl;

cout << endl;


f = str.saca();

cout << "First In First Out String: " << f << endl;

cout << endl;


flo.print();

cout << endl;

str.print();


cout << "Hello world!" << endl;

return 0;

}

</code></pre>

> But when I build, the compiler throws errors in every instance of the template class:

>

> undefined reference to `cola(float)::cola()'... (it's actually cola'<'float'>'::cola(), but this doesn't let me use it like that.)

>

> And so on. Altogether, 17 warnings, counting the ones for the member functions being called in the program.

>

> Why is this? Those functions and constructors WERE defined. I thought that the compiler could replace the "T" in the template with "float", "string" or whatever; that was the advantage of using templates.

>

> I read somewhere here that I should put the declaration of each function in the header file for some reason. Is that right? And if so, why?

>

> Thanks in advance.

>

> This is a common question in C++ programming. There are two valid answers to this. There are advantages and disadvantages to both answers and your choice will depend on context. The common answer is to put all the implementation in the header file, but there's another approach will will be suitable in some cases. The choice is yours.

>

> The code in a template is merely a 'pattern' known to the compiler. The compiler won't compile the constructors cola<float>::cola(...) and cola<string>::cola(...) until it is forced to do so. And we must ensure that this compilation happens for the constructors at least once in the entire compilation process, or we will get the 'undefined reference' error. (This applies to the other methods of cola<T> also.)

>

> Understanding the problem

>

> The problem is caused by the fact that main.cpp and cola.cpp will be compiled separately first. In main.cpp, the compiler will implicitly instantiate the template classes cola<float> and cola<string> because those particular instantiations are used in main.cpp. The bad news is that the implementations of those member functions are not in main.cpp, nor in any header file included in main.cpp, and therefore the compiler can't include complete versions of those functions in main.o. When compiling cola.cpp, the compiler won't compile those instantiations either, because there are no implicit or explicit instantiations of cola<float> or cola<string>. Remember, when compiling cola.cpp, the compiler has no clue which instantiations will be needed; and we can't expect it to compile for every type in order to ensure this problem never happens! (cola<int>, cola<char>, cola<ostream>, cola< cola<int> > ... and so on ...)

>

> The two answers are:

>

> Tell the compiler, at the end of cola.cpp, which particular template classes will be required, forcing it to compile cola<float> and cola<string>.

> Put the implementation of the member functions in a header file that will be included every time any other 'translation unit' (such as main.cpp) uses the template class.

> Answer 1: Explicitly instantiate the template, and its member definitions

>

> At the end of cola.cpp, you should add lines explicitly instantiating all the relevant templates, such as

>

> template class cola<float>;

> template class cola<string>;

> and you add the following two lines at the end of nodo_colaypila.cpp:

>

> template class nodo_colaypila<float>;

> template class nodo_colaypila<std :: string>;

> This will ensure that, when the compiler is compiling cola.cpp that it will explicitly compile all the code for the cola<float> and cola<string> classes. Similarly, nodo_colaypila.cpp contains the implementations of the nodo_colaypila<...> classes.

>

> In this approach, you should ensure that all the of the implementation is placed into one .cpp file (i.e. one translation unit) and that the explicit instantation is placed after the definition of all the functions (i.e. at the end of the file).

>

> Answer 2: Copy the code into the relevant header file

>

> The common answer is to move all the code from the implementation files cola.cpp and nodo_colaypila.cpp into cola.h and nodo_colaypila.h. In the long run, this is more flexible as it means you can use extra instantiations (e.g. cola<char>) without any more work. But it could mean the same functions are compiled many times, once in each translation unit. This is not a big problem, as the linker will correctly ignore the duplicate implementations. But it might slow down the compilation a little.

>

> Summary

>

> The default answer, used by the STL for example and in most of the code that any of us will write, is to put all the implementations in the header files. But in a more private project, you will have more knowledge and control of which particular template classes will be instantiated. In fact, this 'bug' might be seen as a feature, as it stops users of your code from accidentally using instantiations you have not tested for or planned for ("I know this works for cola<float> and cola<string>, if you want to use something else, tell me first and will can verify it works before enabling it.").

>

> Finally, there are three other minor typos in the code in your question:

>

> You are missing an #endif at the end of nodo_colaypila.h

> in cola.h nodo_colaypila<T>* ult, pri; should be nodo_colaypila<T> *ult, *pri; - both are pointers.

> nodo_colaypila.cpp: The default parameter should be in the header file nodo_colaypila.h, not in this implementation file.

> share|improve this answer



