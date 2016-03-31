---
title: CentOS一些常用命令
date: 2016-03-30 11:09:25
categories: Linux
tags:
  - Linux
  - CentOS
---

### 一：查看cpu信息
more /proc/cpuinfo | grep "model name"  
grep "model name" /proc/cpuinfo  
grep "CPU" /proc/cpuinfo
grep "model name" /proc/cpuinfo | cut -f2 -d:
### 二：查看内存信息
grep MemTotal /proc/meminfo

grep MemTotal /proc/meminfo | cut -f2 -d:

grep MemTotal /proc/meminfo | free -m

grep MemTotal /proc/meminfo | grep "Mem"

grep MemTotal /proc/meminfo | awk '{print $2}'
### 三：查看cpu是32位还是64位
getconf LONG_BIT
### 四：查看当前linux的版本信息
cat /etc/issue  #查看具体操作系统类型

more /etc/redhat-release

cat /etc/redhat-release

rpm -q centos-release
### 五：查看内核版本
uname -r

uname -a
### 六：查看系统当前时间
date

clock

clock -w    #同步系统时间
### 七：查看硬盘和分区
df -h

fdisk -l

du -sh   #查看当前目录占用空间大小

du /etc -sh   #查看/etc目录占用空间大小
### 八：查看系统安装的软件包
cat -n /root/install.log    #查看系统默认安装时的软件包

more /root/install.log | wc -l      #查看当前系统安装的软件包数量

rpm -qa

rpm -qa | wc -l

yum list installed | wc -l
### 九：查看键盘布局
cat /etc/sysconfig/keyboard

cat /etc/sysconfig/keyboard | grep KEYTABLE | cut -f2 -d=
### 十：查看selinux是否关闭
sestatus

sestatus | cut -f2 -d:

cat /etc/sysconfig/selinux

### 十一：查看IP地址、网卡mac地址信息
ifconfig

cat /etc/sysconfig/network-scripts/ifcfg-eth0 | grep IPADDR

cat /etc/sysconfig/network-scripts/ifcfg-eth0 | cut -f2 -d=

cat /etc/sysconfig/network-scripts/ifcfg-eth0 | ifconfig eth0

cat /etc/sysconfig/network-scripts/ifcfg-eth0 | grep -v '127.0.0.1'

cat /etc/sysconfig/network-scripts/ifcfg-eth0 | cut -d: -f2

cat /etc/sysconfig/network-scripts/ifcfg-eth0 | awk '{ print $1}'

cat /etc/sysconfig/network  

cat /etc/resolv.conf   #查看DNS
### 十二：查看系统默认语言
echo $LANG $LANGUAGE

cat /etc/sysconfig/i18n
### 十二：查看所属时区和是否使用UTC时间
cat /etc/sysconfig/clock
### 十三：查看主机名
hostname

cat /etc/sysconfig/network
### 十四：查看系统开机运行时间
uptime

vmstat 1 -S m  procs

其他的命令请查看这里：
[初窥Linux 之 我最常用的20条命令 ](http://blog.csdn.net/ljianhui/article/details/11100625)
