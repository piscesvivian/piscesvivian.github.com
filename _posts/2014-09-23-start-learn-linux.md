---
layout: post
title: "start learn linux"
description: "learn how to use linux"
category: learn
tags: [linux]
---
{% include JB/setup %}

#### 一：cpu
```
[root@srv /]# more /proc/cpuinfo | grep "model name"
model name    : Intel(R) Xeon(R) CPU          X3220 @ 2.40GHz
model name    : Intel(R) Xeon(R) CPU          X3220 @ 2.40GHz
model name    : Intel(R) Xeon(R) CPU          X3220 @ 2.40GHz
model name    : Intel(R) Xeon(R) CPU          X3220 @ 2.40GHz

[root@srv /]# grep "model name" /proc/cpuinfo
model name    : Intel(R) Xeon(R) CPU          X3220 @ 2.40GHz
model name    : Intel(R) Xeon(R) CPU          X3220 @ 2.40GHz
model name    : Intel(R) Xeon(R) CPU          X3220 @ 2.40GHz
model name    : Intel(R) Xeon(R) CPU          X3220 @ 2.40GHz

[root@srv /]# grep "model name" /proc/cpuinfo | cut -f2 -d:
Intel(R) Xeon(R) CPU          X3220 @ 2.40GHz
Intel(R) Xeon(R) CPU          X3220 @ 2.40GHz
Intel(R) Xeon(R) CPU          X3220 @ 2.40GHz
Intel(R) Xeon(R) CPU          X3220 @ 2.40GHz
```

#### 二：内存
```
[root@srv /]# grep MemTotal /proc/meminfo
MemTotal:        614400 kB
[root@srv /]# free -m
			total       used        free    shared     buffers    cached
Mem:          600       23       576           0           0           0
-/+ buffers/cache:       23       576
Swap:          0           0           0
[root@srv /]# free -m |grep "Mem" | awk '{print $2}'
600
```

#### 三：查看CPU位数(32 or 64)
```
[root@srv /]# getconf LONG_BIT
32
```

#### 四：查看linux版本
```
[root@srv /]# more /etc/redhat-release
CentOS release 5 (Final)
[root@srv /]# more /etc/issue
CentOS release 5 (Final)
Kernel \r on an \m
[root@srv /]# more /proc/version
Linux version 2.6.18-92.1.18.el5.028stab060.2PAE ([email=root@rhel5-32-build-xemul]root@rhel5-32-build-xemul[/email]) (gc
c version 4.1.2 20071124 (Red Hat 4.1.2-42)) #1 SMP Tue Jan 13 12:31:30 MSK 2009
```

#### 五：查看内核版本
```
[root@srv /]# uname -r
2.6.18-92.1.18.el5.028stab060.2PAE
[root@srv /]# uname -a
Linux srv.eddiechen.cn 2.6.18-92.1.18.el5.028stab060.2PAE #1 SMP Tue Jan 13 12:31:30 MSK 2009 i686 i686 i386 GNU/Linux
```

#### 六：查看时区
```
[root@srv /]# date -R
Wed, 25 Feb 2009 02:20:50 +0000
[root@srv /]# mv /etc/localtime /etc/localtime.save
[root@srv /]# cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
[root@srv /]# date -R
Wed, 25 Feb 2009 10:24:26 +0800
```

#### 七：主机名
1. 查看主机名

```
[root@srv /]# hostname
www.ifuoo.com
```

2. 修改主机名

```
[root@srv /]# cat /etc/sysconfig/network
```

#### 八：查看selinux情况
```
[root@srv /]# sestatus
SELinux status:                disabled
```

#### 九：网络
1. IP

```
[root@srv /]# ifconfig | grep 'inet addr:'| grep -v '127.0.0.1' | cut -d: -f2 | awk '{ print $1}'
207.154.202.216
```

2. 网关

```
[root@srv /]# cat /etc/sysconfig/network
NETWORKING="yes"
GATEWAY="192.0.2.1"
HOSTNAME="srv.eddiechen.cn"
```

3. dns

```
[root@srv /]# cat /etc/resolv.conf
nameserver 208.74.168.131
nameserver 208.74.168.132
nameserver 4.2.2.1
```

4. 修改Host文件

```
[root@srv /]# cat /etc/hosts
```

#### 十：已经安装的软件包
```
[root@srv /]# rpm -qa | wc -l
197
[root@srv /]# yum list installed | wc -l
198
```

#### 十一：磁盘和分区
```
[root@srv /]# df -h
Filesystem          Size    Used          Avail Use    %    Mounted on
/dev/simfs          10G     353M               9.7G   4%    /
[root@srv /]# du -sh
353M
[root@srv /]# du /etc -sh
4.6M     /etc
```

#### 十二：查看键盘布局
```
cat /etc/sysconfig/keyboard
cat /etc/sysconfig/keyboard | grep KEYTABLE | cut -f2 -d=
```

#### 十三：查看默认语言
```
echo $LANG $LANGUAGE
cat /etc/sysconfig/i18n
```

==================================
http://hi.baidu.com/mypc007

-----------------------------------------------------
通过以下命令，可以查看RS/6000系统配备的物理内存的大小。

```
[root@srv /]#lsdev -Cc memory
mem0 Available 00-00 Memory
L2cache0 Available 00-00 L2 Cache
```

再使用命令

```
[root@srv /]# lsattr -El mem0
size 512 Total amount of physical memory in Mbytes False
goodsize 512 Amount of usable physical memory in Mbytes False
```

此例说明机器的物理内存为512MB。如果前面lsdev的输出中有设备名 mem1，则使用同样的命令查看其对应的大小并依此类推。L2cache0 为系统二级缓存(Level 2 Cache)的设备名。同样，使用命令：

