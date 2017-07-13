## register关键字增强

```c
int main22()
{
	register int a = 0; 
	printf("&a = %x\n", &a);

	system("pause");
	return 0;
}
```

register关键字 请求编译器让变量a直接放在寄存器里面，速度快

在c语言中 register修饰的变量不能取地址，但是在c++里面做了内容

### register关键字的变化

- register关键字请求“编译器”将局部变量存储于寄存器中
- C语言中无法取得register变量地址
- 在C++中依然支持register关键字
- C++编译器有自己的优化方式，不使用register也可能做优化
- C++中可以取得register变量的地址
- C++编译器发现程序中需要取register变量的地址时，register对变量的声明变得无效。
- 早期C语言编译器不会对代码进行优化，因此register变量是一个很好的补充。

## 变量检测增强

在C语言中，重复定义多个同名的全局变量是合法的。

在C++中，不允许定义多个同名的全局变量。

C语言中多个同名的全局变量最终会被链接到全局数据区的同一个地址空间上。

C++直接拒绝这种二义性的做法。

```c
int g_var;
int g_var = 1;
int main(int argc, char *argv[])
{
	printf("g_var = %d\n", g_var);
	return 0;
}
```

## struct类型加强

C语言的struct定义了一组变量的集合，C编译器并不认为这是一种新的类型。C++中的struct是一个新类型的定义声明

```C++
struct Student
{
    char name[100];
    int age;
};

int main(int argc, char *argv[])
{
    Student s1 = {"wang", 1};
    Student s2 = {"wang2", 2};    
    return 0;
}
```

## C++中所有的变量和函数都必须有类型

C++中所有的变量和函数都必须有类型，C语言中的默认类型在C++中是不合法的。

在C语言中，`int f();`表示返回值为int，接受任意参数的函数，`int f(void);`表示返回值为int的无参函数。

在C++中，`int f();`和`int f(void)`具有相同的意义，都表示返回值为int的无参函数。

C++更加强调类型，任意的程序元素都必须显示指明类型

## 新增Bool类型关键字

C++中的布尔类型

- C++在C语言的基本类型系统之上增加了bool
- C++中的bool可取的值只有true和false
- 理论上bool只占用一个字节，如果多个bool变量定义在一起，可能会各占一个bit，这取决于编译器的实现
- true代表真值，编译器内部用1来表示
- false代表非真值，编译器内部用0来表示
- bool类型只有true（非0）和false（0）两个值
- C++编译器会在赋值时将非0值转换为true，0值转换为false

## 三目运算符功能增强

### 三目运算符在C和C++编译器的表现

```c
int main()
{
    int a = 10;
    int b = 20;

    //返回一个最小数 并且给最小数赋值成3
    //三目运算符是一个表达式 ，表达式不可能做左值
    (a < b ? a : b )= 30;

    printf("a = %d, b = %d\n", a, b); 

    system("pause");

    return 0;
}
```

（1）C语言返回变量的值，C++语言是返回变量本身

- C语言中的三目运算符返回的是变量值，不能作为左值使用
- C++中的三目运算符可直接返回变量本身，因此可以出现在程序的任何地方

（2）注意：三目运算符可能返回的值中如果有一个是常量值，则不能作为左值使用

`(a < b ? 1 : b )= 30;`

(3）C语言如何支持类似C++的特性呢？

当左值的条件：要有内存空间；C++编译器帮助程序员取了一个地址而已

思考：如何让C中的三目运算法当左值呢？

## C/C++中的const

### const基础知识

初级理解：const是定义常量 --> const意味着只读

```c
int main()
{
const int a;
int const b;

const int *c;
int * const d;
const int * const e ;

return 0;
}

Int func1(const )
```

- 第一个第二个意思一样 代表一个常整形数
- 第三个 c是一个指向常整形数的指针(所指向的内存数据不能被修改，但是本身可以修改)
- 第四个 d 常指针（指针变量不能被修改，但是它所指向内存空间可以被修改）
- 第五个 e一个指向常整形的常指针（指针和它所指向的内存空间，均不能被修改）

Const好处

- 合理的利用const
- 指针做函数参数，可以有效的提高代码可读性，减少bug
- 清楚的分清参数的输入和输出特性

Const修改形参的时候，在利用形参不能修改指针所向的内存空间
```c
int setTeacher_err( const Teacher *p)
```



