### map/multimap的简介
- map是标准的关联式容器，一个map是一个键值对序列，即(key,value)对。它提供基于key的快速检索能力。
- map中key值是唯一的。集合中的元素按一定的顺序排列。元素插入过程是按排序规则插入，所以不能指定插入位置。
- map的具体实现采用红黑树变体的平衡二叉树的数据结构。在插入操作和删除操作上比vector快。
- map可以直接存取key所对应的value，支持[]操作符，如map[key]=value。
- multimap与map的区别：map支持唯一键值，每个键只能出现一次；而multimap中相同键可以出现多次。multimap不支持[]操作符。
- \#include&lt;map>  

### map/multimap对象的默认构造
```c++
// map/multimap采用模板类实现，对象的默认构造形式：
map<T1,T2> mapTT; 
multimap<T1,T2>  multimapTT; 

map<int, char> mapA;
map<string,float> mapB;
//其中T1,T2还可以用各种指针类型或自定义类型
```

### map的插入与迭代器

- map.insert(...);    //往容器插入元素，返回pair<iterator,bool>
- 在map中插入元素的三种方式：

假设  map<int, string> mapStu;

- 通过pair的方式插入对象
  mapStu.insert(  pair<int,string>(3,"小张")  );
- 通过pair的方式插入对象
  mapStu.inset(make_pair(-1,“校长-1”));
- 通过value_type的方式插入对象
  mapStu.insert(  map<int,string>::value_type(1,"小李")  );
- 通过数组的方式插入值
  mapStu[3] = “小刘";
  mapStu[5] = “小王"；
  ​         
- 前三种方法，采用的是insert()方法，该方法返回值为pair<iterator,bool> 
- 第四种方法非常直观，但存在一个性能的问题。插入3时，先在mapStu中查找主键为3的项，若没发现，则将一个键为3，值为初始化值的对组插入到mapStu中，然后再将值修改成“小刘”。若发现已存在3这个键，则修改这个键对应的value。
- string strName = mapStu[2];  //取操作或插入操作
- 只有当mapStu存在2这个键时才是正确的取操作，否则会自动插入一个实例，键为2，值为初始化值。

```c++
map<int, string> mapA;
pair< map<int, string>::iterator, bool > pairResult =
mapA.insert( pair<int, string>( 3, "小张" ) ); // 插入方式1
int	iFirstFirst	= (pairResult.first)->first; // iFirst== 3;
string	strFirstSecond	= (pairResult.first)->second; // strFirstSecond为"小张"
bool bSecond = pairResult.second;
// bSecond== true;
mapA.insert( map<int, string>::value_type( 1, "小李" ) ); // 插入方式2
mapA[3] = "小刘";  // 修改value
mapA[5] = "小王";  // 插入方式3
string	str1	= mapA[2]; // 执行插入 string()操作，返回的str1的字符串内容为空。
string	str2	= mapA[3]; // 取得value，str2为"小刘"
// 迭代器遍历
for ( map<int, string>::iterator it = mapA.begin(); it != mapA.end(); ++it )
{
	pair<int, string> pr = *it;
	intiKey		= pr.first;
	stringstrValue	= pr.second;
}
// map.rbegin()与map.rend()
```

- map<T1,T2,less<T1> > mapA;  //该容器是按键的升序方式排列元素。未指定函数对象，默认采用less<T1>函数对象。
- map<T1,T2,greater<T1>> mapB;   //该容器是按键的降序方式排列元素。
- less<T1>与greater<T1>  可以替换成其它的函数对象functor。
- 可编写自定义函数对象以进行自定义类型的比较，使用方法与set构造时所用的函数对象一样。
- map.begin();  //返回容器中第一个数据的迭代器。
- map.end();  //返回容器中最后一个数据之后的迭代器。
- map.rbegin();  //返回容器中倒数第一个元素的迭代器。
- map.rend();   //返回容器中倒数最后一个元素的后面的迭代器。

### map对象的拷贝构造与赋值

```c++
map(const map &mp); //拷贝构造函数
map& operator=(const map &mp); //重载等号操作符
map.swap(mp);  //交换两个集合容器

map<int,string> mapA;
mapA.insert(pair<int,string>(3,"小张"));         
mapA.insert(pair<int,string>(1,"小杨"));         
mapA.insert(pair<int,string>(7,"小赵"));         
mapA.insert(pair<int,string>(5,"小王"));         
map<int,string> mapB(mapA);  //拷贝构造              
map<int,string> mapC;
mapC= mapA; //赋值
mapC[3]= "老张";
mapC.swap(mapA);  //交换
```

### map的大小

- map.size(); //返回容器中元素的数目
- map.empty();//判断容器是否为空
```c++
map<int, string> mapA;
mapA.insert( pair<int, string>( 3, "小张" ) );
mapA.insert( pair<int, string>( 1, "小杨" ) );
mapA.insert( pair<int, string>( 7, "小赵" ) );
mapA.insert( pair<int, string>( 5, "小王" ) );
if ( mapA.empty() )
{
	intiSize = mapA.size(); // iSize== 4
}
```