```
lsattr -El L2cache0
```

可以查看其大小。

查看LINUX系统位数

1. 编程实现：

在程序中返回sizeof(int)的值，返回的结果是操作系统的字节数。若返回4则是32位操作系统，返回8即是64位。

2. getconf命令：

getconf命令可以获取系统的基本配置信息，比如操作系统位数，内存大小，磁盘大小等。

例如：

确定磁盘 hdisk0 大小，若是 root 用户，则输入：
```
[root@srv /]#getconf DISK_SIZE /dev/hdisk0
```
确定实际内存大小：
```
[root@srv /]#getconf REAL_MEMORY
```
确定是否机器硬件是 32 位或 64 位：
```
[root@srv /]#getconf HARDWARE_BITMODE
```
确定是否内核是 32 位或 64 位：
```
[root@srv /]#getconf KERNEL_BITMODE
```
若以上的getconf KERNEL_BITMODE方法不成功(在我的机器上就不成功)，可能是因为版本不一致，可以再尝试用：getconf WORD_BIT，这个命令返回int类型的长度，与sizeof(int)一致。


----------------------------------------------------------------------华丽丽的分隔线---------------------------------------------------------------------
SecureCRT设置日志：

1. 在菜单里选择：“选项”——“全局选项”
2. 然后选择：常规——默认会话——编辑默认设置
3. 然后选择：日志文件

a.在日志文件名里填入你想保存的日志路径名加日志文件名：
`D:\ProgramData\SecureCRT\Logs\%H\%Y-%M-%D_%h%m%s.log`

b.打勾：
v 在连接上开始记录日志
v 追加到文件
v 半夜时启用新日志

c.自定义日志数据

连接时： `[%Y-%M-%D-%h:%m:%s]`
断开时：
在每行：`[%h:%m:%s]`

这里为了可以每个会话都打成一个日志，可以采用支持的参数
%H 主机名 %S 会话名
%Y 年份 %M 月份 %D 日
%h 小时 %m 分钟 %s 秒

例如我填写的 E:\Development\SecureCRT\Logs\%H\%Y-%M-%D_%h%m%s.log
就是会保持在 E:\Development\SecureCRT\Logs\ 目录下，路径里也可以使用参数 \%H\这样设置可以把同一个主机的日志到到一个文件夹里，文件夹名就是主机名，没有会自动创建文件夹
这里可以勾选上连接上开始记录日志，因为我们经常开着SecureCRT，但不一定一直在用，为了知道我输入的每一行命令是在什么时候，可以在“在每行”这个设置里填写[%h:%m:%s]，这样就会记录每行日志打入的时间

日志样例：
位置： `D:\ProgramData\SecureCRT\Logs\cts-gateway.sh.intel.com\2013-09-05_144909.log`

```log4j
[14:49:09][2013-09-05-14:49:09]
[14:49:09]Last login: Thu Sep  5 14:42:27 2013 from os-controller.cloudtest.intel.com
[14:53:46][root@server-21523 ~]# ll
[14:53:46]total 20
[14:53:46]-rw-------. 1 root root  904 Jul 21  2012 anaconda-ks.cfg
[14:53:46]-rw-r--r--. 1 root root 8734 Jul 21  2012 install.log
[14:53:46]-rw-r--r--. 1 root root 3161 Jul 21  2012 install.log.syslog
[14:53:53][root@server-21523 ~]# cd /
[14:53:54][root@server-21523 /]# ll
[14:53:54]total 88
[14:53:54]dr-xr-xr-x.   2 root root  4096 Sep  4 14:20 bin
[14:53:54]dr-xr-xr-x.   4 root root  4096 Jul 21  2012 boot
[14:53:54]drwxr-xr-x.  16 root root  3440 Sep  4 13:32 dev
[14:53:54]drwxr-xr-x.  86 root root  4096 Sep  4 14:42 etc
[14:53:54]drwxr-xr-x.   5 root root  4096 Sep  4 14:20 hadoop
[14:53:54]drwxr-xr-x.   2 root root  4096 Sep 23  2011 home
[14:53:54]dr-xr-xr-x.   9 root root  4096 Sep  4 14:17 lib
[14:53:54]dr-xr-xr-x.   8 root root 12288 Sep  4 14:20 lib64
[14:53:54]drwx------.   2 root root 16384 Jul 21  2012 lost+found
[14:53:54]drwxr-xr-x.   2 root root  4096 Sep 23  2011 media
[14:53:54]drwxr-xr-x.   2 root root  4096 Sep 23  2011 mnt
[14:53:54]drwxr-xr-x.   2 root root  4096 Sep 23  2011 opt
[14:53:54]dr-xr-xr-x. 135 root root     0 Sep  4 13:32 proc
[14:53:54]dr-xr-x---.   3 root root  4096 Sep  4 14:42 root
[14:53:54]dr-xr-xr-x.   2 root root  4096 Sep  4 14:20 sbin
[14:53:54]drwxr-xr-x.   7 root root     0 Sep  4 13:32 selinux
[14:53:54]drwxr-xr-x.   2 root root  4096 Sep 23  2011 srv
[14:53:54]drwxr-xr-x.  13 root root     0 Sep  4 13:32 sys
[14:53:54]drwxrwxrwt.  22 root root  4096 Sep  5 13:40 tmp
[14:53:54]drwxr-xr-x.  14 root root  4096 Sep  4 14:07 usr
[14:53:54]drwxr-xr-x.  21 root root  4096 Sep  4 14:45 var
```

----------------------------------------------------------------------华丽丽的分隔线---------------------------------------------------------------------

在linux下一些常用的关机/重启命令有shutdown、halt、reboot、及init，它们都可以达到重启系统的目的，但每个命令的内部工作过程是不同的。

Linux centos重启命令：

