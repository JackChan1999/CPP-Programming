一. 通用命令:[6]

1. date ：print or set the system date and time
2. stty -a: 可以查看或者打印控制字符(Ctrl-C, Ctrl-D, Ctrl-Z等)
3. passwd: print or set the system date and time (用passwd -h查看)
4. logout, login: 登录shell的登录和注销命令
5. pwd: print or set the system date and time
6. more, less, head tail: 显示或部分显示文件内容.
7. lp/lpstat/cancel, lpr/lpq/lprm: 打印文件.
8. 更改文件权限： chmod u+x...
9. 删除非空目录：rm -fr dir
    10.拷贝目录： cp -R dir
10. fg jobid :可以将一个后台进程放到前台。
   Ctrl-z 可以将前台进程挂起(suspend), 然后可以用bg jobid 让其到后台运行。
   　　job & 可以直接让job直接在后台运行。
11. kill 的作用: send a signal to a process. eg: kill -9 发送的是SIG_KILL信号。。。 具体发送什么信号 可以通过 man kill 查看。
12. ps 的用法， ps -e 或 ps -o pid,ppid,session,tpgid, comm (其中session显示的sessionid, tpgid显示前台进程组id, comm显示命令名称。)
    二 .ubuntu常用命令:[6]
13. dpkg: package manager for Debian
* 安装： dpkg -i package
* 卸载： dpkg -r package
* 卸载并删除配置文件: dpkg -P |--purge package
* 如果安装一个包时。说依赖某些库。 可以先 apt-get install somelib...
* 查看软件包安装内容 :dpkg -L package
* 查看文件由哪个软件包提供: dpkg -S filename
* 另外 dpkg还有 dselect和aptitude 两个frontend.
　　2. apt
* 安装: apt-get install packs
* apt-get update : 更新源
* apt-get upgrade: 升级系统。
* apt-get dist-upgrade: 智能升级。安装新软件包,删除废弃的软件包
* apt-get -f install ： -f == --fix broken 修复依赖
* apt-get autoremove: 自动删除无用的软件
* apt-get remove packages :删除软件
* apt-get remove package --purge 删除包并清除配置文件
* 清除所以删除包的残余配置文件: dpkg -l |grep ^rc|awk '{print $2}' |tr ["/n"] [" "]|sudo xargs dpkg -P
* 安装软件时候包的临时存放目录 : /var/cache/apt/archives
* 清除该目录: apt-get clean
* 清除该目录的旧版本的软件缓存: apt-get autoclean
* 查询软件some的依赖包： apt-cache depends some
* 查询软件some被哪些包依赖: apt-get rdepends some
* 搜索软件: apt-cache search name|regexp
* 查看软件包的作用：apt-cache show package
* 查看一个软件的编译依赖库: apt-cache showsrc packagename|grep Build-Depends
* 下载软件的源代码 : apt-get source packagename (注: sources.list 中应该有 deb-src 源)
* 安装软件包源码的同时, 安装其编译环境 :apt-get build-dep packagename (有deb-src源)
* 如何将本地光盘加入安装源列表: apt-cdrom add
　　3. 系统命令:
* 查看内核版本： uname -a
* 查看ubuntu 版本: cat /etc/issue
* 查看网卡状态 : ethtool eth0
* 查看内存,cpu的信息： cat /proc/meminfo ; cat /proc/cpuinfo
    (/proc下面的有很多系统信息)
* 打印文件系统空间使用情况: df -h
* 查看硬盘分区情况: fdisk -l
* 产看文件大小: du -h filename;
* 查看目录大小： du -hs dirname ; du -h dirname是查看目录下所有文件的大小
* 查看内存的使用： free -m|-g|-k
* 查看进程： ps -e 或ps -aux -->显示用户
* 杀掉进程: kill pid
* 强制杀掉： killall -9 processname
　　4. 网络相关： 
* 配置 ADSL: sudo pppoeconf
* ADSL手工拨号: sudo pon dsl-provider
* 激活 ADSL : sudo /etc/ppp/pppoe_on_boot
* 断开 ADSL: sudo poff
* 根据IP查网卡地址: arping IP地址
* 产看本地网络信息（包括ip等）: ifconfig | ifconfig eth0
* 查看路由信息: netstat -r
* 关闭网卡： sudo ifconfig eth0 down
* 启用网卡： sudo ifconfig eth0 up
* 添加一个服务: sudo update-rc.d 服务名 defaults 99
* 删除一个服务: sudo update-rc.d 服务名 remove
* 临时重启一个服务: /etc/init.d/服务名 restart
* 临时关闭一个服务: /etc/init.d/服务名 stop
* 临时启动一个服务: /etc/init.d/服务名 start
* 控制台下显示中文: sudo apt-get install zhcon
* 查找某个文件: whereis filename 或 find 目录 -name 文件名
    *通过ssh传输文件
    　　scp -rp /path/filename username@remoteIP:/path #将本地文件拷贝到服务器上
    　　scp -rp username@remoteIP:/path/filename /path #将远程文件从服务器下载到本地
　　5. 压缩:
    *解压缩 a.tar.gz: tar zxvf a.tar.gz
    　　*解压缩 a.tar.bz2: tar jxvf a.tar.bz2
    　　*压缩aaa bbb目录为xxx.tar.gz: tar zcvf xxx.tar.gz aaa bbb
    　　*压缩aaa bbb目录为xxx.tar.bz2: tar jcvf xxx.tar.bz2 aaa bbb[6]
　　6. Nautilus：
    特殊 URI 地址