### map的删除

- map.clear();    //删除所有元素
- map.erase(pos); //删除pos迭代器所指的元素，返回下一个元素的迭代器。
- map.erase(beg,end);     //删除区间[beg,end)的所有元素  ，返回下一个元素的迭代器。
- map.erase(keyElem);     //删除容器中key为keyElem的对组。

```c++
map<int, string> mapA;
mapA.insert(pair<int,string>(3,"小张"));         
mapA.insert(pair<int,string>(1,"小杨"));         
mapA.insert(pair<int,string>(7,"小赵"));         
mapA.insert(pair<int,string>(5,"小王"));   

//删除区间内的元素
map<int,string>::iterator itBegin = mapA.begin();
++itBegin;
++itBegin;
map<int,string>::iterator itEnd = mapA.end();
//此时容器mapA包含按顺序的{1,"小杨"}{3,"小张"}两个元素。
mapA.erase(itBegin,itEnd);  
mapA.insert(pair<int,string>(7,"小赵"));         
mapA.insert(pair<int,string>(5,"小王"));      

//删除容器中第一个元素
//此时容器mapA包含了按顺序的{3,"小张"}{5,"小王"}{7,"小赵"}三个元素
mapA.erase(mapA.begin());             
//删除容器中key为5的元素
mapA.erase(5);    
//删除mapA的所有元素
mapA.clear();//容器为空
```

### map的查找

- map.find(key);   查找键key是否存在，若存在，返回该键的元素的迭代器；若不存在，返回map.end();
- map.count(keyElem);   //返回容器中key为keyElem的对组个数。对map来说，要么是0，要么是1。对multimap来说，值可能大于1。

### map的基本操作

```c++
#include <iostream>
#include "map"
#include "string"
using namespace std;

//map元素的添加/遍历/删除基本操作
void main1()
{
	map<int, string> map1;

	//方法1
	map1.insert(pair<int, string>(1,"teacher01") );
	map1.insert(pair<int, string>(2,"teacher02") );

	//方法2 
	map1.insert(make_pair(3, "teacher04") );
	map1.insert(make_pair(4, "teacher05") );

	//方法3 
	map1.insert(map<int, string>::value_type(5, "teacher05") );
	map1.insert(map<int, string>::value_type(6, "teacher06") );

	//方法4
	map1[7] = "teacher07";
	map1[8] = "teacher08";
	
	//map1['z'] = "teacher08";

	//容器的遍历
	for (map<int, string>::iterator it = map1.begin(); it!=map1.end(); it++ )
	{
		cout << it->first << "\t" << it->second << endl;
	}
	cout << "遍历结束" << endl;

	//容器元素的删除
	while (!map1.empty())
	{
		map<int, string>::iterator it = map1.begin();
		cout << it->first << "\t" << it->second << endl;
		map1.erase(it);
	}
}

//插入的四种方法异同
//前三种方法，返回值为pair<iterator,bool> 若key已经存在 则报错
//方法四，若key已经存在,则修改									
void main2()
{
	map<int, string> map1;
	//typedef pair<iterator, bool> _Pairib;

	//方法1
	pair<map<int, string>::iterator, bool>  mypair1 = 
      map1.insert(pair<int, string>(1,"teacher01") );
	map1.insert(pair<int, string>(2,"teacher02") );

	//方法2 
	pair<map<int, string>::iterator, bool>  mypair3 = map1.insert(make_pair(3, "teacher04") );
	map1.insert(make_pair(4, "teacher05") );

	//方法3 
	pair<map<int, string>::iterator, bool>  mypair5 = 
      map1.insert(map<int, string>::value_type(5, "teacher05") );
	if (mypair5.second != true)
	{
		cout << "key 5 插入失败" << endl;
	}
	else
	{
		cout << mypair5.first->first << "\t" <<  mypair5.first->second <<endl;
	}

	pair<map<int, string>::iterator, bool>  mypair6 =  
      map1.insert(map<int, string>::value_type(5, "teacher55") );
	if (mypair6.second != true)
	{
		cout << "key 5 插入失败" << endl;
	}
	else
	{
		cout << mypair6.first->first << "\t" <<  mypair6.first->second <<endl;
	}

	//方法4
	map1[7] = "teacher07";
	map1[7] = "teacher77";

	//容器的遍历
	for (map<int, string>::iterator it = map1.begin(); it!=map1.end(); it++ )
	{
		cout << it->first << "\t" << it->second << endl;
	}
	cout << "遍历结束" << endl;
}

void main3()
{
	map<int, string> map1;

	//方法1
	map1.insert(pair<int, string>(1,"teacher01") );
	map1.insert(pair<int, string>(2,"teacher02") );

	//方法2 
	map1.insert(make_pair(3, "teacher04") );
	map1.insert(make_pair(4, "teacher05") );

	//方法3 
	map1.insert(map<int, string>::value_type(5, "teacher05") );
	map1.insert(map<int, string>::value_type(6, "teacher06") );

	//方法4
	map1[7] = "teacher07";
	map1[8] = "teacher08";

	//容器的遍历
	for (map<int, string>::iterator it = map1.begin(); it!=map1.end(); it++ )
	{
		cout << it->first << "\t" << it->second << endl;
	}
	cout << "遍历结束" << endl;

	//map的查找 //异常处理
	map<int, string>::iterator it2 = map1.find(100);
	if (it2 == map1.end())
	{
		cout << "key 100 的值 不存在" << endl;
	}
	else
	{
		cout << it2->first << "\t" << it2->second << endl;
	}

	//equal_range //异常处理
	pair<map<int, string>::iterator , map<int, string>::iterator> mypair = 
      map1.equal_range(5); //返回两个迭代器 形成一个 pair
	//第一个迭代器 >= 5的 位置 
	//第一个迭代器 = 5的 位置 

	if (mypair.first == map1.end() )
	{
		cout << "第一个迭代器 >= 5的 位置 不存在" << endl;
	}
	else
	{
		cout << mypair.first->first << "\t" << mypair.first->second << endl;
	}

	//使用第二个迭代器
	if (mypair.second == map1.end() )
	{
		cout << "第二个迭代器 > 5的 位置 不存在" << endl;
	}
	else
	{
		cout << mypair.second->first << "\t" << mypair.second->second << endl;
	}
}
void main1104()
{
	//main1();
	//main2();
	main3();
	system("pause");
	return ;
}
```
### multimap 案例

