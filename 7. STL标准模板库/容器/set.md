### set/multiset的简介
- set是一个集合容器，其中所包含的元素是唯一的，集合中的元素按一定的顺序排列。元素插入过程是按排序规则插入，所以不能指定插入位置。
- set采用红黑树变体的数据结构实现，红黑树属于平衡二叉树。在插入操作和删除操作上比vector快。
- set不可以直接存取元素。（不可以使用at.(pos)与[]操作符）。
- multiset与set的区别：set支持唯一键值，每个元素值只能出现一次；而multiset中同一值可以出现多次。
- 不可以直接修改set或multiset容器中的元素值，因为该类容器是自动排序的。如果希望修改一个元素值，必须先删除原有的元素，再插入新的元素。
- \#include&lt;set>  
### set/multiset对象的默认构造
```c++
set<int> setInt;                    //一个存放int的set容器。
set<float> setFloat;                //一个存放float的set容器。
set<string> setString;              //一个存放string的set容器。
multiset<int> mulsetInt;            //一个存放int的multi set容器。
multi set<float> multisetFloat;     //一个存放float的multi set容器。
multi set<string>multisetString;    //一个存放string的multi set容器。
```
### set的插入与迭代器

| 函数声明         | 功能描述                 |
| ------------ | -------------------- |
| insert(elem) | 在容器中插入元素             |
| begin()      | 返回容器中第一个数据的迭代器       |
| end()        | 返回容器中最后一个数据之后的迭代器    |
| rbegin()     | 返回容器中倒数第一个元素的迭代器     |
| rend()       | 返回容器中倒数最后一个元素的后面的迭代器 |

```c++
set<int> setInt;
setInt.insert(3);
setInt.insert(1);
setInt.insert(5);
setInt.insert(2);
for(set<int>::iterator it=setInt.begin(); it!=setInt.end(); ++it)
{
   int iItem = *it;
   cout << iItem;    //或直接使用cout<< *it
}
//这样子便顺序输出  1 2 3 5。
//set.rbegin()与set.rend()
```
### Set集合的元素排序
- set&lt;int,less&lt;int> > setIntA;  //该容器是按升序方式排列元素。
- set&lt;int,greater&lt;int>> setIntB;   //该容器是按降序方式排列元素。
- set&lt;int> 相当于 set&lt;int,less&lt;int>>。
- less&lt;int>与greater&lt;int>中的int可以改成其它类型，该类型主要要跟set容纳的数据类型一致。
- 疑问1：less&lt;>与greater&lt;>是什么？
- 疑问2：如果set&lt;>不包含int类型，而是包含自定义类型，set容器如何排序？
- 要解决如上两个问题，需要了解容器的函数对象，也叫伪函数，英文名叫functor。
- 下面将讲解什么是functor，functor的用法。
  使用stl提供的函数对象
```c++
set<int,greater<int>>setIntB;   
setIntB.insert(3);
setIntB.insert(1);
setIntB.insert(5);
setIntB.insert(2);
```
此时容器setIntB就包含了按顺序的5,3,2,1元素

### 函数对象functor的用法
- 尽管函数指针被广泛用于实现函数回调，但C++还提供了一个重要的实现回调函数的方法，那就是函数对象。
- functor，翻译成函数对象，伪函数，运算符，是重载了“()”操作符的普通类对象。从语法上讲，它与普通函数行为类似。
- greater<>与less<>就是函数对象。

下面举出greater&lt;int>的简易实现原理。

```C++
struct greater
{
	bool operator()( const int & iLeft, const int & iRight )
	{
		return(iLeft > iRight); // 如果是实现less<int>的话，这边是写return(iLeft<iRight);
	}
}
```

容器就是调用函数对象的operator()方法去比较两个值的大小。

题目：学生包含学号，姓名属性，现要求任意插入几个学生对象到set容器中，使得容器中的学生按学号的升序排序。

