title: CentOS更改yum源与更新系统
categories: Linux
tags:
  - Linux
  - CentOS
date: 2016-01-28 14:57:26
---
# CentOS更改yum源与更新系统
[1] 首先备份/etc/yum.repos.d/CentOS-Base.repo
[root@localhost yum.repos.d]#`mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup`

[2] 进入yum源配置文件所在文件夹
[root@localhost yum.repos.d]#` cd /etc/yum.repos.d/`

[3] 下载163的yum源配置文件，放入/etc/yum.repos.d/(操作前请做好相应备份)

[root@localhost yum.repos.d]#`wget http://mirrors.163.com/.help/CentOS6-Base-163.repo`

[4] 运行yum makecache生成缓存
[root@localhost yum.repos.d]#`yum clean all `
[root@localhost yum.repos.d]#` yum makecache`

[5] 更新系统
[root@localhost yum.repos.d]# `yum -y update`

[6] 安装vim编辑器
[root@localhost ~]#` yum -y install vim*`