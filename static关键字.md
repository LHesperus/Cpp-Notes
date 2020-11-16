---
title: static关键字
mathjax: true
tags:
  - 学习笔记
categories:
  - C/C++
date: 2020-7-24 15:57:00
---
### static关键字

#### 参考
* [C/C++ 中 static 的用法全局变量与局部变量](https://www.runoob.com/w3cnote/cpp-static-usage.html)


#### static 的引入  
在函数内部定义的变量，当程序执行到它的定义处时，编译器为它在栈上分配空间，函数在栈上分配的空间在此函数执行结束时会释放掉，这样就产生了一个问题: 如果想将函数中此变量的值保存至下一次调用时，如何实现？ 最容易想到的方法是定义为全局的变量，但定义一个**全局变量有许多缺点**，最明显的缺点是**破坏了此变量的访问范围**（使得在此函数中定义的变量，不仅仅只受此函数控制）。static 关键字则可以很好的解决这个问题。

另外，在 C++ 中，需要一个数据对象为整个类而非某个对象服务,同时又力求不破坏类的封装性,即要求此成员隐藏在类的内部，对外不可见时，可将其定义为静态数据。


**C++中作用**
（1）在修饰变量的时候，static 修饰的静态局部变量只执行初始化一次，而且延长了局部变量的生命周期，直到程序运行结束以后才释放。
（2）static 修饰全局变量的时候，这个全局变量只能在本文件中访问，**不能在其它文件中访问**，**即便是 extern 外部声明也不可以**。
（3）static 修饰一个函数，则这个函数的只能在本文件中调用，不能被其他文件调用。static 修饰的变量存放在全局数据区的静态变量区，包括全局静态变量和局部静态变量，都在全局数据区分配内存。初始化的时候自动初始化为 0。
（4）不想被释放的时候，可以使用static修饰。比如修饰函数中存放在栈空间的数组。如果不想让这个数组在函数调用结束释放可以使用 static 修饰。
（5）考虑到数据安全性（当程序想要使用全局变量的时候应该先考虑使用 static）。


**全局变量和全局静态变量的区别**

1）**全局变量是不显式用 static 修饰的全局变量**，全局变量默认是有外部链接性的，作用域是整个工程，在一个文件内定义的全局变量，在另一个文件中，通过 extern 全局变量名的声明，就可以使用全局变量。
2）全局静态变量是显式用 static 修饰的全局变量，作用域是声明此变量所在的文件，其他的文件即使用 extern 声明也不能使用。



**静态局部变量有以下特点：**
（1）该变量在全局数据区分配内存；
（2）静态局部变量在程序执行到该对象的声明处时被首次初始化，即以后的函数调用不再进行初始化；
（3）静态局部变量一般在声明处初始化，如果没有显式初始化，会被程序自动初始化为 0；
（4）它**始终驻留在全局数据区**，直到程序运行结束。但其作用域为**局部作用域**，当定义它的函数或语句块结束时，其作用域随之结束。
```C++
#include "static_test.h"
#include <iostream>
using namespace std;
int StaticTest::a = 0;//必须初始化
int StaticTest::c = 0;//必须初始化

void StaticTest::func1(void)
{
	cout<<"func1 a: " << a++ << endl;
	cout<<"func1 b: " << b++ << endl;
	cout<<"func1 c: " << c++ << endl;
    cout << "func1->";
	func2();//类的非静态成员函数可以调用用静态成员函数，但反之不能。
}

void StaticTest::func2(void)
{
	cout << "func2 a: " << a++ << endl;
	//cout << "func2 b: " << b++ << endl;//static修饰的函数不能访问非static变量
	//cout << "func2 c: " << c++ << endl;//static修饰的函数不能访问非static变量
	//fun1();///类的非静态成员函数可以调用用静态成员函数，但反之不能。
}

void StaticTest::func3(void)
{
	int aa = 0;
	static int bb = 0;
	cout << "func3 aa : " << aa++ << endl;
	cout << "func3 bb : " << bb++ << endl;
}

```
```C++
#ifndef STATIC_TEST_H
#define STATIC_TEST_H


class StaticTest
{
public:
	StaticTest() :/*a(0),//错误*/b(0) { }
	void func1(void);
	static void func2(void);
    void func3(void);

	static int c;
private:
	static int a;
	int b;
};

#endif // !STATIC_TEST_H

```
```C++
#include"static_test.h"
int main()
{
    StaticTest ST;
    ST.func1();
    ST.func1();
    ST.func1();
    //ST.c = 1;//错误，文件外不能访问
    //StaticTest::c = 1;//错误，文件外不能访问
    ST.func2();
    ST.func2();
    ST.func2();
    ST.func3();
    ST.func3();
    ST.func3();
   // StaticTest::func1();//错误，不能通过类名来调用类的非静态成员函数。类的对象可以使用静态成员函数和非静态成员函数
    StaticTest::func2();
    return 0;
}
```

**静态成员函数**

（1）静态成员函数和静态数据成员一样，他们都属于类的静态成员，而不是对象成员。
（2）非静态成员函数有 this 指针，**而静态成员函数没有 this 指针**。
（3）静态成员函数主要用来方位静态数据成员而不能访问非静态成员。