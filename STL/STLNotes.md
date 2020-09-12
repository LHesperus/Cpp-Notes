### 参考
[http://www.cplusplus.com/reference/stl/](http://www.cplusplus.com/reference/stl/)
### 分类
* 顺序容器
* 关联容器
    + 有序容器（红黑树）
    + 无序容器（哈希表）
### 通用接口
#### 类型别名
#####  iterator
##### const_iterator
##### size_type
##### difference_type
##### value_type
##### reference
##### const_reference
#### 构造函数
#### 赋值
##### assign(仅顺序容器)
```cpp
a.assign(it1,it2);//给定迭代器范围进行赋值
a.assign({1,2,3,4});//列表赋值
a.assign(n,t);//n个t
```
assign允许从一个不同但相容的类型赋值。
```cpp
   list<string> names;
    vector<const char*> old;
    char* tmp = new char[3];
    tmp[0] = '7';
    tmp[1] = '8';
    tmp[2] = '\0';
    char tmp2[4] = "abc";
    //names.push_back("123");
    //names.push_back("456");
    old.push_back(tmp);
    old.push_back(tmp2);

    names.assign(old.cbegin(), old.cend());
    for (auto i : names) cout << i << " ";//78 abc
    cout << endl;
```
array的assign不能按上述用，a.assign(1)是把assign所有位置都变成1.
##### swap
常数时间交换两个相同容器的内容。元素本身并未交换，只是交换了数据结构(比如迭代器的指向)
但是array特殊，相当于数组。会真正交换其元素。也不是常数时间。

对`string`调用swap会导致迭代器、引用和指针失效。
```cpp
    string s1("123");
    string s2("456");
    string::iterator it0 = s1.begin();
    swap(s1, s2);
    //s1.swap(s2);
    auto it1 = s1.begin();
    cout << *it0 << endl;//迭代器失效
    cout << *it1 << endl;//4
    cout << s1 << endl;//456
    cout << s2 << endl;//123
```
```cpp

    vector<int> v1 = {1,2,3};
    vector<int> v2 = {4,5,6,7};

    auto it0 = v1.begin();
    swap(v1, v2);
    auto it1 = v1.begin();
    cout << *it0 << endl;//未失效，1
    cout << *it1 << endl;//4
    for (auto i : v1)cout << i << " "; cout << endl;//4567
    for (auto i : v2)cout << i << " "; cout << endl;//123
```
#### 大小
##### empty

##### size
不支持forward_list
##### max_size

##### capacity,reserce,shrink_to_fit
capacity,reserve只适用于vector和string
reserve永远不会减少容量，只能用shrink_to_fit减少。
shrink_to_fit将capacity减少为于size相同大小，并**请求**将超出当前大小的多余内存退回给操作系统，但只是一个请求，并不能保证一定退还内存，这是操作系统的事。
```cpp
    vector<int>a;
    for (int i = 0; i < 100; ++i) a.push_back(i);
    cout << a.size() << endl;//100
    cout << a.capacity() << endl;//141
    a.shrink_to_fit();
    cout << a.size() << endl;//100
    cout << a.capacity() << endl;//100
```
每次扩容当大小超过capacity时，都要重新分配大小，将原有的元素移到新位置，释放旧内存，vector的扩张操作通常比list和deque快。
扩容分配策略遵循原则：通过一个初始为空的vector上调用n次push_back()来创建n个元素所花费的时间不能超过n的常数倍。

#### 获取迭代器

#### 反向容器的额外成员

#### 添加
vector,deuqe,string等插入元素会使迭代器，指针，引用失效。
在vector的尾部以外或者deque的首尾以外任何地方添加元素，都**可能**引起整个对象存储空间的重新分配。重新分配内存，将元素从旧的地址空格键移动到新的空间中。
forward_list不支持push_back和empace_back,有自己专属的emplace和insert
##### push_back,push_front,push
传进去的是对象的一个拷贝，而不是对象本身。
##### emplace_back,emplace_front,emplace
emplace_back,emplace_front,emplace分别对应push_back,push_front和insert操作。
区别是当我们调用emplace成员函数时，是将参数传递给元素类型的**构造函数**，emplace成员使用这些参数在容器管理的内存空间中直接构造元素。而push_back()则会创建一个局部临时对象，并将其压入容器。
##### insert
```cpp
  vector<int> a = {1,2,3};
    auto iter = a.begin();
    for (int i = 0; i < 10; ++i)
    {
        iter = a.insert(iter, i);//反复插元素，返回的是新插入的位置的迭代器
    }
    for (auto i : a)cout << i << " "; cout << endl;
    //9 8 7 6 5 4 3 2 1 0 1 2 3
```
#### 访问
返回的是引用
##### at
只适合vector，deque，string，array
越界则抛出`out_of_range`异常。
##### []
```cpp
    vector<int> a = { 1,2,3 };
    auto atmp = a[1];
    atmp = 10;
    for (auto i : a)cout << i << " "; cout << endl;//1 10 3
    auto& atmp2 = a[1];
    atmp2 = 10;
    for (auto i : a)cout << i << " "; cout << endl;// 1 2 3
```
### vector
#### 初始化
```cpp
vector<int>a(10);
vector<int>b(10,1);
vector<int>c={1,2,3,4,5,6};
vector<int>d({1,2,3});
vector<int>e(c);
vector<int>f(c.begin(), c.begin() + 6);
vector<int>g(&c[0], &c[5]+1);
vector<int>e(c,c+6);
```
#### 各接口
```cpp
vector<int>a={1,2,3,4,5,6};
a.push_back();
a.pop_back();
a.emplace_back();
a.empty();
a.size();
b.max_size();//可保存的最大元素数目
a.capacity();
a.resize(100);
a.reserve(100);
a.assign();
a[1];
a.at(1);
a.data();
a.clear();
a.earse();
a.swap(b);;
```

**两种访问方式：**
```cpp
vector::at()//进行边界检查，效率低于[]，超出边界会抛出exception。
vector::operator[]//可能月越界，访问效率高
```
在release模式下,[]越界不会报错，at()会触发异常。

**删除**
clear(),pop_back(),erase()
```cpp
bool cmp(int x)
{
    if (x < 5) return false;
    return true;
}


    vector<int> b;
    b.push_back(1);
    b.push_back(2);
    b.push_back(3);
    b.push_back(4);
    b.push_back(5);
    b.push_back(6);
    auto it = b.begin();
    b.erase(it + 1);
    for (auto i : b) cout << i << "  ";
    cout << endl;
   // auto it2 = remove(b.begin(), b.end(), 5);
    auto it2 = remove_if(b.begin(), b.end(), cmp);
    b.erase(it2,b.end());
    for (auto i : b) cout << i << "  ";
    b.clear();
```
vector\<bool\>较为特殊：可以参考[http://www.cplusplus.com/reference/vector/vector-bool/](http://www.cplusplus.com/reference/vector/vector-bool/)
还有bitset(位图)和valarray(支持高速运算的数组)
#### emplace
##### 参考资料
* [emplace_back减少内存拷贝和移动](https://www.cnblogs.com/carsonzhu/p/5113213.html)

 C++11 起 push_back 需要分配新内存时一般都是把元素移动构造过去，而非复制构造。

在vector文件中debug push_back()和emplace_back()的源码。观察函数调用
```c
#include <map>
struct Complicated
{
	
	int year;
	double country;
	std::string name;

	Complicated(int a, double b, string c) :year(a), country(b), name(c)
	{
		cout << "is constucted" << endl;
	}

	Complicated(const Complicated& other) :year(other.year), country(other.country), name(std::move(other.name))
	{
		cout << "is moved" << endl;
	}
	~Complicated()
	{
		cout << "Deconstruction" << endl;
	}
};
```
```c
	std::map<int, Complicated> m;
	int anInt = 4;
	double aDouble = 5.0;
	std::string aString = "C++";
	cout << "—insert--" << endl;
	m.insert(std::make_pair(4, Complicated(anInt, aDouble, aString)));

	cout << "—emplace--" << endl;
	// should be easier for the optimizer 
	m.emplace(4, Complicated(anInt, aDouble, aString));

	cout << "--emplace_back--" << endl;
	vector<Complicated> v;
	v.emplace_back(anInt, aDouble, aString);
	cout << "--两步push_back--" << endl;
	Complicated tmp(anInt, aDouble, aString);
	//v.push_back(Complicated(anInt, aDouble, aString));
	v.push_back(tmp);
	cout << "--一步push_back--" << endl;
	v.push_back(Complicated(anInt, aDouble, aString));
```
### array
定义时，必须要指定类型和大小，底层有数组实现，所以和数组很像，但可以比数组功能强，如可以直接用`=`进行array之间的赋值。

* 不支持assign
* 不支持列表赋值。
### deque 双端队列
double-ended queue 的缩写，又称双端队列容器。
双端队列(deque) 连续存储的指向不同元素的指针所组成的数组<deque>
![a\b\20200817220411_59767566eb92da851fc640b1dff429e1.png](https://lcg-pic-tencent-1258286866.cos.ap-chengdu.myqcloud.com/a%5Cb%5C20200817220411_59767566eb92da851fc640b1dff429e1.png)
分段连续，然后串接

### list 双向链表

```cpp
#include<list>
void TestList()
{
    list<int> l;
    l.push_back(0);
    l.push_back(4);
    l.push_back(5);
    list<int> l2;
    l2.push_back(1);
    l2.push_back(2);
    l2.push_back(3);
    auto it = l.begin();
    advance(it, 1);//迭代器不能自增，所以想要得到特定位置迭代器，需要调用此函数
    l.insert(it, l2.begin(), l2.end());
    for (auto i : l)cout << i << " ";
    cout << endl;
    list<int> l3 = {-3,-2,-1};
    it = l.begin();
    l.splice(it, l3);//splice删除list的部分或全部元素，并拼接到另一个list的迭代器的位置处。
    for(auto i:l) cout << i << " ";
    cout << endl;
    cout << l3.size() << endl;//l3已经被清除
}
```
![a\b\20200817225258_56d544eed39c72453f7da516d1773c17.png](https://lcg-pic-tencent-1258286866.cos.ap-chengdu.myqcloud.com/a%5Cb%5C20200817225258_56d544eed39c72453f7da516d1773c17.png)

![a\b\20200817225533_d18cb3e22e95d199a29049486e398058.png](https://lcg-pic-tencent-1258286866.cos.ap-chengdu.myqcloud.com/a%5Cb%5C20200817225533_d18cb3e22e95d199a29049486e398058.png)


### stack栈
先进后出
![a\b\20200817231133_bd927e6909da8f2c5614791e05208251.png](https://lcg-pic-tencent-1258286866.cos.ap-chengdu.myqcloud.com/a%5Cb%5C20200817231133_bd927e6909da8f2c5614791e05208251.png)
![a\b\20200817231357_1eb1348431614a72c7a003b0adc1d649.png](https://lcg-pic-tencent-1258286866.cos.ap-chengdu.myqcloud.com/a%5Cb%5C20200817231357_1eb1348431614a72c7a003b0adc1d649.png)
### queue
先进先出,底层结构：deque/list，没有迭代器
```
#include<queue>
queue<int> q;
q.push(1);
q.pop();
q.front();
q.back();
```
### forward_list
迭代器不支持（--）
不支持反向容器的额外成员

### set/multiset
![a\b\20200819110908_5fb92fb48141f02425b03a417c3d5d17.png](https://lcg-pic-tencent-1258286866.cos.ap-chengdu.myqcloud.com/a%5Cb%5C20200819110908_5fb92fb48141f02425b03a417c3d5d17.png)
```cpp
template < class T,                        // 键 key 和值 value 的类型
           class Compare = less<T>,        // 指定 set 容器内部的排序规则
           class Alloc = allocator<T>      // 指定分配器对象的类型
           > class set;
```
```cpp
    set<int> set1 = {4,3,2,1};
    set1.insert(5);
    set1.emplace(6);
    for (int i : set1)  cout << i << " "; cout << endl;
    auto it = set1.find(3);
    it++; cout << *it << endl;
    it = set1.find(0);
    cout <<std::boolalpha<< (it==set1.end()) << endl;
    it = set1.lower_bound(3); cout << *it << endl;
    it = set1.upper_bound(3); cout << *it << endl;
    auto it2 = set1.equal_range(3); cout << *it2.first <<" "<<*it2.second<< endl;
    cout << set1.size() << endl;
    it = set1.find(1);
    set1.erase(it);
    set1.erase(2);
    set1.erase(100);
    for (int i : set1)  cout << i << " "; cout << endl;
```
```
1 2 3 4 5 6
4
true
3
4
3 4
6
3 4 5 6
```

```cpp
    multiset<int> mset1;
    mset1.insert(4);
    mset1.insert(3);
    mset1.insert(2);
    mset1.insert(2);
    mset1.insert(2);
    mset1.insert(5);
    mset1.emplace(6);
    cout << mset1.count(2) << endl;
    for (int i : mset1)  cout << i << " "; cout << endl;
    auto it = mset1.find(3);
    it++; cout << *it << endl;
    it = mset1.find(0);
    cout << std::boolalpha << (it == mset1.end()) << endl;
    it = mset1.lower_bound(2); cout << *it << endl;
    it = mset1.upper_bound(2); cout << *it << endl;
    auto it2 = mset1.equal_range(2); cout << *it2.first << " " << *it2.second << endl;
    cout << mset1.size() << endl;
    it= mset1.find(2);
    mset1.erase(it);//删除一个
    for (int i : mset1)  cout << i << " "; cout << endl;
    mset1.erase(2);//删除所有
    mset1.erase(100);
    for (int i : mset1)  cout << i << " "; cout << endl;
```
```
3
2 2 2 3 4 5 6
4
true
2
3
2 3
7
2 2 3 4 5 6
3 4 5 6
```
### map/multimap
关联容器，不允许相同的key，存储的对象必须可排序。
![a\b\20200817231906_f1a373cb31070bf14c7ea393cd36488e.png](https://lcg-pic-tencent-1258286866.cos.ap-chengdu.myqcloud.com/a%5Cb%5C20200817231906_f1a373cb31070bf14c7ea393cd36488e.png)
```cpp
template < class Key,                                     // 指定键（key）的类型
           class T,                                       // 指定值（value）的类型
           class Compare = less<Key>,                     // 指定排序规则
           class Alloc = allocator<pair<const Key,T> >    // 指定分配器对象的类型
           > class map;
```
```cpp
    map<int,char>mp1;
    mp1[0] = 0 + '0';//可直接赋值
    mp1[0] = 0 + '0';
    cout << mp1.count(0) << endl;
    for (int i = 5; i >= 0; --i) mp1.emplace(i, i + '0');//直接构造
    mp1.insert(make_pair(6, 6 + '0'));
    for (auto i : mp1)cout << i.second << " "; cout << endl;
```
```
1
0 1 2 3 4 5 6
```

```cpp
    multimap<int, char>mp1;
    //mp1[0] = 0 + '0';//不可直接赋值
    for (int i = 5; i >= 0; --i) mp1.emplace(i, i + '0');//直接构造
    mp1.insert(make_pair(6, 6 + '0'));
    mp1.insert(make_pair(0, 0 + '0'));//重复的也会压进去
    mp1.insert(make_pair(0, 7 + '0'));
    mp1.insert(make_pair(0, 8 + '0'));
    cout << mp1.count(0) << endl;
    for (auto i : mp1)cout << i.second << " "; cout << endl;
    for (auto it = mp1.begin(); it != mp1.end(); ++it)cout << it->first << " " << it->second << endl;
```
```
4
0 0 7 8 1 2 3 4 5 6
0 0
0 0
0 7
0 8
1 1
2 2
3 3
4 4
5 5
6 6
```
#### 哈希表
[哈希表（散列表）详解（包含哈希表处理冲突的方法）](http://c.biancheng.net/view/3437.html)
哈希函数：f（）是一个函数，通过这个函数可以快速求出该关键字对应的的数据的哈希地址，称之为“哈希函数”
哈希冲突：对于哈希表而言，冲突只能尽可能地少，无法完全避免。
哈希地址：哈希地址只是表示在查找表中的存储位置，而不是实际的物理存储位置。
**常用的哈希函数的构造方法有 6 种：**
* 直接定址法：其哈希函数为**一次函数**，H（key）= key 或者 H（key）=a * key + b，其中 H（key）表示关键字为 key 对应的哈希地址，a 和 b 都为常数。
* 数字分析法 ：如果关键字由多位字符或者数字组成，就可以考虑抽取其中的 2 位或者多位作为该关键字对应的哈希地址，在取法上尽量**选择变化较多**的位，避免冲突发生。
* 平方取中法：关键字做平方操作，取中间得几位作为哈希地址。
* 折叠法
* 除留余数法：若已知整个哈希表的最大长度 m，可以取一个不大于 m 的数 p，然后对该关键字 key 做取余运算，即：H（key）= key % p，在此方法中，对于 p 的取值非常重要，由经验得知 p 可以为**不大于 m 的质数或者不包含小于 20 的质因数的合数**。
* 随机数法：是取关键字的一个(伪)随机函数值作为它的哈希地址，即：H（key）=random（key），此方法适用于关键字长度不等的情况。

**处理冲突的方法**
* 开放定址法 ： H（key）=（H（key）+ d）MOD m（其中 m 为哈希表的表长，d 为一个增量） 当得出的哈希地址产生冲突时，选取以下 3 种方法中的一种获取 d 的值，然后继续计算，直到计算出的哈希地址不在冲突为止，这 3 种方法为：
    + 线性探测法：d=1，2，3，…，m-1
    + 二次探测法：d=12，-12，22，-22，32，…
    + 伪随机数探测法：d=伪随机数
* 再哈希法
* 链地址法
* 建立一个公共溢出区

#### unordered
* 关联式容器的底层实现采用的树存储结构，更确切的说是红黑树结构；
* 无序容器的底层实现采用的是哈希表的存储结构。

C++ STL 底层采用哈希表实现无序容器时，会将所有数据存储到一整块连续的内存空间中，并且当数据存储位置发生冲突时，解决方法选用的是“``链地址法``”（又称“开链法”）。
* 无序容器内部存储的键值对是无序的，各键值对的存储位置取决于该键值对中的键，
* 和关联式容器相比，无序容器擅长通过指定键查找对应的值（平均时间复杂度为 O(1)）；但对于使用迭代器遍历容器中存储的元素，无序容器的执行效率则不如关联式容器。

[https://blog.csdn.net/wusecaiyun/article/details/46723363](https://blog.csdn.net/wusecaiyun/article/details/46723363)
```
map, set, multimap, and multiset
上述四种容器采用红黑树实现，红黑树是平衡二叉树的一种。不同操作的时间复杂度近似为:
插入: O(logN)
查看:O(logN)
删除:O(logN)

hash_map, hash_set, hash_multimap, and hash_multiset
上述四种容器采用哈希表实现，不同操作的时间复杂度为：
插入:O(1)，最坏情况O(N)。
查看:O(1)，最坏情况O(N)。
删除:O(1)，最坏情况O(N)。
记住，如果你采用合适的哈希函数，你可能永远不会看到最坏情况。但是记住这一点是有必要的。
```
![a\b\20200819165151_1bfe30f9d8d06ff23242a5d7a892b7fb.png](https://lcg-pic-tencent-1258286866.cos.ap-chengdu.myqcloud.com/a%5Cb%5C20200819165151_1bfe30f9d8d06ff23242a5d7a892b7fb.png)
![a\b\20200819165217_3da67063eb2b5c12c28fff080220bde0.png](https://lcg-pic-tencent-1258286866.cos.ap-chengdu.myqcloud.com/a%5Cb%5C20200819165217_3da67063eb2b5c12c28fff080220bde0.png)