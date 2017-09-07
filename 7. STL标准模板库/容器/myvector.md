## 线性表顺序存储

### myvector.h

```c++
#pragma once  
  
template <class T>  
class myvector  
{  
public:  
    myvector();  
    ~myvector();  
  
    void push_back(T t);  
    //增删查改  
    T *find(T t);  
    void change(T*pos, T t);  
    void del(T t);  
    void show();  
    void insert(T findt, T t);
  	T operator [](int i);  
public:  
    T *p;  
    int n;//标记内存长度  
    int realn;//实际长度  
};  
```

### myvector.cpp

```c++
#include "myvector.h"  
  
template <class T>  
myvector<T>::myvector()  
{  
    p = nullptr;  
    n = realn = 0;  
}  
  
template <class T>  
myvector<T>::~myvector()  
{  
    if (p!=nullptr)  
    {  
        delete  []p;  
        p = nullptr;//清空  
    }  
}  
  
template <class T>  
void myvector<T>::push_back(T t)  
{  
    if (p==nullptr)  
    {  
        p = new T;//分配内存  
        *p = t;//赋值  
        realn = n = 1;  
    }   
    else  
    {  
        T *ptemp = new T[n + 1];//重新分配内存  
        for (int i = 0; i < n;i++)  
        {  
            *(ptemp + i) = *(p + i);//拷贝  
        }  
        *(ptemp + n) = t;//赋值最后一个元素  
        delete []p;  
  
        p = ptemp;  
  
        realn += 1;  
        n += 1;  
    }  
}  
  
template <class T>  
void myvector<T>::show()  
{  
    if (p==NULL)  
    {  
        return;  
    }  
    else  
    {  
       for (int i = 0; i < realn;i++)  
       {  
           cout << p[i] << "  ";  
       }  
       cout << "\n";  
    }  
}  
  
template <class T>  
T *  myvector<T>::find(T t)  
{  
    for (int i = 0; i < realn; i++)  
    {  
        if (t==*(p+i))  
        {  
            return p + i;  
        }  
    }  
    return nullptr;  
}  
  
template <class T>  
void myvector<T>::change(T*pos, T t)  
{  
    if (pos==nullptr)  
    {  
        return;  
    }  
    else  
    {  
        *pos = t;  
    }  
}  
  
template <class T>  
void myvector<T>::del(T t)  
{  
    int pos = -1;  
    for (int i = 0; i < realn; i++)  
    {  
        if (t == *(p + i))  
        {  
            pos = i;  
            break;  
        }  
    }  
    if (pos!=-1)  
    {  
        if (pos== realn-1)  
        {  
            realn -= 1;  
        }  
        else   
        {  
            for (int i = pos; i < realn-1;i++)  
            {  
                p[i] = p[i + 1];  
            }  
            realn -= 1;  
        }         
    }  
}  
  
template <class T>  
void myvector<T>::insert(T findt, T t)  
{  
    if (n == realn)  
    {  
        {  
           int pos = -1;  
           for (int i = 0; i < realn; i++)  
           {  
              if (findt== *(p + i))  
               {  
                pos = i;  
                break;  
                }  
           }  
           if (pos!=-1)  
           {  
               //重新分配内存并拷贝  
               {  
                   T *ptemp = new T[n + 1];//重新分配内存  
  
                   for (int i = 0; i < n; i++)  
                   {  
                       *(ptemp + i) = *(p + i);//拷贝  
                   }  
                   delete[] p;  
                   p = ptemp;  
                   realn += 1;  
                   n += 1;  
               }  
  
               for (int  i = realn - 2; i>=pos;i--)  
               {  
                    p[i + 1]=p[i];//往前移动  
  
               }  
               p[pos] = t;  
           }  
        }     
    }  
    else  
    {  
        int pos = -1;  
        for (int i = 0; i < realn; i++)  
        {  
            if (findt == *(p + i))  
            {  
                pos = i;  
                break;  
            }  
        }  
        if (pos != -1)  
        {  
            for (int i = realn - 1; i >= pos; i--)  
            {  
                p[i + 1] = p[i];//往前移动  
  
            }  
            p[pos] = t;  
            realn += 1;  
        }  
    }  
}  
  
template <class T>  
T myvector<T>::operator [](int i)  
{  
    if (i < 0 || i>=realn)  
    {         
        return NULL;  
    }  
    return  p[i];  
}  
```

### main.cpp

```c++
#include <iostream>  
#include<stdlib.h>  
#include <vector>  
#include <string>  
#include "myvector.h"  
#include "myvector.cpp" 
  
using namespace std;  
  
void main()  
{  
    myvector<int> myv1;  
    myv1.push_back(11);  
    myv1.push_back(12);  
    myv1.push_back(13);  
    myv1.push_back(14);  
    myv1.push_back(15);  
    myvector<int> myv2;  
    myv2.push_back(31);  
    myv2.push_back(32);  
    myv2.push_back(33);  
  
    myvector<int> myv3;  
    myv3.push_back(131);  
    myv3.push_back(132);  
    myv3.push_back(133);  
    myv3.push_back(1337);  
  
    myvector< myvector<int>* > myvv;//自己写的模板嵌套用指针  
  
    myvv.push_back(&myv1);  
    myvv.push_back(&myv2);  
    myvv.push_back(&myv3);  
    myvv[0]->show();  
    myvv[1]->show();  
    myvv[2]->show();  
  
    cin.get();  
}  
  
void main1()  
{  
    myvector<int> myv;  
    myv.push_back(11);  
    myv.push_back(12);  
    myv.push_back(13);  
    myv.push_back(14);  
    myv.push_back(15);  
    myv.show();  
  
    cin.get();  
}  
  
void main2()  
{  
    myvector<double> myv;  
    myv.push_back(11.2);  
    myv.push_back(12.0);  
    myv.push_back(13.5);  
    myv.push_back(14.9);  
    myv.push_back(15.90);  
    myv.show();  
  
    cin.get();  
}  
  
void main3()  
{  
    myvector<char *> myv;  
    myv.push_back("av");  
    myv.push_back("va");  
    myv.push_back("cc");  
    myv.push_back("tv");  
  
    myv.show();  
  
    cin.get();  
}  
  
void main4()  
{  
    vector<string> myv;
    myv.push_back("av");  
    myv.push_back("va");  
    myv.push_back("cc");  
    myv.push_back("tv");  
      
    cin.get();  
}  
  
void main5()  
{  
    myvector<int> myv;  
    myv.push_back(11);  
    myv.push_back(12);  
    myv.push_back(13);  
    myv.push_back(14);  
    myv.push_back(15);  
    myv.show();  
    int *p = myv.find(13);  
    cout << p << endl;  
    myv.change(p, 23);
    myv.show();  
    myv.del(12);  
    myv.insert(23, 99);  
  
    myv.show();  
  
    cout << myv[2] << endl;  
    cin.get();  
}  
```