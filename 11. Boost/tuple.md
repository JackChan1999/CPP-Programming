## tuple 库

tuple 库定义在 boost/tuple/tuple.hpp中，可以用来组合成含有不同数据类型的集合，它类似一个可以随时生成的匿名结构体。它最多可以绑定10个对象。当一个函数需要返回多个值的时候，就可以考虑选择 tuple，通过它可以返回一组数据，这样就无需定义结构体。

```C++
#include <iostream>

#include <boost/typeof/typeof.hpp>
#include <boost/tuple/tuple.hpp>

int main()
{
	BOOST_AUTO(t, boost::make_tuple(1, "12345", 'a', 23.534));
	std::cout << t.get<0>() << std::endl;
	std::cout << t.get<1>() << std::endl;
	std::cout << t.get<2>() << std::endl;
	std::cout << t.get<3>() << std::endl;

	return 0;
}
```

```C++
#include <string>
#include <iostream>

#include <boost/typeof/typeof.hpp>
#include <boost/tuple/tuple.hpp>

boost::tuple<int, std::string, char, double> return_group()
{
	return boost::make_tuple(1, "12345", 'a', 23.534);
}

int main()
{
	BOOST_AUTO(t, return_group());
	std::cout << t.get<0>() << std::endl;
	std::cout << t.get<1>() << std::endl;
	std::cout << t.get<2>() << std::endl;
	std::cout << t.get<3>() << std::endl;

	return 0;
}
```