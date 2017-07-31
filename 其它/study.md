printf()  scanf() 操作的是屏幕

fprintf()  fscanf() 操作的是文件

- fprintf()  输出到文件
- fscanf()  从文件读取

sprintf()  sscanf() 操作的是字符串

- sprintf()  输出到连续空间或数组
- sscanf()  从连续空间或数组输入

wprintf()

typeid(var).name() 获取变量数据类型

decltype() 通用的备份接口

C语言把设备当作文件处理

结合顺序，优先级的前提是必须要接触 ++*p

二叉树集合了数组和链表的优点，查询快，增删也快

## 面试

- 链表逆转（必考）
- 找出二叉树的公共祖先（必考）

## 数据存储结构

- 顺序存储
- 链式存储
- 索引存储
- 哈希存储

数据的逻辑结构与存储结构是密切相关的

## 命名空间

```
namespace A1{
	int a = 10;
}
```

使用

```
using namespace A1;// 用法3
cout<<A1::a<<endl;// 用法2
using A1::a;// 用法1
cout<<a;
```

### 标准命名空间std

C++标准库中的所有标识符都被定义于一个名为std的namespace中

## C++对C语言的扩充

增加了bool类型

### 类型转换

- static_cast<>

最常用的类型转换操作符，主要用于非多态的类型转换

- const_cast<>

在进行类型转换时用来修改类型的const或volatile属性，除了const或volatile修饰之外，原来的数据值和数据类型都是不变的

- dynamic_cast<>

该操作符用于运行时检查类型转换是否安全

- reinterpret_cast<>

通常为操作数的位模式提供较低层次的重新解释。如果将一个类型（int类型）的数据a转换为另一个类型（double类型）的数据b时仅仅是将a的比特位复制给b，不作数据转换，也进行类型检查。而且reinterpret_cast要转换的new_type类型必须是指针类型、引用或算术类型

### 字符串string

- length()
- size()
- swap()

### 默认参数

```
int add(int x, int y = 1);
```

### 内联函数

在编译时将函数体嵌入到调用处，省去了函数调用的开销

```
inline int add(int x, int y){
  	return x + y;
}
```

### 重载函数

重载与const形参

## 引用

引用是隐式的指针

```
int a = 10；
int &b = a;
// const引用
const int &c = a;
```

## 动态分配内存

- new 创建对象时返回的是直接带类型信息的指针，会自动执行构造函数进行初始化，从自由存储区（free store）分配内存
- delete 会自动执行析构函数
- malloc() 返回的是void *类型的指针，从堆区（heap）分配内存
- free()

## 指针

- 常指针
- 数组指针
- 指针数组
- 指针函数
- 函数指针

## 面向对象

- this指针
- 构造函数  初始化
- 拷贝构造函数 Person(Person &p)
  - 使用类对象的引用作为参数的构造函数
  - 用一个对象初始化另一个对象
- 析构函数  释放内存空间
- 虚析构函数
- 虚函数
- 纯虚函数
- 在.h中声明函数，在.cpp中实现函数
- 静态成员，访问方式Person::age
- 静态成员函数，调用方式Person::getAge()
- 常函数 int add(int x, int y) const {}
- 类的内联函数应与类的定义一起存放在类的头文件中
- 虚基类

### 深拷贝与浅拷贝

### 友元

- 友元函数
  - 在类外定义的一个函数
  - 在类中声明某一函数为友元，则该函数可以操作类中的私有数据
  - friend int add(IntClass &)
  - 类中的成员函数作为另一个的友元函数，friend void IntAugend::add(IntAddend &)
- 友元类
  - 将一个类声明为另一个类的友元类
  - 在类A中声明类B为友元类，则类B中的所有成员函数称为类A的友元函数，能访问类A中的私有数据

### 继承与多态

多态是设计模式的基础，多态的三个条件是：继承，虚函数，父类指针或引用指向子类对象

- 多重继承的二义性

调用不同基类中的同名成员时产生二义性，解决方法：1.使用作用域限定符；2.派生类中定义与基类同名函数，将基类函数隐藏，但覆盖会产生新的问题，需要用虚函数来解决

派生类中访问公有成员时产生二义性：多重继承中派生类有多个基类，多个基类又可能由同一个基类派生，则在派生类中访问公共基类成员时会出现二义性，数据成员会出现多分拷贝，公共基类的构造函数被调用多次，解决方法是将基类声明为虚基类

- 虚基类

在多次继承中，若一个类声明为虚基类，则能保证一个派生类间接地多次继承该类时，派生类中只继承该基类的一份成员，避免了派生类中访问公共基类公有属性多份拷贝的二义性

```
class Bird:virtual public Animal{
  	// 虚基类表指针vbptr，偏移量表
}
```

- 多态

静态联编，动态联编。虚函数，动态绑定机制

