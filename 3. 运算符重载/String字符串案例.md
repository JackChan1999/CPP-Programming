## String字符串案例

### MyString.h

```C++
#include <iostream>
using namespace std;

//c中没有字符串 字符串类(c风格的字符串)
//空串 ""
class  MyString
{
	friend ostream& operator<<(ostream &out, MyString &s);
	friend istream& operator>>(istream &in, MyString &s);
public:
	MyString(int len = 0);
	MyString(const char *p);
	MyString(const MyString& s);
	~MyString();

public: //重载=号操作符
	MyString& operator=(const char *p);
	MyString& operator=(const MyString &s);
	char& operator[] (int index);

public: //重载 == !== 
	bool operator==(const char *p) const;
	bool operator==(const MyString& s) const;
	bool operator!=(const char *p) const;
	bool operator!=(const MyString& s) const;

public:
	int operator<(const char *p);
	int operator>(const char *p);
	int operator<(const MyString& s);
	int operator>(const MyString& s);

	//把类的指针 露出来
public:
	char *c_str()
	{
		return m_p;
	}
	const char *c_str2()
	{
		return m_p;
	}
	int length()
	{
		return m_len;
	}
private:
	int		m_len;
	char	 *m_p;
};
```

### MyString.cpp

```C++
#define _CRT_SECURE_NO_WARNINGS
#include "MyString.h"

ostream& operator<<(ostream &out, MyString &s)
{
	out<<s.m_p;
	return out;
}
istream& operator>>(istream &in, MyString &s)
{
	cin>>s.m_p;
	return in;
}

MyString::MyString(int len)
{
	if (len == 0)
	{
		m_len = 0;
		m_p = new char[m_len + 1];
		strcpy(m_p, "");
	}
	else
	{
		m_len = len;
		m_p = new char[m_len + 1];
		memset(m_p, 0, m_len);
	}

}

MyString::MyString(const char *p)
{
	if (p == NULL)
	{
		m_len = 0;
		m_p = new char[m_len + 1];
		strcpy(m_p, "");
	}
	else
	{
		m_len = strlen(p);
		m_p = new char[m_len + 1];
		strcpy(m_p, p);
	}
}

//拷贝构造函数
//MyString s3 = s2;
MyString::MyString(const MyString& s)
{
	m_len = s.m_len;
	m_p = new char[m_len + 1];

	strcpy(m_p, s.m_p);
}

MyString::~MyString()
{
	if (m_p != NULL)
	{
		delete [] m_p;
		m_p = NULL;
		m_len = 0;
	}
}

// s4 = "s2222";
MyString& MyString::operator=(const char *p)
{
	//1 旧内存释放掉
	if (m_p != NULL)
	{
		delete [] m_p;
		m_len = 0;
	}
	//2 根据p分配内存
	
	if (p == NULL)
	{
		m_len = 0;
		m_p = new char[m_len + 1];
		strcpy(m_p, "");
	}
	else
	{
		m_len = strlen(p);
		m_p = new char[m_len + 1];
		strcpy(m_p, p);
	}
	return *this;
}

// s4 = s2;
MyString& MyString::operator=(const MyString &s)
{
	//1 旧内存释放掉
	if (m_p != NULL)
	{
		delete [] m_p;
		m_len = 0;
	}
	//2 根据s分配内存
	m_len = s.m_len;
	m_p = new char[m_len + 1];
	strcpy(m_p, s.m_p);

	return *this;
}

char& MyString::operator[] (int index)
{
	return m_p[index];
}

//if (s2 == "s222222")
bool MyString::operator==(const char *p) const
{
	if (p == NULL)
	{
		if (m_len == 0)
		{
			return true;
		}
		else
		{
			return false;
		}
	}
	else
	{
		if (m_len == strlen(p))
		{
			return !strcmp(m_p, p);
		}
		else
		{
			return false;
		}
	}
}

bool MyString::operator!=(const char *p) const
{
	return !(*this == p);
}


bool MyString::operator==(const MyString& s)  const
{
	if (m_len != s.m_len)
	{
		return false;
	}
	return !strcmp(m_p, s.m_p);
}

bool MyString::operator!=(const MyString& s) const
{
	return !(*this == s);
}

//if (s3 < "bbbb")
int MyString::operator<(const char *p)
{
	return strcmp(this->m_p , p);
}

int MyString::operator>(const char *p)
{
	return strcmp(p, this->m_p);
}

int MyString::operator<(const MyString& s)
{
	return strcmp(this->m_p , s.m_p);
}

int MyString::operator>(const MyString& s)
{
	return  strcmp(s.m_p, m_p);
}
```

### MyString_Test.cpp

```C++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include "MyString.h"
using namespace std;

void main01()
{
	MyString s1;
	MyString s2("s2");
	MyString s2_2 = NULL;
	MyString s3 = s2;
	MyString s4 = "s4444444444";

	//测试运算符重载 和 重载[]
	//=

	s4 = s2;
	s4 = "s2222";
	s4[1] = '4';
	printf("%c", s4[1]);

	cout<<s4 <<endl;
	//ostream& operator<<(ostream &out, MyString &s)
	
	//char& operator[] (int index)
	//MyString& operator=(const char *p);
	//MyString& operator=(const MyString &s);

	cout<<"hello..."<<endl;
	system("pause");
	return ;
}

void main02()
{
	MyString s1;
	MyString s2("s2");

	MyString s3 = s2;

	if (s2 == "aa")
	{
		printf("相等");
	}
	else
	{
		printf("不相等");
	}

	if (s3 == s2)
	{
		printf("相等");
	}
	else
	{
		printf("不相等");
	}
	
}
void main03()
{
	MyString s1;
	MyString s2("s2");

	MyString s3 = s2;
	s3 = "aaa";

	int tag = (s3 < "bbbb");
	if (tag < 0 )
	{
		printf("s3 小于 bbbb");
	}
	else
	{
		printf("s3 大于 bbbb");
	}

	MyString s4 = "aaaaffff";
	strcpy(s4.c_str(), "aa111"); //MFC
	cout<<s4<<endl;
}

void main011()
{
	MyString s1(128);
	cout<<"\n请输入字符串(回车结束)";
	cin>>s1;

	cout<<s1;
	system("pause");
	
}

void main()
{
	MyString s1(128);
	cout<<"\n请输入字符串(回车结束)";
	cin>>s1;

	cout<<s1<<endl;
	
	system("pause");
}
```