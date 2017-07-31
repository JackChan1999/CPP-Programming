---
typora-copy-images-to: images
---

## 多元数组 tuple

![1500136771914](images/1500136771914.png)

```C++
#include<iostream>  
#include<map>  
//多元数组  
//tuple必须是一个静态数组
//配合vector, array  
  
void main()
{  
    int int1 = 10;  
    double double1 = 99.8;  
    char ch = 'A';  
    char *str = "hellochina";  
    std::tuple<int, double, char, const char *> mytuple(int1, double1, ch, str);  
    const int num = 3;  
    auto data0 = std::get<0>(mytuple);  
    auto data1 = std::get<1>(mytuple);  
    auto data2 = std::get<2>(mytuple);  
    auto data3 = std::get<num>(mytuple);//下标只能是常量  
    std::cout <<typeid( data3).name()  << std::endl;  
    decltype(data0) dataA;//获取数据类型再次创建  
    //mytuple.swap(mytuple);array.vetor都有交换的公能  
    std::cout << data0 <<"  " << data1 <<"  "<< data2 << "   " <<data3 << std::endl;  
  
    std::cin.get();  
}  
```

## bitset

## auto自动变量

C++11中引入的auto主要有两种用途：自动类型推断和返回值占位。auto在C++98中的标识临时变量的语义，由于使用极少且多余，在C++11中已被删除。前后两个标准的auto，完全是两个概念。

### 自动类型推断

auto自动类型推断，用于从初始化表达式中推断出变量的数据类型。通过auto的自动类型推断，可以大大简化我们的编程工作。下面是一些使用auto的例子。

auto自动变量自动根据类型创建数据

```C++
#include <iostream>  
#include<stdlib.h>  
  
void main()  
{  
    double db = 10.9;  
    double *pdb = &db;  
    auto num = pdb;//通用传入接口  
    std::cout << typeid(db).name() << std::endl;  
    std::cout << typeid(num).name() << std::endl;  
    std::cout << typeid(pdb).name() << std::endl;  
    //typeid(db).name()  db2;  
    decltype(db) numA(10.9);//通用的备份接口  
    std::cout << sizeof(numA) <<"    "<< numA << std::endl;  
  
    system("pause");  
}  
```
### auto与函数模板

```C++
//自动数据类型，根据实际推导出类型，  
template<class T1,class T2>//根据类型获取类型  
auto get(T1 data, T2 bigdata)->decltype(data *bigdata)  
{  
    return  data*bigdata;  
}  

void main()  
{  
    std::cout << typeid(get(12.0, 'A')).name() << std::endl;  
    std::cout << get(12.0, 'A') << std::endl;  
    std::cout << typeid(get(12, 'A')).name() << std::endl;  
    std::cout << get(12, 'A') << std::endl;  
  
    system("pause");  
} 
```
## 智能指针

```C++
#include <iostream>  
  
void main1()  
{  
    //auto_ptr;  
    for (int i = 0; i < 10000000; i++)  
    {  
        double *p = new double;//为指针分配内存  
        std::auto_ptr<double> autop(p);  
        //创建智能指针管理指针p指向内存  
        //智能指针
        //delete p;  
    }  
  
    std::cin.get();  
}
```

### C++11智能指针

```C++
#include<iostream>  
#include<memory>//内存  
  
void main()  
{  
    for (int i = 0; i < 10000000;i++)  
    {  
        //新型指针，新型的数组          
        std::unique_ptr<double> pdb(new double);  
        //double *p = new double;  
    }  
  
    std::cin.get();  
}
```