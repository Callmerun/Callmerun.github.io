---
title: Java面试题
date: 2016-03-23 10:50:04
categories: Java
tags:
  - Java
---

1.try {}里有一个return语句，那么紧跟在这个try后的finally {}里的code会不会被执行，什么时候被执行，在return前还是后?

> 根据java规范：在try-catch-finally中，如果try-finally或者catch-finally中都有return，则两个return语句都执行并且最终
返回到调用者那里的是**finally中return的值**；而如果finally中没有return，则理所当然的返回的是try或者catch中return的值，但是
finally中的代码是必须要执行的,而且是在return前执行,除非碰到exit()。

> 但如果return是一个表达式的话，是先要执行这个表达式的。最后return只是说整个函数返回一个东西。然后函数退出。


2.编程题：用最有效率的方法算出2乘以8等於几？

> 最有效率的就要说位的运算了。
1. 2<<3表示把二进制的2左移3为，即2*2^3(2乘以2的三次方)=16；// 这种符合题意
2. 8<<1表示把二进制的8左移1为，即8*2=16；// 这种根本就不是题目的意思。
3. 如果你硬要用8<<1的话，那为什么不用4<<2或者16<<0呢。
4. 根据题目意思当然是2<<3了。
5. 如果题目问：用最有效率的方法算出8乘以2等于几？那么就是：8<<1.


3.switch语句能否作用在byte上，能否作用在long上，能否作用在String上?

> switch语句中的表达式只能是byte，short，char，int以及枚举（enum），所以当表达式是byte的时候可以隐含转换为int类型，
而long字节比int字节多，不能隐式转化为int类型，所以switch语句可以用在byte上而不可以用在long上，
另外由于在JDK7.0中引入了新特性，所以witch语句可以接收一个String类型的值，String可以作用在switch语句上


4.构造方法是否可被重写
> 子类不能继承父类的构造方法，更不能覆盖父类的构造方法。
> 清楚一个概念，所谓继承是：对于类与类而言的，而覆盖是对方法而言的
我们知道子类覆盖父类的方法需要两者的方法完全一致（权限修饰符除外），而且子类方法的权限要高于父类方法的权限；
因此我们知道，子类和父类的类名不同，构造方法就不存在着所谓的覆盖复写，我们只能在子类中调用父类的构造方法来初始化，也必须调用父类的构造函数进行初始化；
