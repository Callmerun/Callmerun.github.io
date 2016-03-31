---
title: Atom软件的一些使用纪要
date: 2016-03-15T11:37:09.000Z
categories: 工具
tags:
  - 工具
---

请看[Atom使用纪要](http://www.cnblogs.com/Darren_code/p/atom.html)

[Github Atom 安装activate power mode炫酷插件超详细教程](http://blog.sina.com.cn/s/blog_8fdfb1d90102w5h4.html)

# 手动安装插件的方法

```
cd ~/.atom/packages
git clone <你想安装的 Package 的仓库链接> # 比如：git clone https://atom.io/packages/emmet
cd <Package 路径> # cd emmet-atom
npm install
```



在这里搜索需要的 [Package](https://atom.io/packages)
点击 `Repo` 得到链接。


> 我安装了 activate-power-mode-master 这个很酷炫的插件。

> 首先 把 node.js和python2.7装好。 注意python最好选2.7版本的，安装好了之后要配置一下环境变量。
比如我的安装在C:\Python27. 在Path的最后加上 ;C:\Python27即可。

> 把你下载好的package解压后放在 C:\Users\aaa\.atom\packages\

> 然后

> cd /d C:\Users\aaa\.atom\packages\activate-power-mode-master

> npm install

> 这就是离线安装了。祝你成功。
