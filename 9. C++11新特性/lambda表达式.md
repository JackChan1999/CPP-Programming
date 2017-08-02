C++11中引入了lambda表达式，用于定义匿名函数，使得代码执行更加灵活，方便了程序编写。一个lambda表达式表示一个可调用的代码单元，可将其理解为未命名的内联函数。

lambda表达式的定义形式：`[捕获列表](参数列表)->返回值类型 { 函数体 }`

- 捕获列表中是lambda表达式所在函数局部变量构成的列表
- 参数列表、返回值类型、函数体与普通函数的对应内容相同

### 使用lambda表达式比较指定值与函数中变量值的大小

```c++
#include <iostream>
using namespace std;
template <class T1, class T2>
void cpbybyte(T1& x, T2& y)
{
	static_assert(sizeof(x) == sizeof(y), "the parameters must be the same width.");
};
int main()
{
	int n;
	char c;
	cpbybyte(n, c);
	return 0;
}
```