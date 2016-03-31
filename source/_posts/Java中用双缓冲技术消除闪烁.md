---
title: Java中用双缓冲技术消除闪烁
date: 2016-03-30 11:19:58
categories: Java
tags:
  - Java
---
在Java编写具有连贯变化的窗口程序时，通常的办法是在子类中覆盖父类的paint(Graphics)方法，在方法中使用GUI函数实现窗口重绘的过程。连贯变换的窗口会不断地调用update(Graphics)函数，该函数自动的调用paint(Graphics)函数。这样就会出现闪烁的情况。

为了解决这一问题，可以应用双缓冲技术。可以通过截取上述过程，覆盖update(Graphics)函数，在内存中创建一个与窗口大小相同的图形，并获得该图形的图形上下文(Graphics)，再将图片的图形上下文作为参数调用paint(Graphics)函数（paint(Graphics)中的GUI函数会在图片上画图），再在update(Graphics)函数调用drawImage函数将创建的图形直接画在窗口上。

```Java
Image ImageBuffer = null;  
Graphics GraImage = null;  

public void update(Graphics g){     //覆盖update方法，截取默认的调用过程  

    ImageBuffer = createImage(this.getWidth(), this.getHeight());   //创建图形缓冲区  

    GraImage = ImageBuffer.getGraphics();       //获取图形缓冲区的图形上下文  

    paint(GraImage);        //用paint方法中编写的绘图过程对图形缓冲区绘图  

    GraImage.dispose();     //释放图形上下文资源  

    g.drawImage(ImageBuffer, 0, 0, this);   //将图形缓冲区绘制到屏幕上  
}  

public void paint(Graphics g){      //在paint方法中实现绘图过程  
    g.drawLine(0, 0, 100, 100);  
}  
```

因为大部分绘图过程是在内存中进行，所以有效地消除了闪烁。这应用了“以空间换取时间”和“功能分块”的思想