1. reboot
2. shutdown -r now 立刻重启(root用户使用)
3. shutdown -r 10 过10分钟自动重启(root用户使用)
4. shutdown -r 20:35 在时间为20:35时候重启(root用户使用)

如果是通过shutdown命令设置重启的话，可以用shutdown -c命令取消重启

Linux centos关机命令：

1. halt 立刻关机
2. poweroff 立刻关机
3. shutdown -h now 立刻关机(root用户使用)
4. shutdown -h 10 10分钟后自动关机

如果是通过shutdown命令设置关机的话，可以用shutdown -c命令取消重启

1. shutdown
shutdown命令安全地将系统关机。
有些用户会使用直接断掉电源的方式来关闭linux，这是十分危险的。
因为linux与windows不同，其后台运行着许多进程，所以强制关机可能会导致进程的数据丢失，使系统处于不稳定的状态，甚至在有的系统中会损坏硬件设备。
而在系统关机前使用shutdown命令﹐系统管理员会通知所有登录的用户系统将要关闭。并且login指令会被冻结，即新的用户不能再登录。
直接关机或者延迟一定的时间才关机都是可能的，还可能重启。这是由所有进程〔process〕都会收到系统所送达的信号〔signal〕决定的。
这让像vi之类的程序有时间储存目前正在编辑的文档﹐而像处理邮件〔mail〕和新闻〔news〕的程序则可以正常地离开等等。
shutdown执行它的工作是送信号〔signal〕给init程序，要求它改变runlevel。
Runlevel 0被用来停机〔halt〕，
runlevel 6是用来重新激活〔reboot〕系统
runlevel 1则是被用来让系统进入管理工作可以进行的状态，这是预设的。假定没有-h也没有-r参数给shutdown。
要想了解在停机〔halt〕或者重新开机〔reboot〕过程中做了哪些动作。
你可以在这个文件/etc/inittab里看到这些runlevels相关的资料。
shutdown 参数说明:
[-t] 在改变到其它runlevel之前﹐告诉init多久以后关机。
[-r] 重启计算器。
[-k] 并不真正关机﹐只是送警告信号给每位登录者〔login〕。
[-h] 关机后关闭电源〔halt〕。
[-n] 不用init﹐而是自己来关机。不鼓励使用这个选项﹐而且该选项所产生的后果往往不总是你所预期得到的。
[-c] cancel current process取消目前正在执行的关机程序。所以这个选项当然没有时间参数﹐但是可以输入一个用来解释的讯息﹐而这信息将会送到每位使用者。
[-f] 在重启计算器〔reboot〕时忽略fsck。
[-F] 在重启计算器〔reboot〕时强迫fsck。
[-time] 设定关机〔shutdown〕前的时间。

2. halt----最简单的关机命令
其实halt就是调用shutdown -h。halt执行时﹐杀死应用进程﹐执行sync系统调用﹐文件系统写操作完成后就会停止内核。
参数说明:
[-n] 防止sync系统调用﹐它用在用fsck修补根分区之后﹐以阻止内核用老版本的超级块〔superblock〕覆盖修补过的超级块。
[-w] 并不是真正的重启或关机﹐只是写wtmp〔/var/log/wtmp〕纪录。
[-d] 不写wtmp纪录〔已包含在选项[-n]中〕。
[-f] 没有调用shutdown而强制关机或重启。
[-i] 关机〔或重启〕前﹐关掉所有的网络接口。
[-p] 该选项为缺省选项。就是关机时调用poweroff。

3. reboot
reboot的工作过程差不多跟halt一样﹐不过它是引发主机重启﹐而halt是关机。它的参数与halt相差不多。

4. init
init是所有进程的祖先，它的进程号始终为1，所以发送TERM信号给init会终止所有的用户进程、守护进程等。
shutdown 就是使用这种机制。
init定义了8个运行级别(runlevel)，
init 0为关机﹐
init 1为重启。
关于init可以长篇大论，这里就不再叙述。
另外还有telinit命令可以改变init的运行级别。
比如：telinit -iS 可使系统进入单用户模式，并且得不到使用shutdown时的信息和等待时间。

----------------------------------------------------------------------华丽丽的分隔线---------------------------------------------------------------------
##### 神器的bash脚本

`:() { :|:& }; :`                              # <--- 這個別亂跑！好奇會死人的！

`echo '十人|日一|十十o' | sed 's/.../&\n/g'`   # <--- 跟你講就不聽，再跑這個好了...

----------------------------------------------------------------------华丽丽的分隔线---------------------------------------------------------------------
##### 关于SVN目录结构
Subversion有一个很标准的目录结构，是这样的。比如项目是proj，svn地址为svn://proj/，那么标准的svn布局是

```
svn://proj/
|
+-trunk
+-branches
+-tags
```
这是一个标准的布局，trunk为主开发目录，branches为分支开发目录，tags为tag存档目录（不允许修改）。
但是具体这几个目录应该如何使用，svn并没有明确的规范，更多的还是用户自己的习惯。
对于这几个开发目录，一般的使用方法有两种。
我更多的是从软件产品的角度出发 （比如freebsd），因为互联网的开发模式是完全不一样的。
第一种：使用trunk作为主要的开发目录。
一般的：我们的所有的开发都是基于trunk进行开发。
        当一个版本/release开发告一段落（开发、测试、文档、制作安装程序、打包等）结束后，代码处于冻结状态（人为规定，可以通过hook来进行管理）。
        此时应该基于当前冻结的代码库，打tag。当下一个版本/阶段的开发任务开始，继续在trunk 进行开发。
        此时，如果发现了上一个已发行版本（Released Version）有一些bug，或者一些很急迫的功能要求，而正在开发的版本（Developing Version）无法满足时间要求，这时候就需要在上一个版本上进行修改了。
        应该基于发行版对应的tag，做相应的分支（branch）进行开发。
        例如，刚刚发布1.0，正在开发2.0，此时要在1.0的基础上进行bug修正。