虚函数的实现机制：编译器提前布局，编译器在执行过程中遇到virtual关键字时，会为这些包含虚函数的类建立一张虚函数表vtable，在虚函数表中，编译器将按照虚函数的声明顺序依次保存虚函数地址，同时在每一个带有虚函数的类中放置一个虚函数指针vptr，用来指向虚函数表。通常在定义类对象时，为vptr分配空间，该指针被置于对象的起始位置，继而通过对象的构造函数将vptr初始化为类的虚函数表地址

虚析构函数，为了解决基类指针指向派生类对象，并用基类指针销毁派生类的引用产生的。虚析构函数会释放基类指针指向的对象时会调用基类及派生类的析构函数，派生类中的所有资源被回收。

```
virtual ~Person();
```

纯虚函数，作用是在基类中为派生类保留一个函数接口，方便派生类根据需要对它进行实现，实现多态

```
virtual int printf() = 0;
```

### 抽象类

包含至少一个纯虚函数的类称为抽象类。抽象类的作用是通过它为一个类群建立一个公共的接口，使它们能够有效发挥多态特性。

## 运算符重载

运算符重载是对已有的运算符赋予多重含义，使得一个运算符作用于不同的数据类型时做出不同的行为

```
int operator=();
```

### 运算符重载方式

- 重载为类的成员函数

含有this指针，双目运算符，左操作数，右操作数

```
a1.operator+(a2) // 双目运算符
a1.operator++() // 单目运算符
```

- 重载为类的友元函数

加friend关键字即可，没有this指针，因此操作数的个数没有变化，所有的操作数都必须通过函数的形参进行传递，函数参数与操作数自左至右保持一一对应

```
friend int operator+(const A& a1, const A& a2);
```

### 输入输出运算符的重载

### 赋值运算符重载

默认的赋值运算符和默认的拷贝构造函数一样实现的是浅拷贝，需要实现深拷贝，必须重载赋值运算符

先释放(delete)原来的空间 --> 在重新申请(new)空间 --> 拷贝数据到新的空间

### 类型转换函数

## IO流

## 异常与断言

## 模板

泛型编程，参数类型化

### 函数模板

```
template<typename T>
T add(T t1, T t2){
  	return t1 + t2;
}
// 显式实例化
// 可以为同一个模板形参指定两种不同的数据类型
template int add<int>(int t1, int t2);
```

函数模板的重载

### 类模板

```
template<typename T>
class A{
  	public:
  	T t1;
  	T t2;
  	T add(T x, T y);
}
A<int> a; // 创建对象
```

### 模板特化

所谓特化就是将泛型的东西具体化

- 偏特化
- 全特化

## STL

13个头文件

- algorithm
- deque
- functional
- interator
- vector
- list
- map
- memory
- numeric
- set
- statck
- queue
- utility，定义了pair类模板，键值对

### 容器container：保存其他对象的对象

- vector 向量，底层数据结构是动态分配的数组，增删慢，查询快
  - 下标[i]访问
  - capacity()
  - size()
  - assign() 赋值
  - at()
  - push_back()
  - pop_back()
  - front()
  - back()
  - begin() 返回头迭代器
  - end() 返回尾迭代器
  - insert()
  - erase() 删除
- list 双向链表容器，链表实现，增删快，查询慢，不能随机访问
  - remove(elem) 删除所有与elem匹配的元素
  - merge() 合并列表，重新排序
  - splice() 合并列表，不打乱顺序
  - sort()
  - rend()
  - rbegin()
- queue 队列容器
- deque 双端队列，底层数据结构是分段数组，索引数组存放分段数组的首地址
  - push_front()
  - pop_back()
- priority 优先级队列容器
- statck 栈容器

关联型容器，实现为二叉树

- set 集合容器，唯一
  - size()
  - max_size()
  - empty()
  - find() 返回key元素的位置
  - count() 返回元素elem的个数
  - begin()
  - end()
  - insert()
  - erase()
- multiset 可重复的集合容器
- map 键值对容器，键唯一
  - m.at(key)
  - m[key]
  - size()
  - max_size()
  - count()
  - begin()
  - end()
  - insert()
  - erase()
- multimap key可重复的键值对容器

### 迭代器interator：将容器和算法联系起来

- 输入迭代器Inputiterator
- 输出迭代器Outputiterator
- 前向迭代器Forwarditerator
- 双向迭代器
- 随机访问迭代器

基本操作

- 获取当前元素，->或`*`
- 指向下一个元素++
- 相等==
- distance()

### 迭代器适配器

reverse_iterator 逆向迭代器

- base() 转为正常的迭代器

迭代器辅助函数

- advance() 前进
- distance() 后退
- iter_swap() 交换

### 算法algorithm，将数据和操作数据的算法分离

算法实际上是一系列的函数模板

算法的分类

- 不可变序列算法
- 可变序列算法
- 排序算法
- 数值算法

常用算法

- for_each()
- find()
- copy()
- sort()
- accumulate() 累加

## 设计模式

## C++11新特性