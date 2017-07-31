## 函数模板与auto自动变量

### cstdarg

| 函数声明       | 功能描述                                     |
| ---------- | ---------------------------------------- |
| va_list    | 参数列表的指针                                  |
| va_start() | 使 va_list 指向起始的参数                        |
| va_arg()   | 检索参数,每次提取一次可变参数,arg 就向上移动一次.无论它现在指向的是不是参数它都会读取arg参数里存放地址里的内容 |
| va_end()   | 释放 va_list                               |

### 函数模板

```C++
#include<stdlib.h>  
#include<iostream>  
#include<cstdarg>

//函数模板  可变参数  
//参数至少要有一个是模板类型  
template<typename NT>  
NT sum(int count,NT data1 ...)//累加  
{  
    va_list arg_ptr;//参数列表的指针  
    va_start(arg_ptr, count);//限定从count开始,限定多少个参数  
    NT sumres(0);  
    for (int i = 0; i < count;i++)  
    {  
        sumres += va_arg(arg_ptr, NT);  
    }  
    va_end(arg_ptr);//结束  
    return sumres;  
}  
  
//T通用数据类型  
template<typename T>  
T MAX(T*p, const int n)  
{  
    T maxdata(p[0]);  
    for (int i = 1; i < n; i++)    
    {  
        if (maxdata < p[i])  
        {  
            maxdata = p[i];  
        }  
    }  
    return maxdata;  
}  
  
int getmax(int *p, int n)  
{  
    int max(0);  
    max = p[0];//假定第一个数位最大  
    for (int i = 1; i < 10; i++)  
    {  
        if (max<p[i])//确保max>=p[i]  
        {  
            max = p[i];//  
        }  
    }  
    return max;  
}  
  
double getmax(double *p, int n)  
{  
    double max(0);  
    max = p[0];//假定第一个数位最大  
    for (int i = 1; i < 10; i++)  
    {  
        if (max < p[i])//确保max>=p[i]  
        {  
            max = p[i];
        }  
    }  
    return max;   
}  

void main2()  
{  
    std::cout << sum(5,1,2,3,4,5) << std::endl;  
    std::cout << sum(6, 1, 2, 3, 4, 5,6) << std::endl;  
    std::cout << sum(7, 1, 2, 3, 4, 5, 6,7) << std::endl;  
    std::cout << sum(7, 1.1, 2.1, 3.1, 4.1, 5.1, 6.1, 7.1) << std::endl;  
    std::cout << sum(6, 1.1, 2.1, 3.1, 4.1, 5.1, 6.1) << std::endl;  
    system("pause");  
}  
  
void main1()  
{  
    double a[10] = {  2, 3, 4, 98, 77, 999.1, 87, 123, 0, 12 };  
    int  b[10] = { 1, 2, 3, 4, 15, 6, 7, 8, 9, 10 };  
    std::cout << MAX(a,10) << std::endl;  
    std::cout << MAX(b, 10) << std::endl;  
  
    system("pause");  
}  
```

### auto与函数模板

```C++
#include<stdlib.h>  
#include<iostream>  
/* 
auto get(int num, double data)->decltype(num*data) 
{ 
 
} 
*/  
//自动数据类型，根据实际推导出类型，  
template<class T1,class T2>//根据类型获取类型  
auto get(T1 data, T2 bigdata)->decltype(data *bigdata)  
{  
    return  data*bigdata;  
}  
  
//函数参数不允许使用自动变量  
//int putnum(auto num)  
//{  
//  
//}  
  
void main()  
{  
    std::cout << typeid(get(12.0, 'A')).name() << std::endl;  
    std::cout << get(12.0, 'A') << std::endl;  
    std::cout << typeid(get(12, 'A')).name() << std::endl;  
    std::cout << get(12, 'A') << std::endl;  
  
    system("pause");  
}  
```