## 智能指针

```C++
#include <iostream>  
  
void main1()  
{  
    //auto_ptr; C++98
    for (int i = 0; i < 10000000; i++)  
    {  
        double *p = new double;//为指针分配内存  
        std::auto_ptr<double> autop(p);
        //创建智能指针管理指针p指向的内存
        //智能指针   
        //delete p;  
    }  
  
    std::cin.get();  
}
```

C++11智能指针

```C++
#include<iostream>  
#include<memory>//内存  
  
void main()  
{  
    for (int i = 0; i < 10000000;i++)  
    {  
        //新型指针，新型的数组        
        std::unique_ptr<double> pdb(new double);  
        //double *p = new double;  
    }  
  
    std::cin.get();  
}
```

## smartpointers库

智能指针的原理基于一个常见的习语叫做 RAII ：资源申请即初始化。 智能指针只是这个习语的其中一例——当然是相当重要的一例。 智能指针确保在任何情况下，动态分配的内存都能得到正确释放，从而将开发人员从这项任务中解放了出来。 这包括程序因为异常而中断，原本用于释放内存的代码被跳过的场景。 用一个动态分配的对象的地址来初始化智能指针，在析构的时候释放内存，就确保了这一点。 因为析构函数总是会被执行的，这样所包含的内存也将总是会被释放。

## RAII的原理

```C++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <stdlib.h>
#include <string>

using namespace std;

class mystr
{
public:
	char *p = nullptr;
public:
	mystr(const char *str)
	{
		cout << "构建" << endl;
		int length = strlen(str);
		p = new char[length + 1];
		strcpy(p, str);
		p[length] = '\0';
	}
	~mystr()
	{
		cout << "销毁" << endl;
		delete[] p;
	}
};

void go()
{
	char *p = new char[100];
	mystr str1 = "ABCD";//RAII避免内存泄漏，一般情况下，堆上的内存当作栈上来使用
	//栈内存有限，希望自动释放，用很大的内存。
}

void main()
{
	go();
	cin.get();
}
```
```C++
#include <stdio.h>
#include <stdlib.h>

// 定义一个支持各种指针类型的模板
template <typename T>
class MemHandler
{
public:
	MemHandler():_t(NULL) {}
	MemHandler(T* t):_t(t) {}
	~MemHandler() {
		if (_t != NULL)
			delete _t;
	}
private:
	T* _t;
};

class Test
{
public:
	Test() 
    {
      	printf("Test construct\n");
    }
  
	~Test() 
    {
      	printf("~Test destroyed\n");
    }
};

int main()
{
	// 程序退出前,会自动释放
	MemHandler<int> handler1(new int);
	MemHandler<Test> handler2(new Test);
	return 0;
}
```
## smartpointers原理

```C++
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include <functional>
#include <stdlib.h>

using namespace std;

template<class T>
class pmy
{
public:
	pmy()
	{

	}
  
	pmy(T *t)
	{
		p = t;
	}
  
	~pmy()
	{
       if (p!=nullptr)
       {
		   delete p;
       }
	}
  
	T operator *()
	{
		return *p;
	}

private:
	T *p=nullptr;
};

class Test
{
public:
	Test()
	{
		cout << "Test  create" << endl;
	}
	~Test()
	{
		cout << "Test delete" << endl;
	}
};

void run()
{
	pmy<Test> p(new Test);//智能指针，智能释放
	//*p;
}

void main()
{
	run();
	cin.get();
}
```

## 智能指针

- boost::scoped_ptr 作用域指针

```C++
#include <iostream>
#include <boost/scoped_ptr.hpp>
using namespace std;

void main()
{
	boost::scoped_ptr<int> p(new int);//作用域指针，自动释放内存
	*p = 12;
	cout << *p.get() << endl;
	p.reset(new int);
	*p.get() = 3;
	boost::scoped_ptr<int> pA(nullptr);//独占内存
	//pA = p; // err

	cout << *p.get() << endl;
	cin.get();
}
```

- boost::scoped_array 作用域数组

