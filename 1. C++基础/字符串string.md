## 用string定义字符串

```C++
#include<string>

string s1;
s1 = "hello world";
string s2 = "hello C++";
string s3("are you ok");
string s4(6,'a'); // s4 = "aaaaaa"
```

## 去空格

```C++
void  trim(char *str)
{
	int i = 0;
	int j = 0;
	while (( *(str+i) = *(str+j++))!='\0')
	{
		if (*(str + i)!=' ')
		{
			i++;
		}
	}
	//两个下标轮替，往前移动，链表的算法一样，循环向前挖
}
```

## string常用函数

| 函数声明            | 功能描述            |
| --------------- | --------------- |
| assign()        | 给字符串赋值          |
| size()/length() | 获取字符串的字符数量      |
| capacity()      | 获取字符串容量         |
| empty()         | 判断一个字符串是否为空     |
| resize()        | 重新分配字符串的内存大小    |
| begin()         | 获取字符串开头位置       |
| end()           | 获取字符串最后字符的下一个位置 |
| rbegin()        | 获取字符串最后字符的位置    |
| rend()          | 获取字符串开头字符的上一个位置 |
| insert()        | 插入字符或字符串        |
| erase()         | 删除字符串中的字符或字符串   |
| clear()         | 删除全部字符          |
| replace()       | 替换字符串的字符        |
| find()          | 查找              |
| substr()        | 返回某个子串          |
| at()            | 访问某个角标上的字符      |
| []              | 与at()功能相同       |
| +               | 连接两个字符串         |
| ==、!=、<、<=、>、>= | 比较字符串           |
| compare()       | 比较字符串           |
| append()        | 追加字符或字符串        |
| push_back()     | 追加字符或字符串        |
| swap()          | 交换两个字符串的值       |

## 转义字符 R”()”

```C++
#include <iostream>  
#include<string>  
#include<stdlib.h>  

void main()  
{  
    std::string path =R"( "C:\Program Files\Tencent\QQ\QQProtect\Bin\QQProtect.exe")";  
    //R"()"   括号之间去掉转义字符  
    system(path.c_str());  
    system("pause");  
}
```