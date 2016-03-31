---
title: CentOS服务器初始化设置
date: 2016-03-30 09:14:37
categories: Linux
tags:
  - Linux
  - CentOS
---
## 一、安装：

1.CentOS 6.6文本安装模式不支持自定义分区，建议使用图形安装模式安装；
<font Color=Red>
界面说明：
Install or upgrade an existing system 安装或升级现有的系统
install system with basic video driver 安装过程中采用 基本的显卡驱动
Rescue installed system 进入系统修复模式
Boot from local drive 退出安装从硬盘启动
Memory test 内存检测
</font>

这里选择第一项，安装或升级现有的系统，回车。

出现是否对CD媒体进行测试的提问，这里选择“Skip”跳过测试。

2.<font Color=Red>生产环境必须设置强壮复杂的密码，建议使用密码生成器生成一个不规律的密码</font>

3.分区：自定义分区。

<font Color=Red>注意：分区之前，自己先要规划好，怎么分区</font>

我这里的分区如下：

硬盘总共 25G

/boot  #128M

/   #剩余所有空间

创建流程：

1、选中空闲分区Free，点创建Create

选择标准分区Standard Partition，点创建Create

2、挂载点：/boot

文件系统类型：<font Color=MAGENTA>ext3</font>

大小Size：128

其他选项默认即可

确定 OK

3、继续选中空闲分区Free，点创建Create

挂载点：/

文件系统类型：<font Color=MAGENTA>ext4</font>

选中“使用全部可用空间”

其他选项默认即可

确定 OK

4、创建swap区，也可以在后面在创建

swap  #2048M，一般设置为内存2倍

其他的类似。

创建好分区之后，然后点Next

<font Color=Red>特别说明：</font>

<font Color=MAGENTA>
用于正式生产的服务器，切记必须把数据盘单独分区，防止系统出问题时，保证数据的完整性。比如可以再划分一个/data专门用来存放数据。

这里没有划分swap分区，对于大内存服务器，可以不用设置swap分区，或者在确定系统需要使用的内存大小后，再增加swap</font>


## 二、设置IP地址、网关、DNS

<font Color=Red>约定：</font>

第一块网卡为外网

第二块网卡为内网（没有外网的机器也要将内网配置在第二块网卡上）

<font Color=Red>说明：CentOS 6.5默认安装好之后是没有自动开启网络连接的！</font>

输入账号root

再输入安装过程中设置的密码，登录到系统

service NetworkManager stop   #停用 网络管理器，不然到时可能会引起冲突

chkconfig NetworkManager off  #禁止 网络管理器 开机启动

chkconfig --list  #查看状态

vi  /etc/sysconfig/network-scripts/ifcfg-eth0   #编辑配置文件,添加修改以下内容
```
BOOTPROTO=static   #启用静态IP地址

ONBOOT=yes  #开启自动启用网络连接

IPADDR=192.168.21.129  #设置IP地址

NETMASK=255.255.255.0  #设置子网掩码

GATEWAY=192.168.21.2   #设置网关

DNS1=8.8.8.8 #设置主DNS

DNS2=8.8.4.4 #设置备DNS

IPV6INIT=no  #禁止IPV6
```
:wq!  #保存退出

service ip6tables stop   #停止IPV6服务

chkconfig ip6tables off  #禁止IPV6开机启动

service yum-updatesd stop   #关闭系统自动更新

chkconfig yum-updatesd off  #禁止开启启动

service network restart  #重启网络连接

ifconfig  #查看IP地址


 ## 三、添加SWAP分区
swap分区的用处：swap是当物理内存不够用的时候，把数据放到swap中，所以swap起到了一个虚拟内存的作用，在某种意义上来说也算是加大了内存空间。一般swap分区是在安装系统时设置的，如果安装系统时忘记分swap分区了，那也没事，还有补救的方法。下面就讲讲安装完系统后如何添加swap分区。

场景：装完系统后苦逼的发现没有分SWAP分区，对于生产服务器，这样显然不行的，因此需要添加SWAP分区。

1、首先查看swap大小

#free
```
total      used      free    shared    buffers    cached
Mem:      3922944    158168    3764776          0      6948      37384
-/+ buffers/cache:    113836    3809108
Swap:            0          0          0
```
这里很明显的显示为零

2、使用dd命令创建一个swap分区

#dd if=/dev/zero of=/doiido/swap bs=1024 count=8388608