```C++
#include <iostream>
#include <boost/scoped_array.hpp>
using namespace std;

void main()
{
	boost::scoped_array<int> p(new int[10]);//作用域数组，自动释放内存
	//boost::scoped_array<int> pA(p);独享指针
	*p.get() = 1;
	p[3] = 2;
	p.reset(new int[5]);//智能指针

	cin.get();
}
```

- boost::shared_ptr 共享指针

```C++
#include <iostream>
#include <vector>
#include<algorithm>
#include <boost/shared_ptr.hpp>
using namespace std;

void show(boost::shared_ptr<int> p)
{
	cout << *p << endl;
}

void  main()
{
	vector<boost::shared_ptr<int> > v;
	boost::shared_ptr<int> p1(new int(11));
	boost::shared_ptr<int> p2(new int(12));
	boost::shared_ptr<int> p3(p2);//拷贝
	v.push_back(p1);
	v.push_back(p2);
	v.push_back(p3);
	for_each(v.begin(), v.end(), show);
	cin.get();
}
```

```C++
class runclass
{
public:
	int  i = 0;
public:
	runclass(int num) :i(num)
	{
		cout << "i create" <<i<< endl;
	}
	runclass() 
	{
		cout << "i create" << i << endl;
	}
	~runclass()
	{
		cout << "i delete" <<i<< endl;
	}
	void print()
	{
		cout << "i =" << i<<endl;
	}
};

void testfun()
{
	boost::shared_ptr<runclass>  p1(new runclass(10));
	boost::shared_ptr<runclass>  p2(p1);
	boost::shared_ptr<runclass>  p3(p1);
	p1.reset(new runclass(12));
	p1->print();
	p2->print();
	p3->print();
}

void main()
{
	testfun();
	cin.get();
}
```

- boost::shared_array 共享数组

```C++
#include <iostream>
#include <boost/shared_array.hpp>

using namespace std;

void  testfunarray()
{
	boost::shared_array<runclass> p1(new runclass[5]);
	boost::shared_array<runclass> p2(p1);
}

void main()
{
	testfunarray();
	cin.get();
}
```

- 弱指针

```C++
#include <iostream>
#include<boost/weak_ptr.hpp>
#include <windows.h>
using namespace std;

// DWORD <--> unsigned long
DWORD  WINAPI reset(LPVOID p)
{
	boost::shared_ptr<int > *sh = static_cast<boost::shared_ptr<int > *> (p);
	sh->reset();//指针的重置,释放内存
	std::cout << "指针执行释放" << endl;
	return 0;
}

DWORD WINAPI print(LPVOID p)
{
	boost::weak_ptr<int > * pw = static_cast<boost::weak_ptr<int > *>(p);
	boost::shared_ptr<int > sh = pw->lock();//锁定不可以释放
	Sleep(5000);
  
	if (sh)
	{
		std::cout << *sh << endl;
	}
	else
	{
		std::cout << "指针已经被释放" << endl;
	}
	return 0;
}

void main()
{
	boost::shared_ptr<int> sh(new int(99));
	boost::weak_ptr<int > pw(sh);
	HANDLE threads[2];
	threads[0] = CreateThread(0, 0, reset, &sh, 0, 0);//创建一个线程
	threads[1] = CreateThread(0, 0, print, &pw, 0, 0);
	Sleep(1000);
  
	WaitForMultipleObjects(2, threads, TRUE, INFINITE);//等待线程结束
	cin.get();
}
```

1998年修订的第一版C++标准只提供了一种智能指针： std::auto_ptr 。 它基本上就像是个普通的指针： 通过地址来访问一个动态分配的对象。 std::auto_ptr 之所以被看作是智能指针，是因为它会在析构的时候调用 delete 操作符来自动释放所包含的对象。 当然这要求在初始化的时候，传给它一个由 new 操作符返回的对象的地址。 既然 std::auto_ptr 的析构函数会调用 delete 操作符，它所包含的对象的内存会确保释放掉。 这是智能指针的一个优点。

## 作用域指针

