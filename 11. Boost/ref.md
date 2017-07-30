## ref库

库 ref 在 boost/ref.hpp 中提供了模板工厂函数 boost::ref，boost::cref 分别对应包装引用和常引用。在上面的小节中分别介绍了bind以及function的使用，而boost::ref可以用来助它们一臂之力。由于bind在绑定参数时，需要进行拷贝，因此当传入的引用对象无法进行拷贝时，编译器就会发生错误。那如何处理这种无法被拷贝的对象引用呢？这个时候ref就派上用场了。当然它的用处也不仅限于此，当在其它应用中，如果拷贝的代价过高时，就可以选择使用它。

```C++
#include <iostream>
#include <string>
#include <boost/bind.hpp>
#include <boost/function.hpp>
#include <vector>
#include <algorithm>
#include <functional>
#include <stdlib.h>

using namespace std;
using namespace boost;

void print(std::ostream &os,int i)
{
	os << i << endl;
}

void mainF()
{
  	// std::cout为标准输出对象，它是无法进行拷贝的，因此此处必须使用boost::ref来绑定它的引用，
	// 否则会提示拷贝构造失败，因为拷贝构造是私有的
	// 不可以拷贝的对象可以用ref
	boost::function<void(int)> pt = boost::bind(print,boost::ref(cout), _1);
	vector<int > v;
	v.push_back(11);
	v.push_back(12);
	v.push_back(13);
	for_each(v.begin(), v.end(), pt);

	std::cin.get();
}
```

当在某些情况下需要拷贝对象参数时，如果该对象无法进行拷贝，或者拷贝代价过高，这时候就可以选择ref。