### Map和multimap容器
#### map/multimap的简介
- map是标准的**关联式**容器，一个map是一个键值对序列，即(key,value)对。它提供基于key的快速检索能力。
- map中**key****值是唯一的**。集合中的元素按一定的**顺序**排列。元素插入过程是按排序规则插入，所以不能指定插入位置。
- map的具体实现采用红黑树变体的平衡二叉树的数据结构。在插入操作和删除操作上比vector快。
- map可以直接存取key所对应的value，支持[]操作符，如map[key]=value。
- multimap与map的区别：map支持唯一键值，每个键只能出现一次；而multimap中相同键可以出现多次。multimap不支持[]操作符。
- #include <map>  
  **map/multimap****对象的默认构造******
  map/multimap采用模板类实现，对象的默认构造形式：
  map<T1,T2> mapTT; 
  multimap<T1,T2>  multimapTT; 
  如：
  map<int, char> mapA;
  map<string,float> mapB;
  //其中T1,T2还可以用各种指针类型或自定义类型
  **map****的插入与迭代器******
- map.insert(...);    //往容器插入元素，返回pair<iterator,bool>
- 在map中插入元素的三种方式：
  假设  map<int, string> mapStu;
- 一、通过pair的方式插入对象
  mapStu.insert(  pair<int,string>(3,"小张")  );
- 二、通过pair的方式插入对象
  mapStu.inset(make_pair(-1,“校长-1”));
- 三、通过value_type的方式插入对象
  mapStu.insert(  map<int,string>::value_type(1,"小李")  );
- 四、通过数组的方式插入值
  mapStu[3] = “小刘";
  mapStu[5] = “小王"；
  ​         
- 前三种方法，采用的是insert()方法，该方法**返回值为****pair<iterator,bool> **
- 第四种方法非常直观，但存在一个性能的问题。插入3时，先在mapStu中查找主键为3的项，若没发现，则将一个键为3，值为初始化值的对组插入到mapStu中，然后再将值**修改**成“小刘”。若发现已存在3这个键，则修改这个键对应的value。
- string strName = mapStu[2];  //取操作或插入操作
- 只有当mapStu存在2这个键时才是正确的取操作，否则会自动插入一个实例，键为2，值为初始化值。
  假设  map<int, string> mapA;
  pair< map<int,string>::iterator,bool > pairResult = mapA.insert(pair<int,string>(3,"小张"));                         //插入方式一
  int iFirstFirst =(pairResult.first)->first;              //iFirst== 3;
  string strFirstSecond =(pairResult.first)->second;            //strFirstSecond为"小张"
  bool bSecond = pairResult.second;                                                         //bSecond== true;
  ​                   
  mapA.insert(map<int,string>::value_type(1,"小李"));                      //插入方式二
  mapA[3] = "小刘";                     //修改value
  mapA[5] = "小王";                     //插入方式三
  string str1 = mapA[2];                         //执行插入 string()操作，返回的str1的字符串内容为空。
  string str2 = mapA[3];                         //取得value，str2为"小刘"
  //迭代器遍历
         for(map<int,string>::iterator it=mapA.begin(); it!=mapA.end(); ++it)
         {
                   pair<int,string> pr = *it;
                   intiKey = pr.first;
                   stringstrValue = pr.second;
         }
  map.rbegin()与map.rend()  略。
- map<T1,T2,less<T1> > mapA;  //该容器是按键的升序方式排列元素。未指定函数对象，默认采用less<T1>函数对象。
- map<T1,T2,greater<T1>> mapB;   //该容器是按键的降序方式排列元素。
- less<T1>与greater<T1>  可以替换成其它的函数对象functor。
- 可编写自定义函数对象以进行自定义类型的比较，使用方法与set构造时所用的函数对象一样。
- map.begin();  //返回容器中第一个数据的迭代器。
- map.end();  //返回容器中最后一个数据之后的迭代器。
- map.rbegin();  //返回容器中倒数第一个元素的迭代器。
- map.rend();   //返回容器中倒数最后一个元素的后面的迭代器。
#### map对象的拷贝构造与赋值
map(const map &mp);                 //拷贝构造函数
map& operator=(const map &mp);         //重载等号操作符
map.swap(mp);                                    //交换两个集合容器
例如:
map<int,string> mapA;
mapA.insert(pair<int,string>(3,"小张"));         
mapA.insert(pair<int,string>(1,"小杨"));         
mapA.insert(pair<int,string>(7,"小赵"));         
mapA.insert(pair<int,string>(5,"小王"));         
map<int,string> mapB(mapA);                          //拷贝构造
​                   
map<int,string> mapC;
mapC= mapA;                                                                  //赋值
mapC[3]= "老张";
mapC.swap(mapA);                   //交换
#### map的大小
- map.size(); //返回容器中元素的数目
- map.empty();//判断容器是否为空
  map<int,string> mapA;
  mapA.insert(pair<int,string>(3,"小张"));         
  mapA.insert(pair<int,string>(1,"小杨"));         
  mapA.insert(pair<int,string>(7,"小赵"));         
  mapA.insert(pair<int,string>(5,"小王"));         
                   if(mapA.empty())
                   {
                            intiSize = mapA.size();              //iSize== 4
                   }
