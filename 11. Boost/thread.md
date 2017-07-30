在这个库最重要的一个类就是 boost::thread，它是在 boost/thread.hpp 里定义的，用来创建一个新线程。它已经被纳入c++标准库中。

```C++
#include <boost/thread.hpp>
#include <iostream>

void wait(int seconds)
{
	boost::this_thread::sleep(boost::posix_time::seconds(seconds));
}

void thread()
{
	for (int i = 0; i < 5; ++i)
	{
		wait(1);
		std::cout << i << std::endl;
	}
}

int main()
{
	boost::thread t(thread);
	t.join();
}
```

```C++
#include <boost/thread.hpp>
#include <iostream>

void wait(int seconds)
{
	boost::this_thread::sleep(boost::posix_time::seconds(seconds));
}

void thread()
{
	try
	{
		for (int i = 0; i < 5; ++i)
		{
			wait(1);
			std::cout << i << std::endl;
		}
	}
	catch (boost::thread_interrupted&)
	{
	}
}

int main()
{
	boost::thread t(thread);
	wait(3);
	t.interrupt();
	t.join();
}
```

```C++
#include <iostream>
#include <vector>
#include<algorithm>
#include<boost/thread.hpp>
#include <windows.h>

using namespace std;
using namespace boost;

void wait(int sec)
{
	boost::this_thread::sleep(boost::posix_time::seconds(sec));
}

void threadA()
{
	for (int i = 0; i < 10;i++)
	{
		wait(1);
		std::cout << i << endl;
	}
}

void threadB()
{
	try
	{
		for (int i = 0; i < 10; i++)
		{
			wait(1);
			std::cout << i << endl;
		}
	}
	catch (boost::thread_interrupted &)
	{
	}
}

void main()
{
	boost::thread t(threadA );
	wait(3);
	t.interrupt();//结束线程
	t.join();
	cin.get();
}
```

小结：新一代c++标准将线程库引入后，将简化多线程开发。