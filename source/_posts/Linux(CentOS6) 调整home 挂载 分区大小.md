title: Linux 如何查看目录的剩余空间大小？
categories: Linux
tags:
  - Linux
  - CentOS
date: 2016-01-28 14:57:26
---
## linux 如何查看目录的剩余空间大小？

两个命令df 、du结合比较直观
df    -h                     查看整台服务器的硬盘使用情况
cd    /                       进入根目录
du   -sh    *              查看每个文件夹的大小

[Linux查看硬盘使用空间命令df&du](http://www.sugarguo.com/linuxchakanyingpanshiyongkongjianminglingdfdu/)

[linux下df与du查看磁盘剩余空间和文件夹大小](http://www.111cn.net/sys/linux/63833.htm)


/root
    Linux超级权限用户root的家目录。
/home
    如果我们建立一个用户，用户名是"xx",那么在/home目录下就有一个对应的/home/xx路径，用来存放用户的主目录。

root是管理员账号，root文件夹是管理员的主目录，它的配置文件还有root的一些别的东西放在这里。而home是给普通用户的，在home下面有用户名对应的文件夹，这些个文件夹就相当于root文件夹，用来存放对应用户的一些资料，配置。

请按照这个来做：
[Linux(CentOS6) 调整 /home 挂载 分区大小](http://www.07net01.com/security/Linux_CentOS6__diaozheng__home_guazai_fenqudaxiao_54712_1358334113.html)
