## bind库

库 bind 在 boost/bind.hpp 中定义了一个模板函数 boost::bind 。 通过使用这个函数，可以将多个函数参数绑定到一起，形成一个仿函数对象，并可以选择保存成boost::function对象。 

它可用来替换c++标准库中著名的 std::bind1st() 和 std::bind2nd()，简化代码。

它还可以配合其它一些boost库实现特有功能，其中之一就是配合上面提到的boost::function；当两者结合使用时，可以用来实现全局函数和类成员函数回调功能，摆脱以往需要使用虚函数来实现回调接口的麻烦。另外还可以配合boost::asio等库来实现相应功能。

未使用boost::bind前

```C++
#include <iostream>
#include <vector>
#include <algorithm>
#include <functional>

class add : public std::binary_function<int, int, void>
{
public:
	void operator()( int i, int j ) const
	{
		std::cout << i + j << std::endl;
	}
};

int main()
{
	std::vector<int> v;
	v.push_back( 1 );
	v.push_back( 3 );
	v.push_back( 2 );
	std::for_each( v.begin(), v.end(), std::bind1st( add(), 10 ) );
}
```
使用boost::bind

```C++
#include <boost/bind.hpp> 
#include <iostream> 
#include <vector> 
#include <algorithm> 

void add(int i, int j) 
{ 
	std::cout << i + j << std::endl; 
} 

int main() 
{ 
	std::vector<int> v; 
	v.push_back(1); 
	v.push_back(3); 
	v.push_back(2); 

	std::for_each(v.begin(), v.end(), boost::bind(add, 10, _1)); 
}
```
未使用boost::bind前，以上程序将值10加至容器 v 的每个元素之上，并使用标准输出流显示结果。 实现此功能：add() 函数转换为一个派生自std::binary_function 的函数对象。

使用boost::bind，像 add() 这样的函数不再需要为了要用于 std::for_each() 而转换为函数对象。 使用 boost::bind()，这个函数可以忽略其第一个参数而使用。

因为 add() 函数要求两个参数，两个参数都必须传递给 boost::bind()。 第一个参数是常数值10，而第二个参数则是一个 占位符，表示它是一元函数，满足std::for_each() 的使用条件。

```c++
#include <iostream>
#include <string>
#include <boost/bind.hpp>
#include <vector>
#include <algorithm>
#include <functional>

using namespace std;
using namespace boost;

//绑定函数的默认值，继承二进制函数类的所有类容
class add:public std::binary_function<int ,int,void>
{
public:
	void operator()(int i,int j) const
	{
		std::cout << i + j << endl;
	}
};

void   add(int i, int j)
{
	std::cout << i + j << endl;
}


void main()
{
	vector<int> myv;
	myv.push_back(11);
	myv.push_back(23);
	myv.push_back(34);

	//for_each(myv.begin(), myv.end(), bind1st(add(),10));
	for_each(myv.begin(), myv.end(), bind(add, 13, _1));

	//bind设置默认参数调用，函数副本机制，不能拷贝构造
	cin.get();
}
```

bind可被用来生成多元仿函数，功能上可以替换std::bind1st() 和 std::bind2nd()，使得调用形式上更加简洁。对于需要绑定谓词函数，回调等方面的应用，具有很好的使用价值。另外需要注意的是，bind在绑定时会进行值拷贝，因此无法绑定不能拷贝构造的对象。