按照时间的顺序

1.0开发完毕，代码冻结
基于已经冻结的trunk，为release1.0打tag
此时的目录结构：


```
svn://proj/
+trunk/ (freeze)
+branches/
+tags/
    +tag_release_1.0　(copy from trunk)
```

2.0 开始开发，trunk此时为2.0的开发版
发现1.0有bug，需要修改，基于1.0的tag做branch
此时的目录结构：

```
svn://proj/
+trunk/ ( dev 2.0 )
+branches/
    +dev_1.0_bugfix (copy from tag/release_1.0)
+tags/
    +release_1.0　(copy from trunk)
```
在1.0 bugfix branch进行1.0 bugfix开发，在trunk进行2.0开发
在1.0 bugfix 完成之后，基于dev_1.0_bugfix的branch做release等
根据需要选择性的把dev_1.0_bugfix这个分支merge回trunk（什么时候进行这步操作，要根据具体情况）
这是一种很标准的开发模式，很多的公司都是采用这种模式进行开发的。
trunk永远是开发的主要目录。

第二种方法，在每一个release的branch中进行各自的开发，trunk只做发布使用。

这种开发模式当中，trunk是不承担具体开发任务的，一个版本/阶段的开发任务在开始的时候，根据已经 release的版本做新的开发分支，并且基于这个分支进行开发。
还是举上面的例子，这里面的时序关系是。

1.0开发，做 dev1.0的branch
此时的目录结构：

```
svn://proj/
+trunk/ (不担负开发任务 )
+branches/
    +dev_1.0 (copy from trunk)
+tags/
```
1.0开发完成，merge dev1.0到trunk
此时的目录结构：

```
svn://proj/
+trunk/ (merge from branch dev_1.0)
+branches/
     +dev_1.0 (开发任务结束，freeze)
+tags/
```
根据trunk做1.0的tag
此时的目录结构

```
svn://proj/
+trunk/ (merge from branch dev_1.0)
+branches/
    +dev_1.0 (开发任务结束，freeze)
+tags/
    +tag_release_1.0 (copy from trunk)
```

1.0开发，做dev2.0分支
此时的目录结构

```
svn://proj/
+trunk/
+branches/
    +dev_1.0 (开发任务结束，freeze)
    +dev_2.0 （进行2.0开发）
+tags/
    +tag_release_1.0 (copy from trunk)
```

1.0有bug，直接在dev1.0的分支上修复
此时的目录结构

```
svn://proj/
+trunk/
+branches/
    +dev_1.0 (1.0bugfix)
    +dev_2.0 （进行2.0开发）
+tags/
    +tag_release_1.0 (copy from trunk)
```
选择性的进行代码merge
这其实是一种分散式的开发，当各个部分相对 独立一些（功能性的），可以开多个dev的分支进行开发，这样各人/组都不会相互影响。
比如dev_2.0_search和dev_2.0_cache 等。
但是这样merge起来就是一个很痛苦的事情。

这里要注意一下的，第六步进行选择性的merge，是可以当2.0开发结束后一起把 dev_1.0（bugfix用）和dev_2.0（新版本开发 用）merge回trunk。
或者先把dev_1.0 merge到dev_2.0，进行测试等之后再merge回trunk。
这两种方法各有利弊，第一种方法是可以得到一个比较纯的dev_2.0的 开发分支，而第二种方法则更加的保险，因为要测试嘛。

以上呢，就是我说的两种开发模式了，具体哪种好，并没有定论。
这里大致的说一下各自的优缺点:
第一种开发模式（trunk进行主要开发，集中式）：

* 优点：管理简单
* 缺点：当开发的模块比较多，开发人数/小团队比较多 的时候，很容易产生冲突而影响对方的开发。因为所有的改动都有可能触碰对方的改动

第二重开发模式（分支进行主要开发，分散式）：

* 优点：各 自开发独立，不容易相互影响。
* 缺点：管理复杂，merge的时候很麻烦，容易死人。

其实，这里并没有一定之规，更多的时候是两种 模式结合使用。

----------------------------------------------------------------------华丽丽的分隔线---------------------------------------------------------------------
##### 内存统计方法
1. 内存统计

```
ps aux | awk '{sum +=$4}; END {print sum}'
```

2. 内存统计

```
free -m | awk '{if(NR==3) print $3/($3+$4)}'
```

3. 内存统计

```
while :
do
printf "`free -m | awk '{if(NR==3) print $3/($3+$4)}'`\r"
done;
echo
```

----------------------------------------------------------------------华丽丽的分隔线---------------------------------------------------------------------
##### 常见性能测试

###### 网卡速率查看

```
[root@node4 ~]#  ethtool em1
Settings for em1:
        Supported ports: [ TP ]
        Supported link modes:   10baseT/Half 10baseT/Full
                                100baseT/Half 100baseT/Full
                                1000baseT/Half 1000baseT/Full
        Supported pause frame use: No
        Supports auto-negotiation: Yes
        Advertised link modes:  10baseT/Half 10baseT/Full
                                100baseT/Half 100baseT/Full
                                1000baseT/Half 1000baseT/Full
        Advertised pause frame use: Symmetric
        Advertised auto-negotiation: Yes
        Link partner advertised link modes:  10baseT/Half 10baseT/Full
                                             100baseT/Half 100baseT/Full
                                             1000baseT/Full
        Link partner advertised pause frame use: Symmetric
        Link partner advertised auto-negotiation: Yes
        Speed: 1000Mb/s
        Duplex: Full
        Port: Twisted Pair
        PHYAD: 1
        Transceiver: internal
        Auto-negotiation: on
        MDI-X: on
        Supports Wake-on: g
        Wake-on: d
        Current message level: 0x000000ff (255)
                               drv probe link timer ifdown ifup rx_err tx_err
        Link detected: yes
```

