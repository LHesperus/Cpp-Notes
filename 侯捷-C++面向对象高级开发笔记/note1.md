

## 简介
&ensp;&ensp;&ensp; &ensp;此文档为本人边看视频边记的笔记，方便以后自己回看笔记时能快速想起课程内容，详细内容请看老师讲的视频。<br>
&ensp;&ensp;&ensp; &ensp;课程来源:[GeekBand](https://study.163.com/course/introduction/1634002.htm),也可以在b站搜到.

如果不能显示出图，请看Github的博文。

## C++ 编程简介

基于对象(Object Based)
* class without pointer members -Complex
* class with pointer members -String

面向对象(Object Oriented)
* 学习Classes之间的关系
  * 继承(inheritance)
  * 复合(composition)
  * 委托(delegation) 

### C++历史
C语言(1972)
C++(1983)早期叫C with Class

C++有很多版本,如 C++98,C++03,C++ 11,C++14

学习C++(其他语言也是)可以分为两个部分：
* C++语言
* C++标准库

经典的书：
* C++ Primer (C++第一个编译器的作者写的)
* C++ Programming Language (C++之父写的)
* Effective C++ (专家写的，以条款方式写的)
* The C++ Standard Library(写的C++的标准库)
* STL源码剖析(较深入的将标准库)

## 头文件与类的声明
C vs C++ <br>
![C vs. C++](https://github.com/LHesperus/Cpp-Notes/blob/master/%E4%BE%AF%E6%8D%B7-C%2B%2B%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E9%AB%98%E7%BA%A7%E5%BC%80%E5%8F%91%E7%AC%94%E8%AE%B0/pic/2-1.png)

C语言数据和函数是分离的，数据都是全局的。这样编程有很大影响。后来发展的C++是数据和函数合在一起成为class，C++的struct几乎等同于class,只有微小的差别。

前面讲过的class可以分为带指针和不带指针的，典型的例子就是complex(不带指针)和string(带指针)


* Object Based : 面对的是单一class 的设计
* Object Oriented : 面对的是多重classes 的设计，classes 和classes 之间的关系。

### C++的代码基本形式

![头文件](https://github.com/LHesperus/Cpp-Notes/blob/master/%E4%BE%AF%E6%8D%B7-C%2B%2B%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E9%AB%98%E7%BA%A7%E5%BC%80%E5%8F%91%E7%AC%94%E8%AE%B0/pic/2-2.png)

注意头文件：<>对应标准库，“”对应自己写的东西

### 头文件中的防卫式声明(guard)

```cpp
#ifndef __COMPLEX__
#define __COMPLEX__
...
#endif
```
可以避免包含头文件时的顺序问题。

### 头文件的布局
```cpp
#ifndef __COMPLEX__
#define __COMPLEX__
// forward declarations：前置声明
#include <cmath>
class ostream;
class complex;
complex&
__doapl (complex* ths, const complex& r);
// 类：声明
class complex
{ 
    ...
};
//类：定义
complex::function ...
#endif
```
#### class的声明

``` cpp
class complex//class head
{ 
    //class body
    //有些函數在此直接定义，另一些在body 之外定义
public:
    complex (double r = 0, double i = 0)
        : re (r), im (i)
    { }
    complex& operator += (const complex&);//声明
    double real () const { return re; }
    double imag () const { return im; }
private:
    double re, im;
    friend complex& __doapl (complex*, const complex&);
};
```
使用时
```cpp
{
    complex c1(2,1);
    complex c2;
    ...
}
```
上述代码中复数只有double的实部和虚部，为了可以支持多种类型并且不增加类，我们引入模板。
```cpp
template<typename T>//T也可以是其他字母
class complex
{ 
public:
    complex (T r = 0, T i = 0)
        : re (r), im (i)
    { }
    complex& operator += (const complex&);
    T real () const { return re; }
    T imag () const { return im; }
private:
    T re, im;
    friend complex& __doapl (complex*, const complex&);
};
```
使用时:
```cpp
{
    complex<double> c1(2.5,1.5);//T指定为double
    complex<int> c2(2,6);//T指定为int
    ...
}
```


## 内敛函数、访问级别、构造函数
### inline(内联)函数
```cpp
class complex
{ 
public:
    complex (double r = 0, double i = 0)
        : re (r), im (i)
    { }
    complex& operator += (const complex&);
    double real () const { return re; }
    double imag () const { return im; }
private:
    double re, im;

    friend complex& __doapl (complex*, const complex&);
};
```

函数在class本体里定义就形成了inline，inline像宏一样，函数如果是inline的，就会很快，所以所有的函数都写成inline是最好的，然而编译器没有能力把所有函数都做成inline，一般来说程序复杂，编译器就不能做成inline。inline function最后能不能做成inline看编译器的能力。

不在本体里定义inline的要加inline：
```cpp
inline double
imag(const complex& x)
{
    return x.imag ();
}
```

### 访问级别(access level)
* public ：外部能看到的
* private：只有自己能看到的，一般只有内部函数处理的数据要用private
* protect：受保护的

错误示范：
```cpp
{
    complex c1(2,1);
    cout << c1.re;
    cout << c1.im;
}
```
正确示范：
```cpp
{
    complex c1(2,1);
    cout << c1.real();
    cout << c1.imag();
}
```

### 构造函数(construct，ctor)

如果创建一个对象，构造函数会自动调用起来。没有程序里直接调用函数的写法。
如以下三种方法创建对象：
```cpp
{
    complex c1(2,1);
    complex c2;
    complex* p = new complex(4);
}
```
则会自动调用之前写的代码：
```cpp
    complex (double r = 0, double i = 0)//0为默认实参(default argument)
        : re (r), im (i)//初值列，初始列，initialization  list
    { }
```

构造函数的写法很独特，他的函数名称一定要和类的名称相同。
构造函数没有返回值类型，因为不需要有，因为构造函数就是要创建类的。
注意默认值并不是构造函数特性，其他函数也可以写成默认值。
initialization  list是只有构造函数才有的写法。
如上面的代码也可以写成：
```cpp
complex (double r = 0, double i = 0)
{ re = r; im = i; }//赋值
```
两种写法结果是一样的，但是一般有过良好编程习惯的会写成第一种。
第一种效率会好一些，背后有很多原理，简单来讲可以说是因为变量数值的设定有两个阶段，一个是初始化，一个是赋值。第二种写法就是跳过了初始化，效果差。

相对于构造函数有一个析构函数，不带指针的类多半不用写析构函数，如上述所写的Complex。

### 重载 overloading
构造函数可以有很多个重载。
重载：同名的函数名称有很多个以上。重载常常发生在构造函数身上。

![2-3](https://github.com/LHesperus/Cpp-Notes/blob/master/%E4%BE%AF%E6%8D%B7-C%2B%2B%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E9%AB%98%E7%BA%A7%E5%BC%80%E5%8F%91%E7%AC%94%E8%AE%B0/pic/2-3.png)

图中的两个构造函数是不能同时存在的，因为在创建对象时，无法知道应该调用哪个构造函数。

同名的函数在编译器处理后会变成其他名字。这个名字是给机器看的。编译器会把函数的名称，参数有几个，参数是什么类型编码，机器会区分出同名的不同函数。

构造函数也可以放在private里，这是不能按照之前的方法创建对象。详细的使用方法这里先不讲。但要知道有这种写法。
如设计模式Singleton：
```cpp
class A {
public:
    static A& getInstance();
    setup() { ... }
private:
    A();
    A(const A& rhs);
    ...
};
A& A::getInstance()
{
    static A a;
    return a;
}
```
```cpp
A::getInstance().setup();
```

## 参数传递与返回
### 定义函数是确定是否添加const
 ```cpp
 class complex
{ 
public:
    complex (double r = 0, double i = 0)
    : re (r), im (i)
    { }
    complex& operator += (const complex&);
    double real () const { return re; }
    double imag () const { return im; }
private:
    double re, im;
    friend complex& __doapl (complex*, const complex&);
};
 ```
 注意
 ```cpp
 double real () const { return re; }
 ```
 函数中的const是十分重要的，很多人会忽略。
 const在此处的表示函数只是将数据取出，并不改变数据内容。

 __class的函数可以分为会改变函数内容的和不会改变函数内容的。不会改变数据内容的函数要加加const__。
开发人员在设计接口时就要确定是否要加const。
 
 如果不加会产生什么后果：
 ```cpp
 {
    const complex c1(2,1);
    cout << c1.real();
    cout << c1.imag();
}
```
上面的const说明使用者复数中的2，1是不能变的，假如定义时不加const，说明这个函数可能会改数据，这样在定义和调用就发生了冲突。

### 参数传递(pass by value vs. pass by reference(to const))

```cpp
class complex
{ 
public:
    complex (double r = 0, double i = 0)
        : re (r), im (i)
    { }
    complex& operator += (const complex&);//pass by reference
    double real () const { return re; }
    double imag () const { return im; }
private:
    double re, im;
    friend complex& __doapl (complex*, const complex&);
};
```
* by value :是数据整包传过去，数据有几个字节就传几个字节
* by reference(引用)：带&,传引用相当与传指针的速度。
* by reference(to const):没有const时，传给对方的数据，对方可能修改后影响我，如果不想自己受到影响，就要加上const，表示此引用不会该改变数据，对方改变就会出错。

__良好的习惯：参数传递尽量的传引用，速度快__

### 返回值传递(return by value vs. return by reference(to const))
上述代码中
```cpp
complex& operator += (const complex&);//return by reference
double real () const { return re; }//return by value
```
### 友元 friend
```cpp
inline complex&
__doapl (complex* ths, const complex& r)
{
    ths->re += r.re;//自由取得friend 的private成员。
    ths->im += r.im;
    return *ths;
}
```

相同class的各个object互为friends

![4-1](https://github.com/LHesperus/Cpp-Notes/blob/master/%E4%BE%AF%E6%8D%B7-C%2B%2B%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E9%AB%98%E7%BA%A7%E5%BC%80%E5%8F%91%E7%AC%94%E8%AE%B0/pic/4-1.png)

### 使用场景

看下面的例子do assignment plus(__doapl)
```cpp
inline complex&
__doapl(complex* ths, const complex& r)
{
    ths->re += r.re;
    ths->im += r.im;
    return *ths;
}
inline complex&
complex::operator += (const complex& r)
{
    return __doapl (this, r);
}
```
上述代码中的第一个参数(ths)会发生改动,则不能加const，第二个参数(r)不会发生改动,可以加const。

在函数里面创建的东西不能加引用往外传，因为那是局部的，传到外面就消失了，其他情况都可以传引用(inline complex&)。

## 操作符重载(operator overloading)

C++中操作符其实就是一种函数，也可以重载。 
### 成员函数重载

所有的成员函数都带有一个隐藏的参数(this)，谁调用，谁就是this。this是一个指针，下面是一个复数+=运算的例子。

![5-1](https://github.com/LHesperus/Cpp-Notes/blob/master/%E4%BE%AF%E6%8D%B7-C%2B%2B%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E9%AB%98%E7%BA%A7%E5%BC%80%E5%8F%91%E7%AC%94%E8%AE%B0/pic/5-1.png)

__传递者无需知道接收者是以reference形式接收__,上述代码中编写
return *ths;时不必考虑返回的是以什么形式接收的。

此外，complex::operator +=的返回 complex&，也可以返回void，这时执行，c2+=c1没有问题，但是如果连续赋值c3+=c2+=c1;就会有问题，所以严谨写法为返回complex&。

### 非成员函数重载(无this)

```cpp
inline complex
operator + (const complex& x, const complex& y)//复数加复数
{
    return complex (real (x) + real (y),
    imag (x) + imag (y));
}
inline complex
operator + (const complex& x, double y)//复数+实数
{
    return complex (real (x) + y, imag (x));
}
inline complex
operator + (double x, const complex& y)//实数+复数
{
    return complex (x + real (y), imag (y));
}
```
注意上述代码不能return by reference，以为他们的返回必定是local object。

complex()的语法是，temp object(临时对象)， typename();

```cpp
#include <iostream.h>
ostream&
operator << (ostream& os, const complex& x)
{
    return os << '(' << real (x) << ','
              << imag (x) << ')';
}
```
cout是一种对象，ostream.