- 1个key值可以对应多个valude  ==> 分组 
- 公司有销售部 sale （员工2名）、技术研发部 development （1人）、财务部 Financial （2人） 
- 人员信息有：姓名，年龄，电话、工资等组成
- 通过 multimap进行信息的插入、保存、显示
- 分部门显示员工信息 

```c++
#include <iostream>
#include "map"
#include "string"
using namespace std;

class Person
{
public:
	string	name;
	int		age;
	string	tel;
	double	saly;
};

void main1()
{
	Person p1, p2, p3, p4, p5;

	p1.name = "王1";
	p1.age = 31;

	p2.name = "王2";
	p2.age = 32;

	p3.name = "张3";
	p3.age = 33;

	p4.name = "张4";
	p4.age = 34;

	p5.name = "赵5";
	p5.age = 35;

	multimap<string, Person> map2;
	//sale部门
	map2.insert(make_pair("sale", p1) );
	map2.insert(make_pair("sale", p2) );

	//development 部门
	map2.insert(make_pair("development", p3) );
	map2.insert(make_pair("development", p4) );

	//Financial 部门
	map2.insert(make_pair("Financial", p5) );

	for( multimap<string, Person>::iterator it=map2.begin(); it!=map2.end(); it++)
	{
		cout << it->first << "\t" << it->second.name << endl;
	}
	cout << "遍历结束" << endl;

	int num2 = map2.count("development");
	cout << "development部门人数==>" << num2 << endl;

	cout << "development部门员工信息" << endl;
	multimap<string, Person>::iterator it2 = map2.find("development");
	
	int tag = 0;
	while (it2 != map2.end() && tag < num2)
	{
		cout << it2->first << "\t" << it2->second.name << endl;
		it2 ++;
		tag ++;
	}
}

//age = 32修改成 name32
void main2()
{
	Person p1, p2, p3, p4, p5;

	p1.name = "王1";
	p1.age = 31;

	p2.name = "王2";
	p2.age = 32;

	p3.name = "张3";
	p3.age = 33;

	p4.name = "张4";
	p4.age = 34;

	p5.name = "赵5";
	p5.age = 35;

	multimap<string, Person> map2;
	//sale部门
	map2.insert(make_pair("sale", p1) );
	map2.insert(make_pair("sale", p2) );

	//development 部门
	map2.insert(make_pair("development", p3) );
	map2.insert(make_pair("development", p4) );

	//Financial 部门
	map2.insert(make_pair("Financial", p5) );

	cout << "\n按照条件 检索数据 进行修改 " << endl;
	for( multimap<string, Person>::iterator it=map2.begin(); it!=map2.end(); it++)
	{
		//cout << it->first << "\t" << it->second.name << endl;
		if (it->second.age == 32 )
		{
			it->second.name = "name32";
		}
	}
	
	for( multimap<string, Person>::iterator it=map2.begin(); it!=map2.end(); it++)
	{
		cout << it->first << "\t" << it->second.name << endl;
	}
}

void main()
{
	//main1();
	main2();
	system("pause");
	return ;
}
```