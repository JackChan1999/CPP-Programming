## Regex库

Boost C++ 的正则表达式库 Boost.Regex 可以应用正则表达式于 C++ 。 正则表达式大大减轻了搜索特定模式字符串的负担，在很多语言中都是强大的功能。

Boost.Regex 库中两个最重要的类是 boost::regex 和 boost::smatch， 它们都在 boost/regex.hpp 文件中定义。 前者用于定义一个正则表达式，而后者可以保存搜索结果。

| 函数声明            | 功能描述               |
| --------------- | ------------------ |
| expr()          | 构建正则表达式            |
| regex_match()   | 判断给定的字符串与正则表达式是否匹配 |
| regex_search()  | 分类搜索/检索            |
| regex_replace() | 字符串替换              |

```C++
#include <boost/regex.hpp>
#include <locale>
#include <iostream>

int main()
{
	std::locale::global(std::locale("German")); // 本地化
	std::string s = "Boris Schaling";
	boost::regex expr("\\w+\\s\\w+");
	std::cout << boost::regex_match(s, expr) << std::endl;
}
```

函数 boost::regex_match() 用于字符串与正则表达式的比较。 在整个字符串匹配正则表达式时其返回值为 true 。

```C++
#include <boost/regex.hpp>
#include <locale>
#include <iostream>

int main()
{
	std::locale::global(std::locale("German"));
	std::string s = "Boris Schaling";
	boost::regex expr("(\\w+)\\s(\\w+)");
	boost::smatch what;
	if (boost::regex_search(s, what, expr))
	{
		std::cout << what[0] << std::endl;
		std::cout << what[1] << “-----" << what[2] << std::endl;
	}
}
```

函数 boost::regex_search() 可以接受一个类型为 boost::smatch 的引用的参数用于储存结果。 函数 boost::regex_search() 只用于分类的搜索， 本例实际上返回了两个结果， 它们是基于正则表达式的分组。

```C++
#include <boost/regex.hpp>
#include <locale>
#include <iostream>

int main()
{
	std::locale::global(std::locale("German"));
	std::string s = " Boris Schaling ";
	boost::regex expr("\\s");
	std::string fmt("_");
	std::cout << boost::regex_replace(s, expr, fmt) << std::endl;
}
```

除了待搜索的字符串和正则表达式之外， boost::regex_replace() 函数还需要一个格式参数，它决定了子串、匹配正则表达式的分组如何被替换。如果正则表达式不包含任何分组，相关子串将被用给定的格式一个个地被替换。这样上面程序输出的结果为 `_Boris_Schaling_ `。

```C++
#include <boost/regex.hpp>
#include <locale>
#include <iostream>

int main()
{
	std::locale::global(std::locale("German"));
	std::string s = "Boris Schaling";
	boost::regex expr("(\\w+)\\s(\\w+)");
	std::string fmt("\\2 \\1");
	std::cout << boost::regex_replace(s, expr, fmt) << std::endl;
}
```

格式参数可以访问由正则表达式分组的子串，这个例子正是使用了这项技术，交换了姓、名的位置，于是结果显示为 Schaling Boris 。

```C++
#include <boost/regex.hpp>
#include <locale>
#include <iostream>

int main()
{
	std::locale::global(std::locale("German"));
	std::string s = "Boris Schaling";
	boost::regex expr("(\\w+)\\s(\\w+)");
	std::string fmt("\\2 \\1");
	std::cout << boost::regex_replace(s, expr, fmt, boost::regex_constants::format_literal) << std::endl;
}
```

此程序将 boost::regex_constants::format_literal 标志作为第四参数传递给函数 boost::regex_replace() ，从而抑制了格式参数中对特殊字符的处理。 因为整个字符串匹配正则表达式，所以本例中经格式参数替换的到达的输出结果为 \2 \1。

```C++
#include <boost/regex.hpp>
#include <locale>
#include <iostream>
#include <string>

using namespace std;

void main1()
{
	std::locale::global(std::locale("English"));
	string  str = "chinaen8Glish";
	boost::regex expr("\\w+\\d\\u\\w+");//d代表数字，
	//匹配就是1，不匹配就是0
	cout << boost::regex_match(str, expr) << endl;

	cin.get();
}

void main2()
{
	//std::locale::global(std::locale("English"));
	string  str = "chinaen8Glish9abv";
	boost::regex expr("(\\w+)\\d(\\w+)");//d代表数字，
	boost::smatch what;
	if (boost::regex_search(str,what,expr))//按照表达式检索
	{
		cout << what[0] << endl;
		cout << what[1] << endl;
	}
	else
	{
		cout << "检索失败";
	}
  
	cin.get();
}

void   main3()
{
	string  str = "chinaen8  Glish9abv";
	boost::regex expr("\\d");//d代表数字
	string  kongge = "______";
	std::cout << boost::regex_replace(str, expr, kongge) << endl;
	cin.get();
}
```

c++的正则表达式库早已有之，但始终没有哪个库纳入到标准化流程中。目前该库已经顺利的成为新一代c++标准库中的一员，结束了c++没有标准正则表达式支持的时代。