###### 硬盘速度测试

1. 硬盘读速度

```
[root@node4 ~]# time dd if=/dev/zero bs=1024 count=1000000 of=/1Gb.file
1000000+0 records in
1000000+0 records out
1024000000 bytes (1.0 GB) copied, 2.66318 s, 385 MB/s

real    0m2.685s
user    0m0.115s
sys     0m2.553s
```

2. 硬盘写入速度

```
[root@node4 ~]# time dd if=/1Gb.file bs=64k |dd of=/dev/null
15625+0 records in
15625+0 records out
1024000000 bytes (1.0 GB) copied, 2.50884 s, 408 MB/s
2000000+0 records in
2000000+0 records out
1024000000 bytes (1.0 GB) copied, 2.51124 s, 408 MB/s

real    0m2.514s
user    0m0.377s
sys     0m3.364s
```

3. 硬盘读写速度

```
[root@node4 ~]# time dd if=/1Gb.file of=/2Gb.file bs=64k
15625+0 records in
15625+0 records out
1024000000 bytes (1.0 GB) copied, 1.38182 s, 741 MB/s

real    0m1.384s
user    0m0.009s
sys     0m1.375s
```

4. 删除测试生成文件

```
[root@node4 ~]# cd /
[root@node4 /]# ll
total 2000114
-rw-r--r--.   1 root root 1024000000 Dec 24 16:54 1Gb.file
-rw-r--r--.   1 root root 1024000000 Dec 24 16:55 2Gb.file
dr-xr-xr-x.   2 root root       4096 Dec 13 03:10 bin
dr-xr-xr-x.   5 root root       1024 Dec 13 02:30 boot
drwxr-xr-x.   2 root root       4096 Oct 26 01:36 cgroup
drwxr-xr-x.  17 root root       4000 Dec 17 15:05 dev
drwxr-xr-x. 129 root root      12288 Dec 24 09:47 etc
drwxr-xr-x.   3 root root       4096 Dec 13 10:05 hadoop
drwxr-xr-x.   7 root root       4096 Dec 13 02:02 hadoop_data
drwxr-xr-x.   2 root root       4096 Jun 28  2011 home
dr-xr-xr-x.  13 root root       4096 Dec 13 03:10 lib
dr-xr-xr-x.   9 root root      12288 Dec 13 03:10 lib64
drwx------.   2 root root      16384 Dec 13 01:37 lost+found
drwxr-xr-x.   2 root root       4096 Jun 28  2011 media
drwxr-xr-x.   2 root root          0 Dec 13 02:30 misc
drwxr-xr-x.   2 root root       4096 Jun 28  2011 mnt
drwxr-xr-x.   2 root root          0 Dec 13 02:30 net
drwxr-xr-x.   3 root root       4096 Dec 13 02:24 opt
dr-xr-xr-x. 431 root root          0 Dec 13 02:29 proc
dr-xr-x---.  20 root root       4096 Dec 24 09:43 root
dr-xr-xr-x.   2 root root      12288 Dec 13 03:10 sbin
drwxr-xr-x.   7 root root          0 Dec 13 02:29 selinux
drwxr-xr-x.   2 root root       4096 Jun 28  2011 srv
drwxr-xr-x.  13 root root          0 Dec 13 02:29 sys
drwxrwxrwt.  32 root root       4096 Dec 24 16:41 tmp
drwxr-xr-x.  14 root root       4096 Dec 13 09:54 usr
drwxr-xr-x.  23 root root       4096 Dec 18 17:14 var
[root@node4 /]# rm 1Gb.file 2Gb.file
rm: remove regular file `1Gb.file'? y
yrm: remove regular file `2Gb.file'? y
```

----------------------------------------------------------------------华丽丽的分隔线---------------------------------------------------------------------
#### 3AWK:
处理csv时间：2014-01-09T08:33:30+08:00

```awk
awk -F, '{print mktime(gensub("[-:+T]"," ","g",substr($1,0,19)))}' node1.cpu_report.csv
awk -F, -v start="2014-01-09T08:55:00+08:00" -v end="2014-01-09T09:14:30+08:00" '
BEGIN{
starttime=mktime(gensub("[-:+T]"," ","g",start));
endtime=mktime(gensub("[-:+T]"," ","g",end));
}
{now = mktime(gensub("[-:+T]"," ","g",substr($1,0,19)));
if(now>=starttime && now<=endtime && $6!=0){
    a+=100-$6;
    b+=1;
}}
END{print a/b}' node1.cpu_report.csv
```

```awk
awk -F, -v start="2014-01-09T08:55:00+08:00" -v end="2014-01-09T09:14:30+08:00" '
BEGIN{
starttime=mktime(gensub("[-:+T]"," ","g",start));
endtime=mktime(gensub("[-:+T]"," ","g",end));
}
{now = mktime(gensub("[-:+T]"," ","g",substr($1,0,19)));
if(now>=starttime && now<=endtime && $2 > 0 && $8 > 0){
    a+=$2/$7*100;
    b+=1;
}}
END{print a/b}' node1.mem_report.csv
```


----------------------------------------------------------------------华丽丽的分隔线---------------------------------------------------------------------
##### 查看rpm内容

总是记不住这个命令，每用一次就google一次，有时候google的结果还找半天，记录在此。

列出rpm包的内容：
`rpm -qpl *.rpm`

解压rpm包的内容：（没有安装，就像解压tgz包一样rpm包）
`rpm2cpio *.rpm | cpio -div`

