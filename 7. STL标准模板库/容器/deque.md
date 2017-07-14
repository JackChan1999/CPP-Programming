### Deque简介

- deque是“double-ended queue”的缩写，和vector一样都是STL的容器，deque是双端数组，而vector是单端的。
- deque在接口上和vector非常相似，在许多操作的地方可以直接替换。
- deque可以随机存取元素，支持索引值直接存取，用[]操作符或at()方法
- deque头部和尾部添加或移除元素都非常快速。但是在中部安插元素或移除元素比较费时。
- \#include <deque>

### deque对象的默认构造

deque采用模板类实现，deque对象的默认构造形式：deque<T>deqT;  

```c++
deque <int> deqInt; //一个存放int的deque容器。
deque <float> deq Float;     //一个存放float的deque容器。
deque <string> deq String;     //一个存放string的deque容器。
...                                    
//尖括号内还可以设置指针类型或自定义类型。 
```

### deque末尾的添加移除操作

| 函数声明             | 功能描述        |
| ---------------- | ----------- |
| push_back(elem)  | 在容器尾部添加一个数据 |
| push_front(elem) | 在容器头部插入一个数据 |
| pop_back()       | 删除容器最后一个数据  |
| pop_front()      | 删除容器第一个数据   |

```c++
deque<int>deqInt;
deqInt.push_back(1);
deqInt.push_back(3);
deqInt.push_back(5);
deqInt.push_back(7);
deqInt.push_back(9);
deqInt.pop_front();
deqInt.pop_front();
deqInt.push_front(11);
deqInt.push_front(13);
deqInt.pop_back();
deqInt.pop_back();
//deqInt { 13,11,5}
```

### deque的数据存取

| 函数声明      | 功能描述                                  |
| --------- | ------------------------------------- |
| at(index) | 返回索引index所指的数据，如果idx越界，抛出out_of_range |
| [index]   | 返回索引index所指的数据，如果idx越界，不抛出异常，直接出错     |
| front()   | 返回第一个数据                               |
| back()    | 返回最后一个数据                              |

```c++
deque<int>deqInt;
deqInt.push_back(1);
deqInt.push_back(3);
deqInt.push_back(5);
deqInt.push_back(7);
deqInt.push_back(9);
intiA = deqInt.at(0);                  //1
intiB = deqInt[1];                       //3
deqInt.at(0)= 99;                       //99
deqInt[1]= 88;                   //88
intiFront = deqInt.front();        //99
intiBack = deqInt.back();         //9
deqInt.front()= 77;                    //77
deqInt.back()= 66;                    //66
```

### deque与迭代器

| 函数声明     | 功能描述                |
| -------- | ------------------- |
| begin()  | 返回容器中第一个元素的迭代器      |
| end()    | 返回容器中最后一个元素之后的迭代器   |
| rbegin() | 返回容器中倒数第一个元素的迭代器    |
| rend()   | 返回容器中倒数最后一个元素之后的迭代器 |

```c++
deque<int> deqInt;
deqInt.push_back(1);
deqInt.push_back(3);
deqInt.push_back(5);
deqInt.push_back(7);
deqInt.push_back(9);
for(deque<int>::iterator it=deqInt.begin(); it!=deqInt.end(); ++it)
{
         cout<< *it;
         cout<< "";
}
//1 3 5 7 9
for(deque<int>::reverse_iterator rit=deqInt.rbegin(); rit!=deqInt.rend();++rit)
{
         cout<< *rit;
         cout<< "";
}
//97 5 3 1
```

### deque对象的带参数构造

| 函数声明                    | 功能描述                                     |
| ----------------------- | ---------------------------------------- |
| deque(beg,end)          | 构造函数，将[beg,end)区间中的元素拷贝给本身。注意该区间是左闭右开的区间。 |
| deque(n,elem)           | 构造函数，将n个elem拷贝给本身                        |
| deque(const deque &deq) | 拷贝构造函数                                   |

```c++
deque<int> deqIntA;
deqIntA.push_back(1);
deqIntA.push_back(3);
deqIntA.push_back(5);
deqIntA.push_back(7);
deqIntA.push_back(9);
deque<int>deqIntB(deqIntA.begin(),deqIntA.end()); //13 5 7 9
deque<int>deqIntC(5,8);                                                                 //88 8 8 8
deque<int>deqIntD(deqIntA);//13 5 7 9
```

### deque的赋值

