### emplace
#### 参考资料
* [emplace_back减少内存拷贝和移动](https://www.cnblogs.com/carsonzhu/p/5113213.html)



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