```c++
class CStudent // 学生类
{
public:
	CStudent( intiID, string strName )
	{
		m_iID		= iID;
		m_strName	= strName;
	}

	int	m_iID; // 学号
	string	m_strName; // 姓名
}

// 为保持主题鲜明，本类不写拷贝构造函数，不类也不需要写拷贝构造函数。但大家仍要有考虑拷贝构造函数的习惯。
struct StuFunctor // 函数对象
{
	booloperator() ( const CStudent &stu1, const CStudent &stu2 )
	{
		return(stu1.m_iID < stu2.m_iID);
	}
}

void main()
{
	set<CStudent, StuFunctor> setStu;
	setStu.insert( CStudent( 3, "小张" ) );
	setStu.insert( CStudent( 1, "小李" ) );
	setStu.insert( CStudent( 5, "小王" ) );
	setStu.insert( CStudent( 2, "小刘" ) );
	// 此时容器setStu包含了四个学生对象，分别是按姓名顺序的“小李”，“小刘”，“小张”，“小王”
}
```

### set对象的拷贝构造与赋值

```c++
set(const set &st);                //拷贝构造函数
set& operator=(const set &st);     //重载等号操作符
set.swap(st);                      //交换两个集合容器
set<int>setIntA;
setIntA.insert(3);
setIntA.insert(1);
setIntA.insert(7);
setIntA.insert(5);
setIntA.insert(9);
set<int>setIntB(setIntA);  //1 3 5 7 9
     
set<int>setIntC;
setIntC= setIntA;          //1 3 5 7 9
setIntC.insert(6);
setIntC.swap(setIntA);     //交换
```

### set的大小

- set.size();    //返回容器中元素的数目
- set.empty();   //判断容器是否为空

```c++
set<int>setIntA;
setIntA.insert( 3 );
setIntA.insert( 1 );
setIntA.insert( 7 );
setIntA.insert( 5 );
setIntA.insert( 9 );
if ( !setIntA.empty() )
{
	intiSize = setIntA.size(); // 5
}
```
### set的删除

| 方法声明           | 功能说明                             |
| -------------- | -------------------------------- |
| clear()        | 清除所有元素                           |
| erase(pos)     | 删除pos迭代器所指的元素，返回下一个元素的迭代器        |
| erase(beg,end) | 删除区间[beg,end)的所有元素  ，返回下一个元素的迭代器 |
| erase(elem)    | 删除容器中值为elem的元素                   |

删除区间内的元素

```c++
// setInt是用set<int>声明的容器，现已包含按顺序的1,3,5,6,9,11元素。
set<int>::iterator itBegin = setInt.begin();
++ itBegin;
set<int>::iterator itEnd = setInt.begin();
++ itEnd;
++ itEnd;
++ itEnd;
setInt.erase(itBegin,itEnd);
//此时容器setInt包含按顺序的1,6,9,11四个元素。
// 删除容器中第一个元素
setInt.erase(setInt.begin()); //6,9,11
// 删除容器中值为9的元素
set.erase(9);    
// 删除setInt的所有元素
setInt.clear();               //容器为空
```

### set的查找

| 函数声明              | 功能描述                                     |
| ----------------- | ---------------------------------------- |
| find(elem)        | 查找elem元素，返回指向elem元素的迭代器                  |
| count(elem)       | 返回容器中值为elem的元素个数。对set来说，要么是0，要么是1。对multiset来说，值可能大于1 |
| lower_bound(elem) | 返回第一个>=elem元素的迭代器                        |
| upper_bound(elem) | 返回第一个>elem元素的迭代器                         |
| equal_range(elem) | 返回容器中与elem相等的上下限的两个迭代器。上限是闭区间，下限是开区间，如[beg,end) |

- 以上函数返回两个迭代器，而这两个迭代器被封装在pair中。
- 以下讲解pair的含义与使用方法。

```c++
set<int>setInt;
setInt.insert(3);
setInt.insert(1);
setInt.insert(7);
setInt.insert(5);
setInt.insert(9);
set<int>::iterator itA = setInt.find(5);
intiA = *itA;                   //iA == 5
intiCount = setInt.count(5);    //iCount == 1
set<int>::iterator itB = setInt.lower_bound(5);
set<int>::iterator itC = setInt.upper_bound(5);
intiB = *itB;     //iB == 5
intiC = *itC;     //iC == 7
pair<set<int>::iterator, set<int>::iterator > pairIt = setInt.equal_range(5);  //pair是什么？
```
### pair的使用

