## STL
```
<iterator>	<functional>	<vector>	<deque>
<list>	<queue>	<stack>	<set>
<map>	<algorithm>	<numeric>	<memory>
<utility>
```
* 6大部件：
STL可分为容器(containers)、迭代器(iterators)、空间配置器(allocator)、配接器(adapters)、算法(algorithms)、仿函数(functors)六个部分。
最常用的是容器和算法，在使用算法时，不需要考虑内存的分配与释放，这是由分配器完成的，算法通过迭代器操作于容器。
![a\b\20200817124355_fce2e96d3a59375565f5696963afb156.png](https://lcg-pic-tencent-1258286866.cos.ap-chengdu.myqcloud.com/a%5Cb%5C20200817124355_fce2e96d3a59375565f5696963afb156.png)
![a\b\20200818004218_e3676769b32dfa2aca682695d66ba25c.png](https://lcg-pic-tencent-1258286866.cos.ap-chengdu.myqcloud.com/a%5Cb%5C20200818004218_e3676769b32dfa2aca682695d66ba25c.png)
![a\b\20200817125333_37a3139802fa130488b4fcf45538a2bc.png](https://lcg-pic-tencent-1258286866.cos.ap-chengdu.myqcloud.com/a%5Cb%5C20200817125333_37a3139802fa130488b4fcf45538a2bc.png)
迭代器时前闭后开区间，[)，.end()指向最后一个元素下一个元素，迭代器是泛化的指针。

### 容器分类
* 序列式的：
* 关联式容器：可用来快速查找，红黑树实现，set,map,multiset,multimap
* 不定序容器：也可以认为是关联式的，unordered_set/unordered_map/unordered_multiset/unordered_multimap/
![a\b\20200817131907_7d2cb3620f54b17769845213520ef81b.png](https://lcg-pic-tencent-1258286866.cos.ap-chengdu.myqcloud.com/a%5Cb%5C20200817131907_7d2cb3620f54b17769845213520ef81b.png)
![a\b\20200817131929_fd5d64f974a4689d49a4dc8b2a300d28.png](https://lcg-pic-tencent-1258286866.cos.ap-chengdu.myqcloud.com/a%5Cb%5C20200817131929_fd5d64f974a4689d49a4dc8b2a300d28.png)

如果标准库自己提供sort，优先用自己的。


* slist: include<ext\slist>和forward list相同，forward list是标准库中的，slist是早期的


**deque**
分段连续，前后可扩充。
![a\b\20200817151048_9308e327eaca1b987a3618501c9ddbd3.png](https://lcg-pic-tencent-1258286866.cos.ap-chengdu.myqcloud.com/a%5Cb%5C20200817151048_9308e327eaca1b987a3618501c9ddbd3.png)


### 模板
面向对象是数据和方法放一起。
泛型是数据和方法分开。（通过接口传数据，如algorithm中的迭代器）
**模板分类**
* 函数模板:调用时可以不显示指定类型，模板可以重载
* 类模板：必须显示指定类型
* 成员模板(不常用)

类模板里面有全特化和偏特化
偏特化有个数上的偏，也有范围上的偏

全特化，为指定类型执行特殊的程序，优化算法。
注意开头是
```cpp
template<>
```
![a\b\20200817163222_8d672da9b8ea60f36cbee03abb1241ad.png](https://lcg-pic-tencent-1258286866.cos.ap-chengdu.myqcloud.com/a%5Cb%5C20200817163222_8d672da9b8ea60f36cbee03abb1241ad.png)
偏特化
![a\b\20200817163555_95257572ff660612061fddcbd5199c0b.png](https://lcg-pic-tencent-1258286866.cos.ap-chengdu.myqcloud.com/a%5Cb%5C20200817163555_95257572ff660612061fddcbd5199c0b.png)

### 默认的分配器allocator
也可以自定义

### 容器 分类
![a\b\20200817164249_faf4962508cb1863f7a0b58847b43fca.png](https://lcg-pic-tencent-1258286866.cos.ap-chengdu.myqcloud.com/a%5Cb%5C20200817164249_faf4962508cb1863f7a0b58847b43fca.png)


### 迭代器的设计原则
iterator_traits 萃取
iterator有5种associated types相当于迭代器的属性，想用迭代器时使用方法要根据迭代器的这几个特性设计，如algorithm调用迭代器时需要知道这些特性。
![a\b\20200819093520_8553ddf993c6ac34424b013193790bc7.png](https://lcg-pic-tencent-1258286866.cos.ap-chengdu.myqcloud.com/a%5Cb%5C20200819093520_8553ddf993c6ac34424b013193790bc7.png)

iterator_category 单双向
value_type 容器的值类型
pointer 暂时未用
reference 暂时未用
difference_type 用来表示两个迭代器之间的距离

algorithm如果传进来的是指针(退化的迭代器)
则通过偏特化让算法知道这是指针。
![a\b\20200819094239_a6693874fbad7eb7c8bf287e6e6433c0.png](https://lcg-pic-tencent-1258286866.cos.ap-chengdu.myqcloud.com/a%5Cb%5C20200819094239_a6693874fbad7eb7c8bf287e6e6433c0.png)

#### 容器rb_tree
![a\b\20200819105346_78a2943cf3f1a1ef5523a2140ff55b2d.png](https://lcg-pic-tencent-1258286866.cos.ap-chengdu.myqcloud.com/a%5Cb%5C20200819105346_78a2943cf3f1a1ef5523a2140ff55b2d.png)