## 线性表链式存储

### Node.h

```c++
#pragma once  
template <class T>  
class Node  
{  
public:  
    T t; //数据域
    Node *pNext; //指针域   
}; 
```

### List.h

```c++
#pragma once  
#include "Node.h"  
#include <iostream>  
  
template <class T>  
class List  
{  
public:  
    Node<T> *pHead;  
  
public:  
    List();  
  	~List(); 
  
    void add(T t);//尾部插入  
    void show();//显示  
    Node<T> * find(T t);//查找  
    void change(Node<T> *p, T newt);//修改  
    int getnum();//获取个数  
    bool deletet(T t);  
    void sort();  
    void deletesame();//删除相同的元素  
    bool clear();  
    void rev();
    void insert(T oldt, T newt);  
    void merge(List & list);
};  
```

### List.cpp

```c++
#include "List.h"  
  
template <class T>  
List<T>::List()  
{  
    this->pHead = nullptr;//设置空节点   
    cout << "链表创建" << endl;  
}  
  
template <class T>  
List<T>::~List()  
{  
    cout << "链表销毁" << endl;  
}  
  
template <class T>  
void List<T>::add(T t)  
{  
    Node<T> *pnew = new Node<T>;//分配节点  
    pnew->t = t;//赋值  
    pnew->pNext = nullptr;  
  
    if (pHead==nullptr)  
    {         
        this->pHead = pnew;//头结点  
    }   
    else  
    {  
        Node<T> *p = pHead;//获取头结点位置  
        while (p->pNext!=nullptr)//循环到尾部  
        {  
            p = p->pNext;  
        }  
        p->pNext = pnew;  
    }  
}  
  
template <class T>  
void List<T>::show()  
{  
    Node<T> *p = pHead;//获取头结点位置  
    while (p!= nullptr)//循环到尾部  
    {  
        cout << p->t << "  ";  
        p = p->pNext;  
    }  
    cout << endl;  
}  
  
template <class T>  
Node<T> * List<T>::find(T t)  
{  
    Node<T> *p = pHead;//获取头结点位置  
    while (p != nullptr)//循环到尾部  
    {  
        if (p->t==t)  
        {  
            return p;  
        }     
        p = p->pNext;  
    }  
    return nullptr;  
}  
  
template <class T>  
void List<T>::change(Node<T> *p, T newt)  
{  
    if (p==nullptr)  
    {  
        return;  
    }  
    p->t = newt;  
}  
  
template <class T>  
int  List<T>::getnum()  
{  
    int i = 0;  
    Node<T> *p = pHead;//获取头结点位置  
    while (p != nullptr)//循环到尾部  
    {  
        i++;  
        p = p->pNext;  
    }  
    return i;  
}  
  
template <class T>  
bool List<T>::deletet(T t)  
{  
    Node<T> *p = this->find(t);  
    if (p==nullptr)  
    {  
        return  false;  
    }  
    else  
    {         
        if (p==pHead)//头结点  
        {  
            pHead = p->pNext;  
            delete p;  
        }  
        else  
        {  
            Node<T> *p1, *p2;  
            p1 = pHead;  
            p2 = p1->pNext;  
            while (p2!=p)//删除一个节点，获取前一个节点  
            {  
                p1 = p2;  
                p2 = p2->pNext;                
            }  
  
            p1->pNext = p2->pNext;//跳过p2  
            delete p2;  
        }         
        return true;  
    }  
}  
  
  
template <class T>  
void List<T>::sort()  
{  
    for (Node<T> *p1 = pHead; p1 != NULL;p1=p1->pNext)  
    {  
        for (Node<T> *p2 = pHead; p2!= NULL; p2 = p2->pNext)  
        {  
            if (p1->t < p2->t)  
            {  
                T temp;  
                temp = p1->t;  
                p1->t = p2->t;  
                p2 -> t = temp;  
            }  
        }  
    }  
}  
  
  
template<class T>  
void List<T>::deletesame()//重新生成  
{  
        Node<T> *p1, *p2;  
        p1 = pHead->pNext;  
        p2 = pHead;  
        while (p1!=nullptr)  
        {  
            if (p1->t==p2->t)  
            {  
                //cout << "=";  
                p2->pNext = p1->pNext;  
                delete p1;  
                p1 = p2->pNext;  
            }  
            else  
            {  
                p2 =p1;  
                p1 = p1->pNext;  
            }  
        }  
}  
  
template<class T>  
bool List<T>::clear()  
{  
    if (pHead ==nullptr)  
    {  
        return false;  
    }  
  
    Node<T> *p1, *p2;  
    p1 = pHead->pNext;  
    while (p1!=nullptr)  
    {  
        p2 = p1->pNext;  
        delete p1;  
        p1 = p2;  
    }  
    delete pHead;  
    pHead = nullptr;  
    return true;  
}  
  
template<class T>  
//递归  
void  List<T>::rev()  
{  
    if (pHead==nullptr || pHead->pNext==nullptr)  
    {  
        return;  
    }   
    else  
    {  
        Node<T> *p1, *p2, *p3;  
        p1 = pHead;  
        p2 = p1->pNext;  
  
        while (p2!=nullptr)  
        {  
            p3 = p2->pNext;  
            p2->pNext = p1;  
            p1 = p2;  
            p2 = p3;  
        }  
        pHead->pNext= nullptr;  
        pHead = p1;  
    }  
}  
  
template<class T>  
void  List<T>::insert(T oldt, T newt)  
{  
    Node<T> *p = find(oldt);  
    if (p!=nullptr)  
    {  
        Node<T> *p1, *p2;  
        p1 = pHead;  
        p2 = p1->pNext;  
        while (p2 != p)  
        {  
            p1 = p2;  
            p2 = p2->pNext;  
        }  
        Node<T> *pnew = new Node<T>;  
        pnew->t = newt;  
        pnew->pNext = p2;  
        p1->pNext = pnew;  
    }  
}  
  
template<class T>  
void  List<T>::merge(List & list)  
{  
    Node<T> *p = pHead;//获取头结点位置  
    while (p->pNext != nullptr)//循环到尾部  
    {  
        p = p->pNext;  
    }  
    p->pNext = list.pHead;
} 
```