- pair译为对组，可以将两个值视为一个单元。
- pair&lt;T1,T2>存放的两个值的类型，可以不一样，如T1为int，T2为float。T1,T2也可以是自定义类型。
- pair.first是pair里面的第一个值，是T1类型。
- pair.second是pair里面的第二个值，是T2类型。

```c++
set<int> setInt;
... //往setInt容器插入元素1,3,5,7,9
pair< set<int>::iterator ,set<int>::iterator > pairIt = setInt.equal_range(5);
set<int>::iterator itBeg = pairIt.first;
set<int>::iterator itEnd = pairIt.second;
//此时 *itBeg==5  而  *itEnd == 7
```
### 小结
- 容器set/multiset的使用方法；
  红黑树的变体，查找效率高，插入不能指定位置，插入时自动排序。

- functor的使用方法；
  类似于函数的功能，可用来自定义一些规则，如元素比较规则。

- pair的使用方法。
  对组，一个整体的单元，存放两个类型(T1,T2，T1可与T2一样)的两个元素。

### 案例

```c++
int x;
scanf( "%ld", &x );
multiset<int>h; // 建立一个multiset类型，变量名是h，h序列里面存的是int类型,初始h为空
while ( x != 0 )
{
	h.insert( x ); // 将x插入h中
	scanf( "%ld", &x );
}
pair< multiset<int>::iterator, multiset<int>::iterator > pairIt = h.equal_range( 22 );
multiset<int>::iterator itBeg = pairIt.first;
multiset<int>::iterator itEnd = pairIt.second;
int	nBeg	= *itBeg;
int	nEnd	= *itEnd;
while ( !h.empty() )                         // 序列非空h.empty()==true时表示h已经空了
{
	multiset<int>::iterator c = h.begin();  // c指向h序列中第一个元素的地址，第一个元素是最小的元素
	printf( "%ld", *c );                    // 将地址c存的数据输出
	h.erase( c );                           // 从h序列中将c指向的元素删除
}
```
```C++
#define  _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include "set"
using namespace std;

//1 集合 元素唯一 自动排序(默认情况下 是从小到大) 不能按照[]方式插入元素 
// 红黑树  

//set元素的添加/遍历/删除基本操作
void main91()
{
	set<int>  set1;
	
	for (int i=0; i<5; i++)
	{
		int tmp = rand();
		set1.insert(tmp);
	}
	//插入元素 重复的
	set1.insert(100);
	set1.insert(100);
	set1.insert(100);
	
	for (set<int>::iterator it=set1.begin(); it!=set1.end(); it++ )
	{
		cout << *it << " ";
	}

	//删除集合 
	while ( !set1.empty())
	{
		set<int>::iterator it = set1.begin();
		cout << *it << " ";
		set1.erase(set1.begin());
	}
}

//2 对基本的数据类型 set能自动的排序 
void main92()
{
	set<int> set1;  
	set<int, less<int>> set2;   //默认情况下是这样 
	set<int, greater<int>> set3;  //从大 到 小

	for (int i=0; i<5; i++)
	{
		int tmp = rand();
		set3.insert(tmp);
	}

	//从大 到 小
	for (set<int, greater<int>>::iterator it = set3.begin(); it != set3.end(); it++  )
	{
		cout << *it << endl;
	}
}

class Student
{
public:
	Student(char *name, int age)
	{
		strcpy(this->name, name);
		this->age = age;
	}
public:
	char name[64];
	int		age;
};

//仿函数 
struct FuncStudent
{
	bool operator()(const Student &left, const Student &right)
	{
		if (left.age < right.age)  //如果左边的小 就返回真 从小到大按照年龄进行排序
		{
			return true;
		}
		else
		{
			return false; 
		}
	}
};

//3 自定义数据类型的排序 仿函数的用法
void main93()
{
	Student s1("s1", 31);
	Student s2("s2", 22);
	Student s3("s3", 44);
	Student s4("s4", 11);
	Student s5("s5", 31);

	set<Student, FuncStudent> set1;
	set1.insert(s1);
	set1.insert(s2);
	set1.insert(s3);
	set1.insert(s4);
	set1.insert(s5); //如果两个31岁 能插入成功  
	//如何知道 插入 的结果

	//遍历
	for (set<Student, FuncStudent>::iterator it=set1.begin(); it!=set1.end(); it++ )
	{
		cout << it->age << "\t" <<  it->name << endl;
	}
}

//typedef pair<iterator, bool> _Pairib;
//4 如何判断 set1.insert函数的返回值
//Pair的用法 
void main94()
{
	Student s1("s1", 31);
	Student s2("s2", 22);
	Student s3("s3", 44);
	Student s4("s4", 11);
	Student s5("s5", 31);

	set<Student, FuncStudent> set1;
	pair<set<Student, FuncStudent>::iterator, bool> pair1 = set1.insert(s1);
	if (pair1.second == true)
	{
		cout << "插入s1成功" << endl;
	}
	else
	{
		cout << "插入s1失败" << endl;
	}

	set1.insert(s2);

	//如何知道 插入 的结果
	pair<set<Student, FuncStudent>::iterator, bool> pair5 = set1.insert(s5); //如果两个31岁 能插入成功  
	if (pair5.second == true)
	{
		cout << "插入s1成功" << endl;
	}
	else
	{
		cout << "插入s1失败" << endl;
	}

	//遍历
	for (set<Student, FuncStudent>::iterator it=set1.begin(); it!=set1.end(); it++ )
	{
		cout << it->age << "\t" <<  it->name << endl;
	}
}


//find查找  equal_range 
//返回结果是一个pair
void main95()
{
	set<int> set1;  

	for (int i=0; i<10; i++)
	{
		set1.insert(i+1);
	}

	//从大 到 小
	for (set<int>::iterator it = set1.begin(); it != set1.end(); it++  )
	{
		cout << *it << " ";
	}
	cout << endl;

	set<int>::iterator it0 =  set1.find(5);
	cout << "it0:" << *it0 << endl;

	int num1 = set1.count(5);
	cout << "num1:" << num1 << endl;

	set<int>::iterator it1 =   set1.lower_bound(5); // 大于等于5的元素 的 迭代器的位置
	cout << "it1:" << *it1 << endl;
	
	set<int>::iterator it2 =   set1.lower_bound(5); // 大于5的元素 的 迭代器的位置
	cout << "it2:" << *it2 << endl;

	//typedef pair<iterator, bool> _Pairib;
	//typedef pair<iterator, iterator> _Pairii;
	//typedef pair<const_iterator, const_iterator> _Paircc;
	//把5元素删除掉
	set1.erase(5); 
	pair<set<int>::iterator, set<int>::iterator>  mypair = set1.equal_range(5);
	set<int>::iterator it3 = mypair.first;
	cout << "it3:" << *it3 << endl;  //5  //6

	set<int>::iterator it4 =  mypair.second; 
	cout << "it4:" << *it4 << endl;  //6  //6
}

void main999()
{
	//main91();
	//main92();
	//main93();
	//main94();
	//main95();
	cout<<"hello..."<<endl;
	system("pause");
	return ;
}
```