它独占一个动态分配的对象， 对应的类名为 boost::scoped_ptr，定义在 boost/scoped_ptr.hpp 中。不像 std::auto_ptr，作用域指针不能传递它所包含的对象的所有权到另一个作用域指针。一旦用一个地址来初始化，这个动态分配的对象将在析构阶段释放。因为一个作用域指针只是简单保存和独占一个内存地址，所以 boost::scoped_ptr 的实现就要比 std::auto_ptr 简单。 在不需要所有权传递的时候应该优先使用 boost::scoped_ptr 。 在这些情况下，比起 std::auto_ptr 它是一个更好的选择，因为可以避免不经意间的所有权传递。因此我们通常可以用它来实现局部动态对象的自动释放，比如在函数调用中产生的动态对象。

```C++
#include <boost/scoped_ptr.hpp>

int main()
{
	boost::scoped_ptr<int> i(new int);
	*i = 1;
	*i.get() = 2;
	i.reset(new int);
	return 0;
}
```

## 作用域数组

作用域数组的使用方式与作用域指针相似。 关键不同在于，作用域数组的析构函数使用 delete[] 操作符来释放所包含的对象。 因为该操作符只能用于数组对象，所以作用域数组必须通过动态分配的数组来初始化。对应的作用域数组类名为 boost::scoped_array，它的定义在 boost/scoped_array.hpp 里。

```C++
#include <boost/scoped_array.hpp>

int main()
{
	boost::scoped_array<int> i(new int[2]);
	*i.get() = 1;
	i[1] = 2;
	i.reset(new int[3]);
}
```

## 共享指针

这个智能指针命名为 boost::shared_ptr，定义在 boost/shared_ptr.hpp 里。智能指针 boost::shared_ptr 基本上类似于 boost::scoped_ptr。 关键不同之处在于 boost::shared_ptr 不一定要独占一个对象。 它可以和其他 boost::shared_ptr 类型的智能指针共享所有权。 在这种情况下，当引用对象的最后一个智能指针销毁后，对象才会被释放。

因为所有权可以在 boost::shared_ptr 之间共享，任何一个共享指针都可以被复制，这跟 boost::scoped_ptr 是不同的。 这样就可以在标准容器里存储智能指针了——你不能在标准容器中存储std::auto_ptr，因为它们在拷贝的时候传递了所有权。

```C++
#include <vector>
#include <algorithm>
#include <boost/shared_ptr.hpp>

void print(boost::shared_ptr<int> i)
{
	printf("%d\n", *i);
}

int main()
{
	std::vector<boost::shared_ptr<int> > v;
	{
		boost::shared_ptr<int> i1(new int(1));
		boost::shared_ptr<int> i2(new int(2));
		v.push_back(i1);
		v.push_back(i2);
	}
	
	std::for_each(v.begin(), v.end(), print);

	return 0;
}
```

```C++
#include <stdio.h>
#include <stdlib.h>
#include <boost/shared_ptr.hpp>

class Test {
public:
	Test(int i):_i(i) 
    {
      	printf("Test construct: %d\n", _i);
    }
  
	~Test() 
    {
      	printf("~Test destroyed: %d\n", _i);
    }
  
	void print() 
    {
      	printf("Test value: %d\n", _i);
    }
  
private:
	int _i;
};

int main()
{
	boost::shared_ptr<Test> i1(new Test(1));
	boost::shared_ptr<Test> i2(i1);
	boost::shared_ptr<Test> i3(i1);
	i1.reset(new Test(2));
	i1->print();
	i2->print();
	i3->print();

	return 0;
}
```

默认情况下，boost::shared_ptr 使用 delete 操作符来销毁所含的对象。 然而，具体通过什么方法来销毁，是可以指定的，就像下面的例子里所展示的：

```C++
#include <boost/shared_ptr.hpp>
#include <windows.h>

int main() 
{
	// 指定指针对象的释放方式
	boost::shared_ptr<void> h(OpenProcess(
      PROCESS_SET_INFORMATION, 
      FALSE, 
      GetCurrentProcessId()), 
      CloseHandle);
  
	SetPriorityClass(h.get(), HIGH_PRIORITY_CLASS);

	return 0;
}
```

