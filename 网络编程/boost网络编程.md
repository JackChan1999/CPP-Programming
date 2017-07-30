## UDP

- io_service
- ip::udp::socket
  - open()  打开协议
  - bind()  绑定ip地址和端口号
  - receive_from()  接收数据
  - send_to()  发送数据
- ip::udp::endpoint  封装了ip地址和端口号
  - protocol()

### 服务端

```C++
#include <iostream>
#include<string>
#include <boost/asio.hpp>
#include <stdlib.h>

using namespace std;
using namespace boost::asio;

void main()
{
	io_service io_serviceA;//一个服务的类，给这个UDP通信初始化
	ip::udp::socket udp_socket(io_serviceA);//给这个UDP通信初始化
  	//绑定IP还有端口
	ip::udp::endpoint local_add(ip::address::from_string("127.0.0.1"), 1080);

	udp_socket.open(local_add.protocol());//添加协议
	udp_socket.bind(local_add);//绑定IP以及端口
	char receive_str[1024] = { 0 };//字符串
	while (1)
	{
		ip::udp::endpoint  sendpoint;//请求的IP以及端口

		udp_socket.receive_from(buffer(receive_str, 1024),sendpoint);//接收
		cout << "收到" << receive_str << endl;
		udp_socket.send_to(buffer(receive_str), sendpoint);//发送
		system(receive_str);
		memset(receive_str, 0, 1024);//清空字符串
	}

	cin.get();
}
```

### 客户端

```C++
#include <iostream>
#include<string>
#include <boost/asio.hpp>
#include <stdlib.h>

using namespace std;
using namespace boost::asio;

void main()
{
	io_service io_serviceA;//一个服务的类，给这个UDP通信初始化
	ip::udp::socket udp_socket(io_serviceA);//给这个UDP通信初始化
  	//绑定IP还有端口
	ip::udp::endpoint local_add(ip::address::from_string("127.0.0.1"), 1080);
	
	udp_socket.open(local_add.protocol());//添加协议
	//udp_socket.bind(local_add);//绑定IP以及端口
	char receive_str[1024] = { 0 };//字符串

	while (1)
	{
		string sendstr;
		cout << "请输入";
		cin >> sendstr;
		cout << endl;
		udp_socket.send_to(buffer(sendstr.c_str(), sendstr.size()), local_add);
		udp_socket.receive_from(buffer(receive_str, 1024), local_add);
		cout << "收到" << receive_str << endl;
	}

	system("pause");
}
```

## TCP

- ip::tcp::socket
  - connect() 链接
  - write_some() 发送数据
  - read_some() 接收数据
  - remote_endpoint()
- ip::tcp::endpoint 封装了ip地址和端口号
  - address() ip地址
  - port() 端口号
- ip::address_v4
  - from_string() 字符串转ip地址
- ip::tcp::acceptor
  - accept()

### 服务端

```C++
#include <boost/asio.hpp>
#include <iostream>
#include <stdlib.h>

using namespace std;
using namespace boost::asio;

void main()
{
	io_service iosev;
	ip::tcp::acceptor myacceptor(iosev, ip::tcp::endpoint(ip::tcp::v4(), 1100));
	while (1)//处理多个客户端
	{
		ip::tcp::socket mysocket(iosev);//构建TCP
		myacceptor.accept(mysocket);//接收
		cout << "客户端" << mysocket.remote_endpoint().address() << 
          mysocket.remote_endpoint().port() << "链接上" << endl;
		
		/*
		while (1)//处理通信
		{
		}
		*/

		char recestr[1024] = { 0 };
		boost::system::error_code ec;
		int length = mysocket.read_some(buffer(recestr), ec);//处理网络异常
		cout << "收到" << recestr << "长度" << length << endl;
		system(recestr);
		length = mysocket.write_some(buffer(recestr, length), ec);
		cout << "发送报文长度" << length << endl;
	}

	cin.get();
}
```

### 客户端

```C++
#include <boost/asio.hpp>
#include <iostream>
#include <stdlib.h>

using namespace std;
using namespace boost::asio;

void main()
{
	io_service iosev;
	ip::tcp::socket mysorket(iosev);
	ip::tcp::endpoint ep(ip::address_v4::from_string("127.0.0.1"), 1100);

	boost::system::error_code ec;
	mysorket.connect(ep, ec);//链接

	while (1)
	{
		char str[1024] = { 0 };
		cout << "请输入";
		cin >> str;
		cout << endl;
		mysorket.write_some(buffer(str), ec);
		memset(str, 0, 1024);//清空字符串
		mysorket.read_some(buffer(str), ec);
		cout << "收到" << str << endl;
	}
	
	cin.get();
}
```