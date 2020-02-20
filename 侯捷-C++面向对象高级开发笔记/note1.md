# 侯捷老师讲C++
&ensp;&ensp;&ensp; &ensp;此文档为本人边看视频边记的笔记，方便以后自己回看笔记时能快速想起课程内容，详细内容请看老师讲的视频。<br>
&ensp;&ensp;&ensp; &ensp;课程来源:[GeekBand](https://study.163.com/course/introduction/1634002.htm),也可以在b站搜到.

如果不能显示出图，请看Github的博文。

## 1、C++ 编程简介

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

## 2、头文件与类的声明
C vs C++ <br>
![C vs. C++](https://github.com/LHesperus/Cpp-Notes/blob/master/%E4%BE%AF%E6%8D%B7-C%2B%2B%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E9%AB%98%E7%BA%A7%E5%BC%80%E5%8F%91%E7%AC%94%E8%AE%B0/pic/2-1.png)

C语言数据和函数是分离的，数据都是全局的。这样编程有很大影响。后来发展的C++是数据和函数合在一起成为class，C++的struct几乎等同于class,只有微小的差别。

前面讲过的class可以分为带指针和不带指针的，典型的例子就是complex(不带指针)和string(带指针)


* Object Based : 面對的是單一class 的設計
* Object Oriented : 面對的是多重classes 的設計，
classes 和classes 之間的關係。

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
    complex& operator += (const complex&);
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
