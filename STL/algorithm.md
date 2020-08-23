### STL algorithm
泛型算法本身不会执行容器的操作，他们只会运行迭代器之上。永远不会添加和删除元素。不是说容器经过算法后容器大小不变，而是算法会通过使用迭代器而控制容器，而不是直接控制容器。如插入器(inserter)

#### remove
移除指定元素

#### remove_if
[c++ remove_if](https://www.cnblogs.com/zhangdongsheng/p/8590221.html)
remove_if()并不会实际移除序列[start, end)中的元素; 如果在一个容器上应用remove_if(), 容器的长度并不会改变(remove_if()不可能仅通过迭代器改变容器的属性), 所有的元素都还在容器里面. 实际做法是, remove_if()将所有应该移除的元素都移动到了容器尾部并返回一个分界的迭代器. 移除的所有元素仍然可以通过返回的迭代器访问到. 为了实际移除元素, 你必须对容器自行调用erase()以擦除需要移除的元素.
```cpp
container.erase(remove_if(container.begin(), container.end(), pred), container.end());
```