### 常用函数

| 方法声明              | 功能说明                                     |
| ----------------- | ---------------------------------------- |
| size()            | 返回容器中元素的数目                               |
| max_size()        | 返回容器的容量                                  |
| empty()           | 判断容器是否为空                                 |
| insert()          | 在容器中插入元素                                 |
| begin()           | 返回容器中第一个数据的迭代器                           |
| end()             | 返回容器中最后一个数据之后的迭代器                        |
| rbegin()          | 返回容器中倒数第一个元素的迭代器                         |
| rend()            | 返回容器中倒数最后一个元素的后面的迭代器                     |
| find(elem)        | 查找elem元素，返回指向elem元素的迭代器                  |
| count(elem)       | 返回容器中值为elem的元素个数。对set来说，要么是0，要么是1。对multiset来说，值可能大于1 |
| lower_bound(elem) | 返回第一个>=elem元素的迭代器                        |
| upper_bound(elem) | 返回第一个>elem元素的迭代器                         |
| equal_range(elem) | 返回容器中与elem相等的上下限的两个迭代器。上限是闭区间，下限是开区间，如[beg,end) |
| clear()           | 清除所有元素                                   |
| erase(pos)        | 删除pos迭代器所指的元素，返回下一个元素的迭代器                |
| erase(beg,end)    | 删除区间[beg,end)的所有元素  ，返回下一个元素的迭代器         |
| erase(elem)       | 删除容器中值为elem的元素                           |