| 函数声明                               | 功能描述                                    |
| ---------------------------------- | --------------------------------------- |
| assign(beg,end)                    | 将[beg, end)区间中的数据拷贝赋值给本身。注意该区间是左闭右开的区间。 |
| assign(n,elem)                     | 将n个elem拷贝赋值给本身                          |
| deque& operator=(const deque &deq) | 重载等号操作符                                 |
| swap(deq)                          | 将vec与本身的元素互换                            |

```c++
deque<int> deqIntA,deqIntB,deqIntC,deqIntD;
deqIntA.push_back(1);
deqIntA.push_back(3);
deqIntA.push_back(5);
deqIntA.push_back(7);
deqIntA.push_back(9);
deqIntB.assign(deqIntA.begin(),deqIntA.end());     // 1 3 5 7 9
                   
deqIntC.assign(5,8);                                                        //88 8 8 8
deqIntD= deqIntA;                                                         //13 5 7 9
deqIntC.swap(deqIntD);                                                //互换
```

### deque的大小

| 函数声明              | 功能描述                                     |
| ----------------- | ---------------------------------------- |
| size()            | 返回容器中元素的个数                               |
| empty()           | 判断容器是否为空                                 |
| resize(num)       | 重新指定容器的长度为num，若容器变长，则以默认值填充新位置。如果容器变短，则末尾超出容器长度的元素被删除。 |
| resize(num, elem) | 重新指定容器的长度为num，若容器变长，则以elem值填充新位置。如果容器变短，则末尾超出容器长度的元素被删除。 |

```c++
deque<int>deqIntA;
deqIntA.push_back(1);
deqIntA.push_back(3);
deqIntA.push_back(5);
intiSize = deqIntA.size();  //3
if(!deqIntA.empty())
{
deqIntA.resize(5);             //1 3 5 0 0
deqIntA.resize(7,1); //1 3 5 0 0 1 1
deqIntA.resize(2);             //1 3
}
```

### deque的插入

- deque.insert(pos,elem);   //在pos位置插入一个elem元素的拷贝，返回新数据的位置。
- deque.insert(pos,n,elem);   //在pos位置插入n个elem数据，无返回值。
- deque.insert(pos,beg,end);  //在pos位置插入[beg,end)区间的数据，无返回值。

```c++
deque<int>deqA;
deque<int>deqB;
deqA.push_back(1);
deqA.push_back(3);
deqA.push_back(5);
deqA.push_back(7);
deqA.push_back(9);
deqB.push_back(2);
deqB.push_back(4);
deqB.push_back(6);
deqB.push_back(8);

deqA.insert(deqA.begin(),11);                 //{11, 1, 3, 5, 7, 9}
deqA.insert(deqA.begin()+1,2,33);           //{11,33,33,1,3,5,7,9}
deqA.insert(deqA.begin(), deqB.begin() , deqB.end() );         //{2,4,6,8,11,33,33,1,3,5,7,9}
```
### deque的删除

- deque.clear();    //移除容器的所有数据
- deque.erase(beg,end);  //删除[beg,end)区间的数据，返回下一个数据的位置。
- deque.erase(pos);    //删除pos位置的数据，返回下一个数据的位置。
  删除区间内的元素
```c++
deqInt是用deque<int>声明的容器，现已包含按顺序的1,3,5,6,9元素。
deque<int>::iteratoritBegin=deqInt.begin()+1;
deque<int>::iteratoritEnd=deqInt.begin()+3;
deqInt.erase(itBegin,itEnd);
//此时容器deqInt包含按顺序的1,6,9三个元素。
假设 deqInt 包含1,3,2,3,3,3,4,3,5,3，删除容器中等于3的元素
for(deque<int>::iterator it=deqInt.being();it!=deqInt.end(); )    //小括号里不需写  ++it
{
  if(*it == 3)
   {
       it  =  deqInt.erase(it);       //以迭代器为参数，删除元素3，并把数据删除后的下一个元素位置返回给迭代器。
        //此时，不执行  ++it；  
   }
  else
   {
      ++it;
   }
}
//删除deqInt的所有元素
deqInt.clear();                    //容器为空
```
### deque常用函数

| 方法声明         | 功能描述         |
| ------------ | ------------ |
| assign()     | 赋值           |
| at()         | 取元素          |
| front()      | 获取第一个元素      |
| back()       | 获取最后一个元素     |
| begin()      | 获取迭代器        |
| end()        | 最后一个位置的下一个位置 |
| insert()     | 插入           |
| push_front() | 头插入          |
| push_back()  | 尾插入          |
| erase()      | 删除           |
| pop_front()  | 删除第一个元素      |
| pop_back()   | 删除最后一个元素     |