## 共享数组

共享数组的行为类似于共享指针。 关键不同在于共享数组在析构时，默认使用 delete[] 操作符来释放所含的对象。 因为这个操作符只能用于数组对象，共享数组必须通过动态分配的数组的地址来初始化。共享数组对应的类型是 boost::shared_array，它的定义在boost/shared_array.hpp 里。

```C++
#include <iostream>

#include <boost/shared_array.hpp>

int main()
{
	boost::shared_array<int> i1(new int[2]);
	boost::shared_array<int> i2(i1);
	i1[0] = 1;
	std::cout << i2[0] << std::endl;

	return 0;
}
```

## 弱指针

弱指针 boost::weak_ptr 的定义在boost/weak_ptr.hpp 里。到目前为止介绍的各种智能指针都能在不同的场合下独立使用。 相反，弱指针只有在配合共享指针一起使用时才有意义。因此弱指针被看做是共享指针的观察者，用来观察共享指针的使用情况。当用到共享指针时，就要考虑是否需要使用弱指针了，这样可以解除一些环形依赖问题。

```C++
#include <windows.h>
#include <boost/shared_ptr.hpp>
#include <boost/weak_ptr.hpp>
#include <iostream>

DWORD WINAPI reset(LPVOID p) {
	boost::shared_ptr<int> *sh = static_cast<boost::shared_ptr<int>*>(p);
	sh->reset();
	return 0;
}

DWORD WINAPI print(LPVOID p) {
	boost::weak_ptr<int> *w = static_cast<boost::weak_ptr<int>*>(p);
	boost::shared_ptr<int> sh = w->lock();
	if (sh)
		std::cout << *sh << std::endl;
	return 0;
}

int main()
{
	boost::shared_ptr<int> sh(new int(99));
	boost::weak_ptr<int> w(sh);
	HANDLE threads[2];
	threads[0] = CreateThread(0, 0, reset, &sh, 0, 0);
	threads[1] = CreateThread(0, 0, print, &w, 0, 0);
	WaitForMultipleObjects(2, threads, TRUE, INFINITE);
	return 0;
}
```

boost::weak_ptr 必定总是通过 boost::shared_ptr 来初始化的。一旦初始化之后，它基本上只提供一个有用的方法: lock()。此方法返回的boost::shared_ptr 与用来初始化弱指针的共享指针共享所有权。 如果这个共享指针不含有任何对象，返回的共享指针也将是空的。当函数需要一个由共享指针所管理的对象，而这个对象的生存期又不依赖于这个函数时，就可以使用弱指针。 只要程序中还有一个共享指针掌管着这个对象，函数就可以使用该对象。 如果共享指针复位了，就算函数里能得到一个共享指针，对象也不存在了。

上例的 main() 函数中，通过 Windows API 创建了2个线程。 于是乎，该例只能在 Windows 平台上编译运行。第一个线程函数 reset() 的参数是一个共享指针的地址。 第二个线程函数 print() 的参数是一个弱指针的地址。 这个弱指针是之前通过共享指针初始化的。一旦程序启动之后，reset() 和 print() 就都开始执行了。 不过执行顺序是不确定的。 这就导致了一个潜在的问题：reset() 线程在销毁对象的时候print() 线程可能正在访问它。通过调用弱指针的 lock() 函数可以解决这个问题：如果对象存在，那么 lock() 函数返回的共享指针指向这个合法的对象。否则，返回的共享指针被设置为0，这等价于标准的null指针。弱指针本身对于对象的生存期没有任何影响。 lock() 返回一个共享指针，print() 函数就可以安全的访问对象了。 这就保证了——即使另一个线程要释放对象——由于我们有返回的共享指针，对象依然存在。

## 小结

对智能指针的良好运用可以有效处理内存、资源等的自我管理释放问题。对于现代c++来说，是必不可少的一件“神器”。同时在boost库里面也有广泛应用，如在asio中，shared_ptr的使用就是其中一例，有兴趣的可以再进行了解。