count的计算公式: count=SIZE*1024 (size以MB为单位）
这样就建立一个/doiido/swap的分区文件，大小为8G

3、格式化新建的分区
#mkswap /doiido/swap

4、把新建的分区变成swap分区
#swapon /doiido/swap

注:关闭SWAP分区命令为：#  swapoff /doiido/swap

5、首先查看swap大小
#free

```
total      used      free    shared    buffers    cached
Mem:      3922944    158168    3764776          0      6948      37384
-/+ buffers/cache:    113836    3809108
Swap:      8388608          0    8388608
```

6、开机自动挂载swap
#echo "/doiido/swap swap swap defaults  0 0" >> /etc/fstab

这样SWAP分区就创建完毕


## 四、系统安全设置

### 1、创建普通账号

useradd osyunwei #创建普通账号

passwd 123456 #设置密码

### 2、禁用root直接登录

vi /etc/ssh/sshd_config #编辑

找到PermitRootLogin，将后面的yes改为no

:wq! #保存退出

### 3、给系文件加锁,防止未经许可的删除或添加

chattr +ia /etc/passwd

chattr +ia /etc/shadow

chattr +ia /etc/group

chattr +ia /etc/gshadow

chattr +ia /etc/services

lsattr  /etc/passwd  /etc/shadow  /etc/group  /etc/gshadow  /etc/services  #显示文件的属性

<font Color=Red>注意：执行以上权限修改之后，就无法添加删除用户了。</font>
<font Color=Red>如果再要添加删除用户，需要先取消上面的设置，等用户添加删除完成之后，再执行上面的操作</font>


chattr -ia /etc/passwd

chattr -ia /etc/shadow

chattr -ia /etc/group

chattr -ia /etc/gshadow

chattr -ia /etc/services

### 4、开启防火墙

yum install iptables #安装防火墙 yum install wget 先安装下载工具

chkconfig iptables on #设置开机启动

vi /etc/sysconfig/iptables #编辑，添加以下代码
```

# Firewall configuration written by system-config-firewall

# Manual customization of this file is not recommended.

*filter

:INPUT ACCEPT [0:0]

:FORWARD ACCEPT [0:0]

:OUTPUT ACCEPT [0:0]

-A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

-A INPUT -p icmp -j ACCEPT

-A INPUT -i lo -j ACCEPT

-A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT

-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT

-A INPUT -s 192.168.1.1/24 -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT

-A INPUT -j REJECT --reject-with icmp-host-prohibited

-A FORWARD -j REJECT --reject-with icmp-host-prohibited
```

COMMIT

#Iptables For OsYunWei.Com Date 2015/05/22

service iptables start #启动防火墙

<font Color=Red>备注：-s 192.168.1.1/24 表示只允许这个ip段访问3306端口，可以根据需求修改<font>

### 5、关闭SELINUX

vi /etc/selinux/config
```

#SELINUX=enforcing #注释掉

#SELINUXTYPE=targeted #注释掉

SELINUX=disabled #增加
```

:wq! #保存退出

setenforce 0 #使配置立即生效

### 6、修改ssh默认端口

把ssh默认远程连接端口22修改为222

vi /etc/ssh/sshd_config

在端口#Port 22下面增加Port 222

:wq! #保存退出

vi /etc/ssh/ssh_config

在端口#Port 22下面增加Port 222

:wq! #保存退出

/etc/init.d/sshd restart #重启sshd服务

vi /etc/sysconfig/iptables #编辑

把22端口修改为222

:wq! #保存退出

/etc/init.d/iptables restart #重启防火墙,使配置生效

### 7、修改主机名称

这里设置主机名为：www.osyunwei.com

1、hostname “www.osyunwei.com” #设置主机名为www.osyunwei.com

2、vi /etc/sysconfig/network #编辑配置文件CentOS 5.x CentOS 6.x

HOSTNAME= www.osyunwei.com #修改localhost.localdomain为www.osyunwei.com

:wq! #保存退出

vi /etc/hostname #编辑配置文件CentOS 7.x

www.osyunwei.com #修改localhost.localdomain为www.osyunwei.com

:wq! #保存退出

3、vi /etc/hosts #编辑配置文件

127.0.0.1 www.osyunwei.com localhost #修改localhost.localdomain为www.osyunwei.com

:wq! #保存退出

### 8、同步系统时间

yum install -y ntp #安装ntp

ntpdate cn.pool.ntp.org #执行时间同步

hwclock --systohc #系统时钟和硬件时钟同步

CentOS 5.x

echo -e "0 0 * * * <font Color=Red>/sbin/ntpdate</font> cn.pool.ntp.org > /dev/null" >> /var/spool/cron/root #添加计划任务

CentOS 6.x

echo -e "0 0 * * * <font Color=Red>/usr/sbin/ntpdate</font> cn.pool.ntp.org > /dev/null" >> /var/spool/cron/root #添加计划任务

service crond restart #重启服务

### 9、安装基础软件包

yum install -y apr* autoconf automake bison cloog-ppl compat* cpp curl curl-devel fontconfig fontconfig-devel freetype freetype* freetype-devel gcc gcc-c++ gtk+-devel gd gettext

gettext-devel glibc kernel kernel-headers keyutils keyutils-libs-devel krb5-devel libcom_err-devel libpng* libjpeg* libsepol-devel libselinux-devel libstdc++-devel libtool*

libgomp libxml2 libxml2-devel libXpm* libtiff libtiff* libX* make mpfr ncurses* ntp openssl openssl-devel patch pcre-devel perl php-common php-gd policycoreutils ppl telnet

t1lib t1lib* nasm nasm* wget zlib-devel

至此，CentOS服务器初始化设置设置完成。

来源：[CentOS服务器初始化设置](http://www.osyunwei.com/archives/9034.html)
