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

使用auto说明符还有一个重要的用途是：在无法确定某对象的类型时，使用auto可以根据情况完成变量定义：

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
```c++
template <class T1, class T2>
void multiply(T1 x, T2 y)
{
	auto result = x * y;
}
```

本例中定义了一个模板函数，运算结果被记录在auot说明符修饰的result变量中，则会根据模板实例化的情况确定变量类型。