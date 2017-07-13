## inline内联函数

C++中的const常量可以替代宏常数定义，如：

```C++
const int A = 3; 
#define A 3
```

C++中是否有解决方案替代宏代码片段呢？（替代宏代码片段就可以避免宏的副作用！）

C++中推荐使用内联函数替代宏代码片段，C++中使用inline关键字声明内联函数

内联函数声明时inline关键字必须和函数定义结合在一起，否则编译器会直接忽略内联请求。

宏替换和函数调用区别

```C++
#include "iostream"
using namespace std;
#define MYFUNC(a, b) ((a) < (b) ? (a) : (b))  

inline int myfunc(int a, int b) 
{
	return a < b ? a : b;
}

int main()
{
	int a = 1;
	int b = 3;
	//int c = myfunc(++a, b);  //头疼系统
	int c = MYFUNC(++a, b);  

	printf("a = %d\n", a); 
	printf("b = %d\n", b);
	printf("c = %d\n", c);

	system("pause");
	return 0;
}
```

说明1：必须inline int myfunc(int a, int b)和函数体的实现，写在一块

说明2

- C++编译器可以将一个函数进行内联编译
- 被C++编译器内联编译的函数叫做内联函数
- 内联函数在最终生成的代码中是没有定义的
- C++编译器直接将函数体插入在函数调用的地方 
- 内联函数没有普通函数调用时的额外开销(压栈，跳转，返回)

说明3：C++编译器不一定准许函数的内联请求！

说明4

- 内联函数是一种特殊的函数，具有普通函数的特征（参数检查，返回类型等）
- 内联函数是对编译器的一种请求，因此编译器可能拒绝这种请求
- 内联函数由 编译器处理，直接将编译后的函数体插入调用的地方
- 宏代码片段 由预处理器处理， 进行简单的文本替换，没有任何编译过程

说明5：

- 现代C++编译器能够进行编译优化，因此一些函数即使没有inline声明，也可能被编译器内联编译
- 另外，一些现代C++编译器提供了扩展语法，能够对函数进行强制内联，如：g++中的`__attribute__((always_inline))`属性

说明6：

- C++中内联编译的限制：
- 不能存在任何形式的循环语句    
- 不能存在过多的条件判断语句
- 函数体不能过于庞大
- 不能对函数进行取址操作
- 函数内联声明必须在调用语句之前

编译器对于内联函数的限制并不是绝对的，内联函数相对于普通函数的优势只是省去了函数调用时压栈，跳转和返回的开销。因此，当函数体的执行开销远大于压栈，跳转和返回所用的开销时，那么内联将无意义。

### 结论：

1）内联函数在编译时直接将函数体插入函数调用的地方
2）inline只是一种请求，编译器不一定允许这种请求
3）内联函数省去了普通函数调用时压栈，跳转和返回的开销 

## 默认参数

C++中可以在函数声明时为参数提供一个默认值，当函数调用时没有指定这个参数的值，编译器会自动用默认值代替

```C++
void myPrint(int x = 3)
{
	printf("x:%d", x);
}
```

函数默认参数的规则

- 只有参数列表后面部分的参数才可以提供默认参数值
- 一旦在一个函数调用中开始使用默认参数值，那么这个参数后的所有参数都必须使用默认参数值

```C++
//默认参数
void printAB(int x = 3)
{
	printf("x:%d\n", x);
}

//在默认参数规则 ，如果默认参数出现，那么右边的都必须有默认参数
void printABC(int a, int b, int x = 3, int y=4, int z = 5)
{
	printf("x:%d\n", x);
}
int main62(int argc, char *argv[])
{
	printAB(2);
	printAB();
	system("pause");
	return 0;
}
```

## 函数占位参数

函数占位参数：占位参数只有参数类型声明，而没有参数名声明；一般情况下，在函数体内部无法使用占位参数。

```C++
int func(int a, int b, int ) 
{
	return a + b;
}

int main01()
{
    //func(1, 2); //可以吗？
	printf("func(1, 2, 3) = %d\n", func(1, 2, 3));

	getchar();	
	return 0;
}
```

## 默认参数和占位参数

可以将占位参数与默认参数结合起来使用

意义：为以后程序的扩展留下线索；兼容C语言程序中可能出现的不规范写法

C++可以声明占位符参数，占位符参数一般用于程序扩展和对C代码的兼容

```C++
int func2(int a, int b, int = 0)
{
	return a + b;
}
void main()
{
	//如果默认参数和占位参数在一起，都能调用起来
	func2(1, 2);
	func2(1, 2, 3);
	system("pause");
}
```

结论：如果默认参数和占位参数在一起，都能调用起来

## 函数重载（Overroad）

### 函数重载概念

- 函数重载(Function Overload)
- 用同一个函数名定义不同的函数
- 当函数名和不同的参数搭配时函数的含义不同

### 函数重载的判断标准

函数重载至少满足下面的一个条件：

- 参数个数不同
- 参数类型不同
- 参数顺序不同

### 函数返回值不是函数重载的判断标准

### 函数重载的调用准则

编译器调用重载函数的准则

- 将所有同名函数作为候选者
- 尝试寻找可行的候选函数
- 精确匹配实参
- 通过默认参数能够匹配实参
- 通过默认类型转换匹配实参
- 匹配失败
- 最终寻找到的可行候选函数不唯一，则出现二义性，编译失败。
- 无法匹配所有候选者，函数未定义，编译失败。

### 函数重载的注意事项

- 重载函数在本质上是相互独立的不同函数（静态链编）
- 重载函数的函数类型是不同的
- 函数返回值不能作为函数重载的依据
- 函数重载是由函数名和参数列表决定的。

### 函数重载遇上函数默认参数

### 函数重载和函数指针结 

- 当使用重载函数名对函数指针进行赋值时
- 根据重载规则挑选与函数指针参数列表一致的候选者
- 严格匹配候选者的函数类型与函数指针的函数类型

```C++
#include <iostream>
using namespace std;

void myfunc(int a)
{
	printf("a:%d \n", a);
}

void myfunc(char *p)
{
	printf("%s \n", p);
}


void myfunc(int a, int b)
{
	printf("a:%d \n", a);
}

void myfunc(char *p1, char *p2)
{
	printf("p1:%s ", p1);
	printf("p2:%s \n", p2);
}

//函数指针 基础的语法
//1声明一个函数类型
typedef void (myTypeFunc)(int a,int b) ;  //int
//myTypeFunc *myfuncp = NULL; //定义一个函数指针 这个指针指向函数的入口地址

//声明一个函数指针类型 
typedef void (*myPTypeFunc)(int a,int b) ;  //声明了一个指针的数据类型 
//myPTypeFunc fp = NULL;  //通过  函数指针类型 定义了 一个函数指针

//定义一个函数指针 变量
void (*myVarPFunc)(int a, int b);

void main()
{
	myPTypeFunc fp; //定义了一个 函数指针 变量  
	fp = myfunc;
	//fp(1);
	//myVarPFunc = myfunc;
	fp(1, 2);

	/*
	{
		char buf1[] = "aaaaafff";
		char buf2[] = "bbbb";
		fp(buf1, buf2);
	}
	*/
	system("pause");
	return ;
}
```