改天有时间研究一下rpm包压缩格式和cpio等命令，知其然知其所以然。

----------------------------------------------------------------------华丽丽的分隔线---------------------------------------------------------------------
#### Linux 时间同步设置

##### 一. 使用ntpdate 命令
1.1 服务器可链接外网时
`# crontab -e`
加入一行：
*/1 * * * * ntpdate 210.72.145.44
210.72.145.44 为中国国家授时中心服务器地址，这样该机每隔1分重就可以与国家授时中心进行同步了。
注意： 在使用ntpdate 命令时， ntpd 服务必须是关闭的， 否则会报the NTP socket is in use, exiting 错误。
关闭 ntpd 服务命令如下：

```shell
[root@node2 init.d]# /etc/init.d/ntpd stop
Shutting down ntpd:                                        [  OK  ]
```
1.2. 架设本地时间服务器
需要修改 /etc/ntp.conf文件里的几个配置就可以了，比如本地时间服务器IP 为 10.85.10.119， 配置如下：

```
server 210.72.145.44 prefer （中国国家授时中心服务器地址 prefer表示优先 注意把默认的server更改成这样）
server 127.127.1.0 （本地时间)
restrict 10.85.10.0 mask 255.255.255.0 nomodify (允许10..85.10.* 的IP 使用该时间服务器)
restrict 0.0.0.0 mask 0.0.0.0 nomodify notrap noquery notrust （屏蔽其他IP过来更新时间）
```
其他的保持默认不动。

使NTP服务可以在系统引导的时候自动启动，执行：

```
# chkconfig ntpd on
```
启动/关闭/重启NTP的命令：

```
# /etc/init.d/ntpd start
# /etc/init.d/ntpd stop
# /etc/init.d/ntpd restart
#service ntpd restart
```
将同步好的时间写到CMOS里

```
vi /etc/sysconfig/ntpd
SYNC_HWCLOCK=yes
```

每次修改了配置文件后都需要重新启动服务来使配置生效。
可以使用下面的命令来检查NTP服务是否启动，你应该可以得到一个进程ID号：
`# pgrep ntpd`
使用下面的命令检查时间服务器同步的状态：
`# ntpq -p`
用ntpstat 也可以查看一些同步状态，用netstat -ntlup查看端口使用情况！

安装完毕客户端需过5-10分钟才能从服务器端更新时间！

客户端设置：
`# crontab -e`
加入一行：
`*/1 * * * * ntpdate 10.85.10.119。`
相关配置参数说明

```
#　　restrict权限控制语法为：
#　　restrict IP mask netmask_IP parameter
#　　其中 IP 可以是软件地址，也可以是 default ，default 就类似 0.0.0.0 咯！
#　　至于 paramter 则有：
#　　　ignore　：关闭所有的 NTP 联机服务
#　　　nomodify：表示 Client 端不能更改 Server 端的时间参数，不过，
#　　　　　　　　Client 端仍然可以透过 Server 端来进行网络校时。
#　　　notrust ：该 Client 除非通过认证，否则该 Client 来源将被视为不信任网域
#　　　noquery ：不提供 Client 端的时间查询
#　　如果 paramter 完全没有设定，那就表示该 IP (或网域) 『没有任何限制！』
# 　设定上层主机主要以 server这个参数来设定，语法为：
#　　server [IP|FQDN] [prefer]
#　　Server 后面接的就是我们上层 Time Server 啰！而如果 Server 参数
#　　后面加上 perfer 的话，那表示我们的 NTP 主机主要以该部主机来作为
#　　时间校正的对应。另外，为了解决更新时间封包的传送延迟动作
```

二、使用rdate同步时间
如果要用vmware安装RAC，则各个几点间时间必须一致，可以以一个节点作为标准，其他节点与该节点进行时间同步。
假如有两个节点：
A: 10.85.10.119
B: 10.85.10.121
以A作为时间标准，B节点用A节点时间进行同步。
1、在A节点开放37端口
最简单，但也最不安全的方法是关闭防火墙：`iptables -F`
2. 在A节点启动时间服务
`#chkconfig time on `    #在系统引导的时候自动启动

如果不启动该服务，则其他节点与该节点同步时间时会报错：Connect Refused
注意：要用root 用户
3、在B节点与A节点同步时间
`rdate -s 10.85.10.119`
可以在crontab 中做执行计划， 每分钟执行一次，这样保证时间的同步。

