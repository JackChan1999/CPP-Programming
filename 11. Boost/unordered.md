## Unordered 库

Boost.Unordered 在 C++ 标准容器 std::set， std::multiset， std::map 和 std::multimap 的基础上多实现了四个容器：

- boost::unordered_set
- boost::unordered_multiset
- boost::unordered_map
- boost::unordered_multimap

那些名字很相似的容器之间并没有什么不同， 甚至还提供了相同的接口。 在很多情况下， 替换这两种容器 (std 和 boost) 对你的应用不会造成任何影响。

```C++
#include <iostream>
#include <string>
#include <boost/unordered_set.hpp>

int main()
{
	typedef boost::unordered_set<std::string> unordered_set;
	unordered_set set;

	set.insert("Boris");
	set.insert("Anton");
	set.insert("Caesar");

	for (unordered_set::iterator it = set.begin(); it != set.end(); ++it)
		std::cout << *it << std::endl;

	std::cout << set.size() << std::endl;
	std::cout << set.max_size() << std::endl;

	std::cout << (set.find("David") != set.end()) << std::endl;
	std::cout << set.count("Boris") << std::endl;

	return 0;
}
```

```C++
#include <iostream>
#include<boost/unordered_set.hpp>
#include<string>
using namespace std;

void main()
{
	boost::unordered_set<std::string> myhashset;
	myhashset.insert("ABC");
	myhashset.insert("ABCA");
	myhashset.insert("ABCAG");

	for (auto ib = myhashset.begin(); ib != myhashset.end();ib++)
	{
		cout << *ib << endl;
	}
	std::cout << (myhashset.find("ABCA1") != myhashset.end()) << endl;

	cin.get();
}
```

小结：它里面的库与原有c++标准库中的map、set等相对应，只是内部数据结构改红黑树为hash表来实现，虽然会使得对象的存储空间增大不少，但是查找的平均复杂度却由O(logN))变成O(1)，大大提升查找效率。对于需要平凡进行索引查找的应用来说，是极好的选择(当然你还需要考虑效费比，主要是存储空间)。