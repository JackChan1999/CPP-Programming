### list简介
- list是一个双向链表容器，可高效地进行插入删除元素。
- list不可以随机存取元素，所以不支持at.(pos)函数与[]操作符。It++(ok) it+5(err)
- \#include <list>  

### list对象的默认构造

list采用采用模板类实现,对象的默认构造形式：list<T>lstT;  如：

```C++
list<int> lstInt;            //定义一个存放int的list容器。
list<float> lstFloat;     //定义一个存放float的list容器。
list<string> lstString;     //定义一个存放string的list容器。
...                                   
//尖括号内还可以设置指针类型或自定义类型。
```

### list头尾的添加移除操作

| 函数声明             | 功能描述         |
| ---------------- | ------------ |
| push_back(elem)  | 在容器尾部加入一个元素  |
| pop_back()       | 删除容器中最后一个元素  |
| push_front(elem) | 在容器开头插入一个元素  |
| pop_front()      | 从容器开头移除第一个元素 |

```c++
list<int>lstInt;
lstInt.push_back(1);
lstInt.push_back(3);
lstInt.push_back(5);
lstInt.push_back(7);
lstInt.push_back(9);
lstInt.pop_front();
lstInt.pop_front();
lstInt.push_front(11);
lstInt.push_front(13);
lstInt.pop_back();
lstInt.pop_back();
// lstInt   {13,11,5}
```

### list的数据存取

| 函数声明    | 功能描述     |
| ------- | -------- |
| front() | 返回第一个元素  |
| back()  | 返回最后一个元素 |

```C++
list<int>lstInt;
lstInt.push_back(1);
lstInt.push_back(3);
lstInt.push_back(5);
lstInt.push_back(7);
lstInt.push_back(9);
intiFront = lstInt.front(); //1
intiBack = lstInt.back();            //9
lstInt.front()= 11;                       //11
lstInt.back()= 19;                       //19
```

### list与迭代器

| 函数声明     | 功能描述                 |
| -------- | -------------------- |
| begin()  | 返回容器中第一个元素的迭代器       |
| end()    | 返回容器中最后一个元素之后的迭代器    |
| rbegin() | 返回容器中倒数第一个元素的迭代器     |
| rend()   | 返回容器中倒数最后一个元素的后面的迭代器 |

```C++
list<int>lstInt;
lstInt.push_back(1);
lstInt.push_back(3);
lstInt.push_back(5);
lstInt.push_back(7);
lstInt.push_back(9);
for(list<int>::iterator it=lstInt.begin(); it!=lstInt.end(); ++it)
{
cout<< *it;
cout<< " ";
}
for(list<int>::reverse_iterator rit=lstInt.rbegin(); rit!=lstInt.rend();++rit)

   {
             cout<< *rit;
             cout<< " ";
   }
```
### list对象的带参数构造

| 函数声明                  | 功能描述                                     |
| --------------------- | ---------------------------------------- |
| list(beg,end)         | 构造函数，将[beg,end)区间中的元素拷贝给本身。注意该区间是左闭右开的区间 |
| list(n,elem)          | 构造函数，将n个elem拷贝给本身                        |
| list(const list &lst) | 拷贝构造函数                                   |

```C++
  list<int>lstIntA;
  lstIntA.push_back(1);
  lstIntA.push_back(3);
  lstIntA.push_back(5);
  lstIntA.push_back(7);
  lstIntA.push_back(9);
  list<int>lstIntB(lstIntA.begin(),lstIntA.end());          //13 5 7 9
  list<int>lstIntC(5,8);                                                                 //88 8 8 8 
  list<int>lstIntD(lstIntA);                                                //13 5 7 9
```
### list的赋值

| 函数声明                             | 功能描述                                    |
| -------------------------------- | --------------------------------------- |
| assign(beg,end)                  | 将[beg, end)区间中的数据拷贝赋值给本身。注意该区间是左闭右开的区间。 |
| assign(n,elem)                   | 将n个elem拷贝赋值给本身                          |
| list& operator=(const list &lst) | 重载等号操作符                                 |
| swap(lst)                        | 将lst与本身的元素互换                            |

```C++
list<int>lstIntA,lstIntB,lstIntC,lstIntD;
  lstIntA.push_back(1);
  lstIntA.push_back(3);
  lstIntA.push_back(5);
  lstIntA.push_back(7);
  lstIntA.push_back(9);
  lstIntB.assign(lstIntA.begin(),lstIntA.end());             //1 3 5 7 9
  lstIntC.assign(5,8);                                                           //88 8 8 8
  lstIntD= lstIntA;                                                               //13 5 7 9
  lstIntC.swap(lstIntD);                                                      //互换
```

### list的大小

| 函数声明              | 功能描述                                     |
| ----------------- | ---------------------------------------- |
| size()            | 返回容器中元素的个数                               |
| empty()           | 判断容器是否为空                                 |
| resize(num)       | 重新指定容器的长度为num，若容器变长，则以默认值填充新位置。如果容器变短，则末尾超出容器长度的元素被删除 |
| resize(num, elem) | 重新指定容器的长度为num，若容器变长，则以elem值填充新位置。如果容器变短，则末尾超出容器长度的元素被删除 |

