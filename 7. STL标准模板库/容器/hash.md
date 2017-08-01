## 哈希存储

### Hashnode.h

```c++
#pragma once  
template<class T>

class hashnode  
{  
public:   
    int key;//索引  
    T  t;  //代表数据  
    int cn;//代表查找次数  
};  
```

### Hash.h

```c++
#pragma once  
#include "hashnode.h"  
  
template<class T>  
class Hash  
{  
public:  
    hashnode<T> *p;//p->存储哈希表  
    int n;//长度  
public:  
     int   myhash(int key );  
     void init(T * pt, int nt);  
  
     bool  isin(int key,T t);  
     hashnode<T> *  find(int key);  
    Hash();  
    ~Hash();  
};  
```

### Hash.cpp

```C++
#include "Hash.h"  
  
template<class T>  
Hash<T>::Hash()  
{  
    this->n = 10;  
    this->p = new hashnode<T>[this->n];  
}  
  
template<class T>  
Hash<T>::~Hash()  
{  
    delete[] p;  
}  
  
template<class T>  
int  Hash<T>::myhash(int key)  
{  
    return key % n;  
}  
  
template<class T>  
void Hash<T>::init(T *pt,int  nt)  
{  
    for (int i = 0; i < 10;i++)  
    {  
        p[i].key = i;  
        p[i].t = pt[i];  
        p[i].cn = 0;  
    }  
}  
  
template<class T>  
bool  Hash<T>::isin(int key,T t)  
{  
    int res = myhash(key);  
    if (p[res].t==t)  
    {  
        return true;  
    }  
    else  
    {  
        return false;  
    }  
}  
  
template<class T>  
hashnode<T> *   Hash<T>::find(int key)  
{  
    int res = myhash(key);  
    return  p + res;  
}  
```

### main.cpp

```c++
#include<iostream>  
#include "Hash.h"  
#include "Hash.cpp"  
#include "hashnode.h"  
using namespace std;  
  
void main()  
{     
    Hash<int> myhash;  
    int a[10] = { 10, 11, 22, 33, 44, 55, 56, 67, 78, 99 };  
    myhash.init(a, 10);  
    cout << myhash.isin(43,43) << endl;  
    hashnode<int > *p = myhash.find(8);  
    cout << p->key << endl;  
    cout << p->t << endl;  
  
    cin.get();  
}  
```