库 array 在 boost/array.hpp 中定义了一个模板类 boost::array 。 通过使用这个类，可以创建一个跟 C++ 里传统的数组有着相同属性的容器。

```C++
template<typename T, std::size_t N> 
class array{...}
```

它可用来替换传统数组，等同于内部使用传统数组包装而成的类，提供了c++容器标准接口。

```C++
#include<boost/array.hpp>
#include <iostream>
#include <string>

using namespace std;
using namespace boost;

void main()
{
	array <int, 5> barray = { 1, 2, 3, 4, 5 };
	barray[0] = 10;
	barray.at(4) = 20;
	int *p = barray.data();//存储数组的指针
	for (int i = 0; i < barray.size();i++)
	{
		cout << barray[i] << "  " << p[i] << endl;
	}

	array<string, 3> cmd = { "calc", "notepad", "tasklist" };

	cin.get();
}
```

作为普通c++定长数组的替代品，它提供了数组包含的所有特性，并且提供了与c++标准库容器一致的接口，在使用上面极具亲和力（迎合了c++开发者的使用习惯），并且由于内部数据结构就是普通数组，因此完全可以用来替代普通数组。