#### map的删除
- map.clear();                 //删除所有元素
- map.erase(pos); //删除pos迭代器所指的元素，返回下一个元素的迭代器。
- map.erase(beg,end);     //删除区间[beg,end)的所有元素  ，返回下一个元素的迭代器。
- map.erase(keyElem);     //删除容器中key为keyElem的对组。
  map<int, string> mapA;
  mapA.insert(pair<int,string>(3,"小张"));         
  mapA.insert(pair<int,string>(1,"小杨"));         
  mapA.insert(pair<int,string>(7,"小赵"));         
  mapA.insert(pair<int,string>(5,"小王"));         
  //删除区间内的元素
  map<int,string>::iteratoritBegin=mapA.begin();
  ++itBegin;
  ++itBegin;
  map<int,string>::iteratoritEnd=mapA.end();
  mapA.erase(itBegin,itEnd);                        //此时容器mapA包含按顺序的{1,"小杨"}{3,"小张"}两个元素。
  mapA.insert(pair<int,string>(7,"小赵"));         
  mapA.insert(pair<int,string>(5,"小王"));         
  //删除容器中第一个元素
  mapA.erase(mapA.begin());             //此时容器mapA包含了按顺序的{3,"小张"}{5,"小王"}{7,"小赵"}三个元素
  //删除容器中key为5的元素
  mapA.erase(5);    
  //删除mapA的所有元素
  mapA.clear();                     //容器为空
#### map的查找
- map.find(key);   查找键key是否存在，若存在，返回该键的元素的迭代器；若不存在，返回map.end();
- map.count(keyElem);   //返回容器中key为keyElem的对组个数。对map来说，要么是0，要么是1。对multimap来说，值可能大于1。
### 容器共性机制研究
#### 容器的共通能力
C++模板是容器的概念。
**理论提高：**所有容器提供的都是值（value）语意，而非引用（reference）语意。**容器执行插入元素的操作时，内部实施拷贝动作。**所以STL容器内存储的元素必须**能够被拷贝**（必须提供拷贝构造函数）。
- 除了queue与stack外，每个容器都提供可返回迭代器的函数，运用返回的迭代器就可以访问元素。
- 通常STL不会丢出异常。要求使用者确保传入正确的参数。
- 每个容器都提供了一个默认构造函数跟一个默认拷贝构造函数。
- 如已有容器vecIntA。 
- vector<int> vecIntB(vecIntA); //调用拷贝构造函数，复制vecIntA到vecIntB中。
- 与大小相关的操作方法(c代表容器)：
  c.size();   //返回容器中元素的个数
  c.empty();   //判断容器是否为空
- 比较操作(c1,c2代表容器)：
  c1 == c2     判断c1是否等于c2
  c1 != c2      判断c1是否不等于c2
  c1 = c2        把c2的所有元素指派给c1
####  10.2.9.2各个容器的使用时机
- Vector的使用场景：比如软件历史操作记录的存储，我们经常要**查看历史记录**，比如上一次的记录，上上次的记录，但却不会去删除记录，因为记录是事实的描述。
- deque的使用场景：比如排队购票系统，对排队者的存储可以采用deque，支持头端的快速移除，尾端的快速添加。如果采用vector，则头端移除时，会移动大量的数据，速度慢。
- vector与deque的比较：
- 一：vector.at()比deque.at()效率高，比如vector.at(0)是固定的，deque的开始位置却是不固定的。
- 二：如果有大量释放操作的话，vector花的时间更少，这跟二者的内部实现有关。
- 三：deque支持头部的快速插入与快速移除，这是deque的优点。
- list的使用场景：比如公交车乘客的存储，随时可能有乘客下车，支持频繁的不确实位置元素的移除插入。
- set的使用场景：比如对手机游戏的个人得分记录的存储，存储要求从高分到低分的顺序排列。 
- map的使用场景：比如按ID号存储十万个用户，想要快速要通过ID查找对应的用户。二叉树的查找效率，这时就体现出来了。如果是vector容器，最坏的情况下可能要遍历完整个容器才能找到该用户。