* computer:/// - 全部挂载的设备和网络
* network:/// - 浏览可用的网络
* burn:/// - 一个刻录 CDs/DVDs 的数据虚拟目录
* smb:/// - 可用的 windows/samba 网络资源
* x-nautilus-desktop:/// - 桌面项目和图标
* file:/// - 本地文件
* trash:/// - 本地回收站目录
* ftp:// - FTP 文件夹
* ssh:// - SSH 文件夹
* fonts:/// - 字体文件夹，可将字体文件拖到此处以完成安装
* themes:/// - 系统主题文件夹
* 显示隐藏文件: Ctrl+h
* 显示地址栏: Ctrl+l
* 查看已安装字体: 在nautilus的地址栏里输入”fonts:///“，就可以查看本机所有的fonts[6]
    7.补充部分：
* 查看本地所有的tpc,udp监听端口: netstat -tupln (t=tcp, u=udp, p=program, l=listen, n=numric)
* 通过man搜说相关命令: man -k keyword . eg: man -k user
* 或者用 apropos
* 统计文件所占用的实际磁盘空间： du (du - estimate file space usage)
* 统计文件中的字符，字节数: wc -c/-l/-w (wc - print the number of newlines, words, and bytes in files)
* 查看文件的内容： od -x/-c/.... (od - dump files in octal and other formats)
    我认为od最有用的就是文件的字节流了: od -t x1 filename
    　　查看文件的 Ascii 码形式: od -t c filename (其中统计信息最左边的是： 字节数)
* 查找命令所在文件的位置： which od 输出： /usr/bin/od
    查看该文件由哪个包提供： dpkg -S /usr/bin/od 输出: coreutils: /usr/bin/od
    　　再查看coreutils包的全部内容就知道了linux的核心命令: dpkg -L coreutils
    　　然后 info coreutils 哈哈，认真学吧， 满世界都是命令!
* 可以用man 命令产看某个命令的所有section 的解释: man -a tty
    然后用q,和next 转换到下一个section的解释
* bash 的好用的快捷键:
    ctrl+a:光标移到行首。
    　　ctrl+b:光标左移一个字母
    　　ctrl+c:杀死当前进程。
    　　ctrl+d:退出当前 Shell。
    　　ctrl+e:光标移到行尾。
    　　ctrl+h:删除光标前一个字符，同 backspace 键相同。
    　　ctrl+k:清除光标后至行尾的内容。
    　　ctrl+l:清屏，相当于clear。
    　　ctrl+r:搜索之前打过的命令。会有一个提示，根据你输入的关键字进行搜索bash的history
    　　ctrl+u: 清除光标前至行首间的所有内容。
    　　ctrl+w: 移除光标前的一个单词
    　　ctrl+t: 交换光标位置前的两个字符
    　　ctrl+y: 粘贴或者恢复上次的删除
    　　ctrl+d: 删除光标所在字母;注意和backspace以及ctrl+h的区别，这2个是删除光标前的字符
    　　ctrl+f: 光标右移
    　　ctrl+z : 把当前进程转到后台运行，使用’ fg ‘命令恢复。比如top -d1 然后ctrl+z ，到后台，然后fg,重新恢复
* 快速粘贴：先在一个地方选中文字，在欲粘贴的地方按鼠标 中键 即可。
* 等效中键：a 、按下滑轮等效于中键。b、同时按下鼠标 左右键，等效于中键。
* 快速重启X服务： 同时按下： Alt + Ctrl + Backspace 三个键。
* 打开"运行"窗口： 同时按下 Alt + F2 键。
* 戴屏： a、全屏：直接按下 PrtScr 键。
    b、当前窗口：同时按下 Alt + PrtScr 键。
    　　c、延时戴屏：在 终端 或 "运行"窗口中输入命令： gnome-screenshot --delay 3 ，将延时 3 秒后戴屏。
* 直接将 文件管理器 中的文件拖到 GNOME终端 中就可以在终端中得到完整的路径名。[6]  8.ulimit
    ulimit：显示（或设置）用户可以使用的资源的限制（limit），这限制分为软限制（当前限制）和硬限制（上限），其中硬限制是软限制的上限值，应用程序在运行过程中使用的系统资源不超过相应的软限制，任何的超越都导致进程的终止。
    　　ulimited 不限制用户可以使用的资源，但本设置对可打开的最大文件数（max open files）
    　　和可同时运行的最大进程数（max user processes）无效
    　　-a 列出所有当前资源极限
    　　-c 设置core文件的最大值.单位:blocks
    　　-d 设置一个进程的数据段的最大值.单位:kbytes
    　　-f Shell 创建文件的文件大小的最大值，单位：blocks
    　　-h 指定设置某个给定资源的硬极限。如果用户拥有 root 用户权限，可以增大硬极限。任何用户均可减少硬极限
    　　-l 可以锁住的物理内存的最大值
    　　-m 可以使用的常驻内存的最大值,单位：kbytes
    　　-n 每个进程可以同时打开的最大文件数
    　　-p 设置管道的最大值，单位为block，1block=512bytes
    　　-s 指定堆栈的最大值：单位：kbytes
    　　-S 指定为给定的资源设置软极限。软极限可增大到硬极限的值。如果 -H 和 -S 标志均未指定，极限适用于以上二者
    　　-t 指定每个进程所使用的秒数,单位：seconds
    　　-u 可以运行的最大并发进程数
    　　-v Shell可使用的最大的虚拟内存，单位：kbytes
    　　eg: ulimit -c 1000(可以先通过ulimit -c 查看原来的值)