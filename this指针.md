---
title: this指针
mathjax: true
tags:
  - 学习笔记
categories:
  - C/C++
date: 2020-7-25 15:23:00
---
### this指针
```C
#ifndef THIS_TEST_H
#define THIS_TEST_H

class ThisTest
{
public:
	void func1(int a,int b);
	static void func2();
private:
	int a;
};
#endif // !THIS_TEST_H
```
```C
#include "this_test.h"
#include<iostream>
#include<complex>
using namespace std;


void ThisTest::func1(int a, int b)
{
	cout << this << endl;
	cout << this->a << endl;
	this->a = a;//同名用this
	cout << this->a << endl;
	a = b;
	cout << a << endl;
	cout << (*this).a << endl;
}

void ThisTest::func2()
{
	//cout << this->a << endl;//错误：this只能作用与非静态成员函数的内部
}

```