### main.cpp

```c++
#include<iostream>  
#include<string>  
#include "List.h"  
#include "List.cpp"  
#include "Node.h"  
  
using namespace std;  
  
void main()  
{  
    List<int> * pmylist1 = new List<int>;  
    pmylist1->add(11);  
    pmylist1->add(11);  
    pmylist1->add(12);  
    pmylist1->add(12);  
      
    List<int> * pmylist2 = new List<int>;  
    pmylist2->add(11);  
    pmylist2->add(11);  
    pmylist2->add(12);  
  
    List< List<int> *>  *p=new List< List<int> *>;  
    p->add(pmylist1);  
    p->add(pmylist2);  
  
    p->pHead->t->show();  
  
    p->pHead->pNext->t->show();  
    p->show();  
  
    cin.get();  
}  
  
void main1()  
{  
    List<char *> cmdlist;  
    cmdlist.add("china");  
    cmdlist.add("calc");  
    cmdlist.add("born");  
    cmdlist.add("xery");  
    cmdlist.show();  
      
    cin.get();  
}  
  
void main2()  
{  
    List<int> * pmylist = new List<int>;  
    pmylist->add(11);  
    pmylist->add(11);  
    pmylist->add(12);  
    pmylist->add(12);  
    pmylist->add(12);  
    pmylist->add(12);  
    pmylist->add(15);  
    pmylist->add(11);  
    pmylist->show();  
  
  
    List<int>  list;  
    list.add(1231);  
    list.add(1232);  
    list.add(1239);  
    list.add(1237);  
    list.show();  
    pmylist->merge(list);  
    pmylist->show();  
      
    delete pmylist;  
  
    cin.get();  
}  
  
void main3()  
{  
    List<int> * pmylist = new List<int>;  
    pmylist->add(11);  
    pmylist->add(11);  
    pmylist->add(12);  
    pmylist->add(12);  
    pmylist->add(12);  
    pmylist->add(12);  
    pmylist->add(15);  
    pmylist->add(11);  
    pmylist->add(12);  
    pmylist->add(12);  
    pmylist->add(12);  
    pmylist->add(12);  
    pmylist->add(15);  
    pmylist->show();  
    pmylist->sort();  
    pmylist->show();  
    pmylist->deletesame();  
    pmylist->show();  
    pmylist->rev();  
    pmylist->show();  
    pmylist->insert(12, 1100);  
    pmylist->show();  
    pmylist->clear();  
    pmylist->show();  
  
    delete pmylist;  
  
    cin.get();  
}  
  
void main4()  
{  
    List<int> * pmylist =new List<int>;  
    pmylist->add(11);  
    pmylist->add(12);  
    pmylist->add(13);  
    pmylist->add(15);  
    pmylist->show();  
    Node<int > *p = pmylist->find(13);  
    pmylist->change(p, 19);  
    pmylist->show();  
    pmylist->deletet(15);  
    pmylist->show();  
  
    delete pmylist;  
  
    cin.get();  
}  
```