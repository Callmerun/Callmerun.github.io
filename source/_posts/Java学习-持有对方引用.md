---
title: 'Java学习:持有对方引用'
date: 2016-03-21 10:53:19
categories: Java
tags:
  - Java
  - 学习
---
**在一个类中访问另外一个类的成员变量，可以利用持有对方的引用来访问。**
其实就是设置一个属性而已,只不过是个对象,所以直接指向引用就可以了,不用再new一个。

例如：

```Java
import java.awt.*;
import java.awt.event.*;

public class TFMath {
	public static void main(String[] args) {
		new TFFrame().launchFrame();
	}
}

class TFFrame extends Frame {
	TextField num1, num2, num3;

	public void launchFrame() {
		num1 = new TextField(10);
		num2 = new TextField(10);
		num3 = new TextField(15);
		Label lblPlus = new Label("+");
		Button btnEqual = new Button("=");
		btnEqual.addActionListener(new MyMonitor(this));
		setLayout(new FlowLayout());
		add(num1);
		add(lblPlus);
		add(num2);
		add(btnEqual);
		add(num3);
		pack();
		setVisible(true);
	}
}

class MyMonitor implements ActionListener {

  /***********这是不持有之前的代码*****************/
	/*
  TextField num1, num2, num3;
	public MyMonitor(TextField num1, TextField num2, TextField num3) {
	    this.num1 = num1;
      this.num2 = num2;
      this.num3 = num3;
  }
	*/

  /*********这是修改之后持有对方引用的代码**********/

	TFFrame tf = null;

	public MyMonitor(TFFrame tf) {
		this.tf = tf;
	}
  /**********************************************/

	public void actionPerformed(ActionEvent e) {
		int n1 = Integer.parseInt(tf.num1.getText());
		int n2 = Integer.parseInt(tf.num2.getText());
		tf.num3.setText("" + (n1 + n2));

	}
}

```
PS：很明显程序修改前要传每一个成员变量，比较不方便，而程序修改后相当于在跟一个持有全部成员变量的大管家在打交道，自然可以也可以跟成员变量打交道，方面了程序。


简单点来说就是下面的样子：
```Java
class A{
    B b;//这儿就叫做持有对方的引用，他持有对B的引用（好处就是当A需要用到对象B的时候可以直接实例化b，然后使用，不需要再传值）
    method_A(){

    }
}

class B{
  method_B(){}
}
```

PS：你可以这样理解，把对方引用想成一个总裁，通常员工在处理重大决策中必须得到总裁的许可，通常总裁不会和员工这种小角色打交道吧，如果你时刻有办法把总裁绑在你身边，你要处理一些重大决策的时候是不是可以直接向总裁汇报了。就是有了总裁（对方引用），员工（持有对方引用的类）的处理效率会大大提高。这也就是持有对方引用的好处。