```
[root@node2 ~]# crontab -l
*/1 * * * * rdate -s 10.85.10.119
```
关于crontab 的介绍参考blog：
[Unix crontab 命令详解][http://blog.csdn.net/tianlesoftware/archive/2010/02/21/5315039.aspx]

##### 三、使用 Network Time Protocol (NTP) 服务器
1. 假如公司网络里有一个时间服务器: `10.85.10.80`, 此时只需要在每个结点上修改NTP 服务配置文件，让每个结点和时间服务器进行同步即可。

```shell
$# vi /etc/ntp.conf
Server 10.85.10.80 prefer
Driftfile /var/lib/ntp/drift
Broadcastdelay 0.008
```
修改完后在重启一下 ntp 服务

```
#/etc/init.d/ntpd restart
```
2. 如果没有时间服务，则可以用RAC 2个结点中一个做为服务器。另一个与此服务器同步即可。
加入用node1做服务器， 其IP 为：10.85.10.119，修改配置文件

```
#vi /etc/ntp.conf
Server 127.127.1.0  -- 本地时钟
Fudge 127.127.1.0 stratum 11
Broadcastdelay 0.008
```
Node2 与node1 同步。

修改node2的ntp 配置文件

```
# vi /etc/ntp.conf
Server 10.85.10.119 prefer
Driftfile /var/lib/ntp/drift
Broadcastdelay 0.008
```
修改完后在重启一下 ntp 服务
`$#/etc/init.d/ntpd restart`

或者在node2是使用crontab 与服务器同步时间
`*/15 * * * * ntpdate 10.85.10.119`


----------------------------------------------------------------------华丽丽的分隔线---------------------------------------------------------------------
#### 还是Linux时间同步设置

校准时间的东西网上一大堆,乱78糟的没几个是看一下就明白的

我目前用的方法很简单,写在这里省的自己以后忘了

1. 首先要安ntp

检查是否安装NTP服务

`rpm -aq | grep ntp`

如果没有，则安装：

`yum install ntp`

运行NTP前要配置好时区,centos或者rhel系的用户用setup命令进图形界面自己设一下时区设为`shanghai`就OK了,中国用户一般都要用上海作为自己的时区之后可以更新自己的时间了,有2个方法:

1. 一个是启动`NTPD`这个`daemon`,这个服务会根据`/etc/ntp.conf` 里的配置信息自己去更新时间,不过我不喜欢这种方法,总开着一个进程浪费内存不是很爽,我用第2个方法

2. 再一个方法是用`ntpdate`命令去更新时间,非常简单好用

`ntpdate time.windows.com`

就OK了,有人会说为什么这里有WINDOWS, 其实`time.windows.com`是微软的NTP时间校准服务器地址,不想用微软的就打开自己的WIN7或者XP双击右下角时间,再点internet时间看一下里面服务器的地址 就行了,除了`time.windows.com`还有一个是

`time.nist.gov`  (这个不是微软的,不喜欢微软的人用这个就行了)

当然还有别的NTP服务器,只不过地址不好找,不如双击桌面右下角时间看自己的WINDOWS的校准服务器地址方便,现在还需要做一步,就是让系统时间和BIOS时间同步,用hwclock命令就行了

命令行里输入:

`hwclock --systohc`

或者这个命令的简写：

`hwclock -w`

就可以让BIOS时间和KERNEL时间一样了

----------------------------------------------------------------------华丽丽的分隔线---------------------------------------------------------------------
awk分行打印各个字段
---------------
awk -F'|' '{for (i=1;i<=NF;i++){ print i,$i}}'
---------------
批量重命名文件

---------------

```
FILE=`find . -name "*.gz"`;total=`wc -l $FILE`;I=0;for F in $FILE; do dir=`dirname $F`;echo dealing $I Total:$total; mv $F $dir/wap_gd_cdr_file_`date +%Y%m%d%H%M%S`_`printf "%08d%d" $I $((($I%6+1)))`.bcp.gz;((I+=1));done
```
时间，文件夹，文件名，生成日期，序号，节点号

```
FILE=`ls *.gz`;I=0;PRE=$(date +%Y%m%d);DIR=$(basename `pwd`);PRE=${PRE}${DIR}_wap_gd_cdr_file;for F in $FILE; do mv $F ${PRE}_`date +%Y%m%d%H%M%S`_`printf "%08d%d" $I $((($I%6+1)))`.gz;((I+=1));done
```
文件名，生成日期，序号，节点号

```
FILE=`ls *.gz`;I=0;PRE=wap_gd_cdr_file;for F in $FILE; do mv $F ${PRE}_$(date +%Y%m%d%H%M%S)_$(printf "%08d_%d" $I $((($I%6+1)))).bcp.gz;((I++));done
```

---------------

```shell
HOUR="01 02 03 04 05 06 07 08 09 10 11 12 23 15 15 16 17 18 19 20 21 22 23"
for ONE in $HOUR; do
cd $ONE
FILE=`ls *.gz`;
I=0;
PRE=$(date +%Y%m%d);
DIR=$(basename `pwd`);
PRE=${PRE}${DIR}_wap_gd_cdr_file;
for F in $FILE; do
mv $F ${PRE}_`date +%Y%m%d%H%M%S`_`printf "%08d_%d" $I $((($I%6+1)))`.gz;
(($I+=1));
done
cd ..
done
```

---------------

```
FILE=`find . -name "*.gz"`;I=0;for F in $FILE; do dir=`dirname $F`; mv $F $dir/`date +%Y%m%d%``basename $dir`_wap_gd_cdr_file_`date +%Y%m%d%H%M%S`_`printf "%08d_%d" $I $((($I%6+1)))`.bcp.gz;((I+=1));done
```
---------------
批量分割文件

```
FILE=`ls *.bcp`; I=0;for F in $FILE;do mv $F 100M-file/;cd 100M-file/;split -l 220000 $F file_$I;((I++));mv $F split-done;cd ..;done;
FILE=`ls *.bcp`; I=0;for F in $FILE;do mv $F tmp/;cd tmp/;split -l 220000 $F file_$I;((I++));mv $F ../split-done;cd ..;done;
```
统计大小

```
`ll *1.bcp | awk '{a+=$5} END {print a/1024/1024"MB"}'`
```
统计时间

```
awk '{if(NR==5)a=$4;}END{ print a,$4;}' /home/rainstor/install/local/logs/rs_watcher.log
```
统计时间并计算时间差

```awk
awk '{if(NR==5)a=$4;}END{ta=gensub("[:]"," ","g",a);t4=gensub("[:]"," ","g",$4);
suff=mktime(sprintf("2013 02 19 %s",t4))-mktime(sprintf("2013 02 19 %s",ta));
print a,$4,suff;}' /home/rainstor/install/local/logs/rs_watcher.log
```
----------------------------------------------------------------------华丽丽的分隔线---------------------------------------------------------------------
#### Shell并发编程

```shell
#!/bin/bash

c=0
for i in {1..100}; do
        while [ $c -ge 10 ]; do
                c=$(jobs -p | wc -w)
                sleep 1
        done
        sleep $i && echo $i &
        c=$(jobs -p | wc -w)
done
```
以上是示例，sleep $i 就是你要启动的子进城

```shell
#!/bin/bash

c=0
for i in {1..100}; do
        sleep $i &
        let c++
        if [ $c -eq 10 ]; then
                wait
                c=0
        fi
done
```

这个写法虽然更简单，但是要等10个子进程全部退出，才再启10个, 推荐第一种写法。

----------------------------------------------------------------------华丽丽的分隔线---------------------------------------------------------------------

`wget -r -np -nd --accept=rpm <URL?`
`wget  -c -m -np`

获取当前时间秒，精确到毫秒数
`date +%s.%N | cut -c 1-14`

```awk
awk 'BEGIN{FS=OFS="|"} {printf $1,$2;print $2 | "egrep -o \"\\w+\\.\\w+\$\""; print $2 | "egrep -o \\"\\w+\\.\\w+\\.\\w+\$\"";}' host_with_sort.txt
```
----------------------------------------------------------------------华丽丽的分隔线---------------------------------------------------------------------
####  wget下载JDK

```shell
wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F" "http://download.oracle.com/otn-pub/java/jdk/6u45-b06/jdk-6u45-linux-x64.bin" -O jdk-6u45-linux-x64.bin
```

需要修改输出文件名

```shell
# Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F 可以绕过ORACLE的网站验证
http://download.oracle.com/otn/java/jdk/6u45-b06/jdk-6u45-linux-x64.bin

rsync -azv jdk1.7.0_55/ node3:/usr/lib/jdk/jdk1.7.0_55/
# rsync不能建多层目录，所以需要同步上层目录文件夹

sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jdk/jdk1.7.0_55/bin/javac 300
sudo update-alternatives --install /usr/bin/java java /usr/lib/jdk/jdk1.7.0_55/bin/java 300
#300 是优先级
```

####  rpm安装多版本java

```
sudo rpm -ivh jdk-7u4-linux-x64.rpm --force
sudo update-alternatives --install /usr/bin/java java /usr/java/jdk1.6.0_31/bin/java 300
sudo update-alternatives --install /usr/bin/javac javac /usr/java/jdk1.6.0_31/bin/javac 300
sudo update-alternatives --config java
sudo update-alternatives --config javac
```

####  修改JAVA_HOME无效，java版本保持不变的问题解决
配置`JAVA_HOME`和`PATH`之后，怎么运行都无效，JAVA_HOME显示已经配置好了，但是`java -version`命令的版本结果还是不会变。
之前的配置：

```
export JAVA_HOME=/usr/java/jdk1.7.0_04
export CLASSPATH=.:$JAVA_HOME/lib/jre/dt.jar
export PATH=$PATH:$JAVA_HOME/bin
```
在网上了解到环境变量是有优先级的，如果在前面的环境变量里面找到了对应的程序，就不会继续去找后面的路径。
所以，需要把环境变量设置在前面

```
export JAVA_HOME=/usr/java/jdk1.7.0_04
export CLASSPATH=.:$JAVA_HOME/lib/jre/dt.jar
export PATH=$JAVA_HOME/bin:$PATH
```


在现公司，遇到一个问题，就是配置JAVA_HOME无效，不管怎么改，运行java -version始终是最初的那个java版本。直接在PATH环境变量里追加写死的java路径也没用。

解决过程：
曾经在一个人机器上发现此问题，然后又在两个机器上发现同样的问题，于是我迷茫了。
接着冷静下来想想，在以前的地方从未遇到过这种情况，在现公司三个机器都遇到同样的情况，那么，很有可能就是因为现公司的系统的环境问题，或许是因为大家都装了某个软件引起的。
然后突然想到，难道是在系统目录里面有java.exe？导致优先调用了系统目录中的java.exe，而不是自己配置的JAVA_HOME中的java.exe？
立马来到C:\WINDOWS\system32目录下进行验证。果然，java.exe、javac.exe等exe程序华丽丽地躺在那里！
尼玛，哪个牛掰软件啊！居然把整个JDK安装到system32目录下面了。

解决途径：
接下来问题就简单了，修改环境变量即可解决。
因为PATH环境变量中默认将system32等系统重要目录添加在最前面，所以运行java -version时当然是调用system32目录下的java.exe了。所以只要将%JAVA_HOME%/bin这一句放到PATH环境变量的最前面，问题就迎刃而解了。

[links][http://yunzhu.iteye.com/blog/1551433]

----------------------------------------------------------------------华丽丽的分隔线---------------------------------------------------------------------
#### # （总结）Linux下使用rsync最快速删除海量文件的方法

昨天遇到了要在Linux下删除海量文件的情况，需要删除数十万个文件。这个是之前的程序写的日志，增长很快，而且没什么用。这个时候，我们常用的删除命令rm -fr * 就不好用了，因为要等待的时间太长。所以必须要采取一些非常手段。我们可以使用rsync来实现快速删除大量文件。
1、先安装rsync：
yum install rsync
2、建立一个空的文件夹：
mkdir /tmp/test
3、用rsync删除目标目录：
rsync --delete-before -a -H -v --progress --stats /tmp/test/ log/
这样我们要删除的log目录就会被清空了，删除的速度会非常快。rsync实际上用的是替换原理，处理数十万个文件也是秒删。

选项说明：
–delete-before 接收者在传输之前进行删除操作
–progress 在传输时显示传输过程
-a 归档模式，表示以递归方式传输文件，并保持所有文件属性
-H 保持硬连接的文件
-v 详细输出模式
–stats 给出某些文件的传输状态
----------------------------------------------------------------------华丽丽的分隔线---------------------------------------------------------------------

[links][http://www.ha97.com/4107.html]