```C++
list<int>lstIntA;
  lstIntA.push_back(1);
  lstIntA.push_back(3);
  lstIntA.push_back(5);
  if(!lstIntA.empty())
  {
  intiSize = lstIntA.size();             //3
  lstIntA.resize(5);                         //1 3 5 0 0
  lstIntA.resize(7,1);                      //1 3 5 0 0 1 1
  lstIntA.resize(2);                         //1 3
  }
```
### list的插入

| 函数声明                | 功能描述                         |
| ------------------- | ---------------------------- |
| insert(pos,elem)    | 在pos位置插入一个elem元素的拷贝，返回新数据的位置 |
| insert(pos,n,elem)  | 在pos位置插入n个elem数据，无返回值        |
| insert(pos,beg,end) | 在pos位置插入[beg,end)区间的数据，无返回值  |

```C++
  list<int>lstA;
  list<int>lstB;
  lstA.push_back(1);
  lstA.push_back(3);
  lstA.push_back(5);
  lstA.push_back(7);
  lstA.push_back(9);
  lstB.push_back(2);
  lstB.push_back(4);
  lstB.push_back(6);
  lstB.push_back(8);
  lstA.insert(lstA.begin(),11);              //{11, 1, 3, 5, 7, 9}
  lstA.insert(++lstA.begin(),2,33);                 //{11,33,33,1,3,5,7,9}
  lstA.insert(lstA.begin(), lstB.begin() , lstB.end() ); //{2,4,6,8,11,33,33,1,3,5,7,9}
```

### list的删除

| 函数声明           | 功能描述                        |
| -------------- | --------------------------- |
| clear()        | 移除容器的所有数据                   |
| erase(beg,end) | 删除[beg,end)区间的数据，返回下一个数据的位置 |
| erase(pos)     | 删除pos位置的数据，返回下一个数据的位置       |
| remove(elem)   | 删除容器中所有与elem值匹配的元素          |

删除区间内的元素

```C++
lstInt是用list<int>声明的容器，现已包含按顺序的1,3,5,6,9元素。
list<int>::iteratoritBegin=lstInt.begin();
++ itBegin;
list<int>::iteratoritEnd=lstInt.begin();
++ itEnd;
++ itEnd;
++ itEnd;
lstInt.erase(itBegin,itEnd);
//此时容器lstInt包含按顺序的1,6,9三个元素。
假设 lstInt 包含1,3,2,3,3,3,4,3,5,3，删除容器中等于3的元素的方法一
for(list<int>::iteratorit=lstInt.being(); it!=lstInt.end(); )   //小括号里不需写  ++it
{
if(*it == 3)
{
 it  =  lstInt.erase(it);       //以迭代器为参数，删除元素3，并把数据删除后的下一个元素位置返回给迭代器。
  //此时，不执行  ++it
 }
else
 {

++it;
 }
}
删除容器中等于3的元素的方法二
lstInt.remove(3);
删除lstInt的所有元素
lstInt.clear();                      //容器为空
```

### list的反序排列

| 函数声明          | 功能描述                                     |
| ------------- | ---------------------------------------- |
| lst.reverse() | 反转链表，比如lst包含1,3,5元素，运行此方法后，lst就包含5,3,1元素 |

```C++
list<int>lstA;
      
     lstA.push_back(1);
     lstA.push_back(3);
     lstA.push_back(5);
     lstA.push_back(7);
     lstA.push_back(9);
     lstA.reverse();                    //9 7 5 3 1
```

### list常用函数

| 方法声明         | 功能描述                 |
| ------------ | -------------------- |
| assign()     | 赋值                   |
| front()      | 返回第一个元素              |
| back()       | 返回最后一个元素             |
| begin()      | 返回容器中第一个元素的迭代器       |
| end()        | 返回容器中最后一个元素之后的迭代器    |
| rbegin()     | 返回容器中倒数第一个元素的迭代器     |
| rend()       | 返回容器中倒数最后一个元素的后面的迭代器 |
| insert()     | 插入元素                 |
| push_front() | 在容器开头插入一个元素          |
| push_back()  | 在容器尾部加入一个元素          |
| pop_front()  | 从容器开头移除第一个元素         |
| pop_back()   | 删除容器中最后一个元素          |
| erase()      | 删除元素                 |
| remove()     | 删除元素                 |
| clear()      | 清空列表                 |
| merge()      | 合并，打乱顺序              |
| sort()       | 排序                   |
| splice()     | 连接两个列表               |
| reverse()    | 反转列表                 |
| size()       | 获取列表的大小              |
| empty()      | 判断列表是否为空             |
| resize()     | 重新设置大小               |

### 小结
- 容器deque的使用方法，适合在头尾添加移除元素。使用方法与vector类似。
- 容器queue,stack的使用方法，适合队列，堆栈的操作方式。
- 容器list的使用方法，适合在任意位置快速插入移除元素。