## regex指南

这部分包含了boost.regex库的正则表达式的语法。这是一份程序员指南，实际的语法由在程序中的正则表达式的选项决定。（译注：即regex类构造函数的flag参数。）

### 文字（Literals）

```
“.”, “ | ”, “*”, “ ? ”, “ + ”, “(“, “)”, “{ “, “ }”, “[“, “]”, “^”, “$” 和 “\”
```

要使用这些字符的字面意义，要在前面使用 “\” 字符。一个字面意义的字符匹配其本身，或者匹配 traits_type::translate() 的结果，这里的traits_type 是 basic_regex 类的特性模板参数(the traits template parameter)。

### 通配符（Wildcard）

点号 ”.” 匹配任意的单个字符。当在匹配算法中使用了 match_not_dot_null 选项，那么点号不匹配空字符(null character)。当在匹配算法中使用了 match_not_dot_newline 选项，那么点号不匹配换行字符（newline character）。

### 重复（Repeats）

一个重复是一个表达式（译注：正则表达式）重复任意次数。一个表达式后接一个 “*” 表示重复任意次数（包括0次）。一个表达式后接一个 “ + ” 表示重复任意次数（但是至少1次）。如果表达式使用 regex_constants::bk_plus_qm编译（译注：regex类构造函数的flag参数），那么 “ + ” 是一个普通的字符（译注：即 “ + ” 表示其字面意义）， “\ + ” 用来表示重复一或多次。一个表达式后接一个 “ ? ” 表示重复0或1次。如果表达式使用 regex_constants::bk_plus_qm 选项，那么 “ ? ” 是一个普通字符，”\ ? ” 用来表示重复0或1次。如果需要显式的指定重复的最大最小次数的话，请使用边界操作符 “{}”，那么 “a{ 2 }” 表示字母 “a” 重复2次， “a{ 2, 4 }” 表示字母 “a” 重复2至4次， “a{ 2, }” 表示字母 “a” 重复至少2次（无上限）。注意：在{}之间是没有任何空格的，并且上下边界的大小是没有上限的。如果表达式使用 regex_constants::bk_braces选项编译，那么 “{ ” 和 “ }” 是普通字符， “\{” 和 “\}” 用来表示边界操作符。所有的重复表达式是最短的前置子串（the shortest possible previous sub -expression）：单个字符，字符集合，或者是用诸如 “()” 括起来的子表达式。

例：“ba*” 匹配 “b”, “ba”, “baaa” 等等

“ba + ” 匹配诸如 “ba”, “baaaa” 此类，而不匹配 “b”

“ba ? ” 匹配 “b” 或 “ba”

“ba{ 2, 4 }” 匹配 “baa”, “baaa” 和 “baaaa”

### 非“贪心”重复（Non - greedy repeats）

无论是否启用“扩展(extended)”正则表达式语法（默认的），总是允许使用非贪心重复，只要在重复的后面加一个 “ ? ” 。非贪心重复是匹配最短可能串(the shortest possible string)的重复。

例如要匹配html的一对标签，可以使用：

```
“<\s*tagname[^>] * > (.* ? ) < \s* / tagname\s* > ”
```

### 圆括号(Parenthesis)

圆括号有两个作用：组成子表达式和标记匹配(to group items together into a sub - expression, and to mark what generated the match.)。例如，表达式 “(ab)*” 匹配所有的 “ababab”字符串。匹配算法 regex_match 和 regex_search 各需要一个match_results对象来报告是怎样匹配的，函数返回后match_results会包含整个表达式和各个子表达式的匹配。比如在上述的例子中，match_results[1]会包含表示最后一个 “ab” 的迭代器对(pair)。子表达式也允许匹配空串。如果子表达式匹配为空 - 例如子表达式为选择中的不匹配的那一部分 – 那么一对迭代器指向输入字符串的结尾，并且这个子表达式的matched属性为false。子表达式从左向右，从1开始索引，子表达式0是整个表达式。（译注：上述表达式或子表达式都是指正则表达式。）

### 非标记圆括号(Non - Marking Parenthesis)

有时你需要使用圆括号组成一个子表达式，但是不像要产生一个标记的子表达式（译注：在match_results中的表达式都是标记的子表达式）。在这种情况下，非标记圆括号(? : expression) 可以使用。例如下列表达式不产生子表达式：“(? : abc)*”

### 前看断言(Forward Lookahead Asserts)

这有两种形式：一个是正的前看断言；一个是负的前看断言：

- “(? = abc)” 匹配0个字符，除非表达式以 “abc” 开头。
- “(? !abc)” 匹配0个字符，除非表达式不以 “abc” 开头。

（译注：断言并不匹配，例如： “(? = abc)abcdef” 匹配 “abcdef” ，前面的 “(? = abc)” 并不匹配 “abc” ，而是查看是否已abc开头，如果需要匹配 “abc” 还是需要在后面写上的。）

### 独立子表达式(Independent sub - expressions)

“(? > expression)” 匹配 “expression” 作为一个独立的原子动作（除非产生错误，算法不会回退产看）。