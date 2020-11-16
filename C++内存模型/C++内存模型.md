---
title: C++ 内存模型
mathjax: true
tags:
  - 学习笔记
categories:
  - C/C++
date: 2020-7-24 15:09:00
---
# C++ 内存模型

## 参考资料
* [C/C++程序内存的分配](https://blog.csdn.net/cherrydreamsover/article/details/81627855)
* [深入理解计算机系统]()


#### 一个C/C++编译的程序占用内存分为以下几个部分：
* 栈区（stack）：由编译器自动分配与释放，存放为运行时函数分配的局部变量、函数参数、返回数据、返回地址等。其操作类似于数据结构中的栈。
* 堆区（heap）：一般由程序员自动分配，如果程序员没有释放，程序结束时可能有OS回收。其分配类似于链表。
* 全局区（静态区static）：存放全局变量、静态数据、常量。程序结束后由系统释放。全局区分为已初始化全局区（data）和未初始化全局区（bss）。
* 常量区（文字常量区）：存放常量字符串，程序结束后有系统释放。
* 代码区：存放函数体（类成员函数和全局区）的二进制代码。

#### 三种内存分配方式
* **从静态存储区分配** ：内存在程序编译的时候已经分配好，这块内存在程序的整个运行期间都存在。例如全局变量，static变量。
* **在栈上创建**： 在执行函数时，函数内局部变量的存储单元可以在栈上创建，函数执行结束时，这些内存单元会自动被释放。栈内存分配运算内置于处理器的指令集，效率高，但是分配的内存容量有限。
* **从堆上分配**：亦称为动态内存分配。程序在运行的时候使用malloc或者new申请任意多少的内存，程序员自己负责在何时用free或delete释放内存。
动态内存的生命周期有程序员决定，使用非常灵活，但如果在堆上分配了空间，既有责任回收它，否则运行的程序会出现内存泄漏，频繁的分配和释放不同大小的堆空间将会产生内存碎片。
#### 简图
![](https://lcg-pic-tencent-1258286866.cos.ap-chengdu.myqcloud.com/github/C%2B%2B%20Notes/C%2B%2B%E5%86%85%E5%AD%98%E6%A8%A1%E5%9E%8B/1.png)

在 C 语言中，全局变量又分为初始化的和未初始化的（未被初始化的对象存储区可以通过 void* 来访问和操纵，程序结束后由系统自行释放），在 C++ 里面没有这个区分了，他们共同占用同一块内存区。
![](https://lcg-pic-tencent-1258286866.cos.ap-chengdu.myqcloud.com/github/C%2B%2B%20Notes/C%2B%2B%E5%86%85%E5%AD%98%E6%A8%A1%E5%9E%8B/2.png)


#### 
* **管理方式不同**：栈是由编译器自动申请和释放空间，堆是需要程序员手动申请和释放；
* **空间大小不同**：栈的空间是有限的，在32位平台下，VC6下默认为1M，堆最大可以到4G；
* **能否产生碎片**：栈和数据结构中的栈原理相同，在弹出一个元素之前，上一个已经弹出了，不会产生碎片，如果不停地调用malloc、free对造成内存碎片很多；
生长方向不同：堆生长方向是向上的，也就是向着内存地址增加的方向，栈刚好相反，向着内存减小的方向生长。
* **分配方式不同**：堆都是动态分配的，没有静态分配的堆。栈有静态分配和动态分配。静态分配是编译器完成的，比如局部变量的分配。动态分配由 malloc 函数进行分配，但是栈的动态分配和堆是不同的，它的动态分配是由编译器进行释放，无需我们手工实现。
* **分配效率不同**：栈的效率比堆高很多。栈是机器系统提供的数据结构，计算机在底层提供栈的支持，分配专门的寄存器来存放栈的地址，压栈出栈都有相应的指令，因此比较快。堆是由库函数提供的，机制很复杂，库函数会按照一定的算法进行搜索内存，因此比较慢。

**64位Ubnutu下测试**
```c++
#include<stdio.h>
#include<stdlib.h>
#include<iostream>
using namespace std;
void test()
{
  int i=0;
  cout<<"func() int:  "<<&i<<endl;
  int *j=new int;
  cout<<"func() new int:  "<<j<<endl;
  delete j;
}
int global=10;
int *gp=new int;
int main()
{
    int s=sizeof(int);
    int s1=sizeof(long);
    printf("%d\n",s);
    printf("%d\n",s1);
    printf("hello\n");
    int i=0;
    int *j=new int;
    int *k=new int;
    int *ii=&i;
    int *iii=new int;
    iii=&i;
    int *a=(int*)malloc(sizeof(int)*10);
    int *p=NULL;
    const static int x=1;
    cout<<"& int: "<<&i<<endl;
    cout<<"new int:  "<<j<<endl;
    cout<<"new int:   "<<k<<endl;
    cout<<"malloc int:  "<<a<<endl;
     cout<<"int*:   "<<p<<endl;
    cout<<"int*=&int:   "<<ii<<endl;
    cout<<"new int*=&int:   "<<iii<<endl;
    cout<<"const static int :"<<&x<<endl;
    cout<<"global int"<<&global<<endl;
    cout<<"global new int*"<<gp<<endl;
    test();
    delete j;
    delete k;
    free(a);
    delete gp;
    return 0;
}
```
```
4
8
hello
& int: 0x7ffff146db8c
new int:  0x1156050
new int:   0x1156070
malloc int:  0x11560b0
int*:   0
int*=&int:   0x7ffff146db8c
new int*=&int:   0x7ffff146db8c
const static int :0x400fd4
global int0x6020a0
global new int*0x1155c20
func() int:  0x7ffff146db5c
func() new int:  0x11560e0

```
windows下测试结果不同

参考[堆、栈的地址高低？ 栈的增长方向？ - RednaxelaFX的回答 - 知乎
](https://www.zhihu.com/question/36103513/answer/66101372)