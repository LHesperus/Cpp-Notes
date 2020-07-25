### extern 

```C
//h文件
#ifndef EXTERNC_TEST_H
#define EXTERNC_TEST_H

#ifdef __cplusplus
extern "C" {
#endif // __cplusplus
int func1(int x, int y);
int func2();
#ifdef __cplusplus
}
#endif // __cplusplus

int func3();

#endif // !EXTERNC_TEST_H
```

```C
//C文件
#include"externC_test.h"


int func1(int x,int y)
{
	return x + y;
}

int func2()
{
	return 2;
}
int func3()
{
	return 3;
}

```
```C
//cpp文件
int main()
{
    cout << func1(1,2) << endl;
    cout << func2() << endl;
  //  cout << func3() << endl;//错误
    return 0;
```
**如果运行func3():**
```
1>------ 已启动生成: 项目: test, 配置: Debug Win32 ------
1>main.cpp
1>Debug\static_test.obj : warning LNK4042: 对象被多次指定；已忽略多余的指定
1>Debug\this_test.obj : warning LNK4042: 对象被多次指定；已忽略多余的指定
1>main.obj : error LNK2019: 无法解析的外部符号 "int __cdecl func3(void)" (?func3@@YAHXZ)，函数 _main 中引用了该符号
1>  已定义且可能匹配的符号上的提示:
1>    "public: void __thiscall StaticTest::func3(void)" (?func3@StaticTest@@QAEXXZ)
1>    _func3
1>F:\Cpptest\test\Debug\test.exe : fatal error LNK1120: 1 个无法解析的外部命令
```