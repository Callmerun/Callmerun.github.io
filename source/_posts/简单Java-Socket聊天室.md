title: 简单Java Socket聊天室
categories: Java
tags:
  - Java
  - 多线程
date: 2016-03-01 15:57:49
---
## 服务器端
 ```java
 import java.io.IOException;
 import java.net.ServerSocket;
 import java.net.Socket;

 /**
  * tcp聊天室的设计思路：
  * 需求：
  * 1.可以多个用户并发访问服务器
  *
  *
  * 思路：
  * 1.服务器端要可以开启多线程
  * 2.客户端同时多个在线
  *
  * 步骤：
  * 1.创建一个线程类来处理连接到服务器的客户
  * */
 public class TcpServer {

 	/**
 	 * @param args
 	 * @throws IOException
 	 */
 	public static void main(String[] args) throws IOException {
 		// TODO Auto-generated method stub

 		//服务器Socket
 		ServerSocket ss = new ServerSocket(8888);

 		//获取客户端
 		while(true){
 			System.out.println("等待客户端");
 			//响应客服端的连接，方式是阻塞式，将一直等待别人的连接
 			Socket client = ss.accept();
 			System.out.println("连接成功" + client.toString());

 			//每当客户端连接后启动一条TaskThread线程为该客户端服务
 			new Thread(new TaskThread(client)).start();
 		}
 	}
 }
 ```

## 处理对话线程类
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;

/**
 * 负责处理每个线程通信的线程类
 * */
public class TaskThread implements Runnable{
	private Socket client;

	TaskThread(Socket client) {
		this.client = client;
	}

	//每接受到一 个客户端都要做的事情
	public void run() {
		// TODO Auto-generated method stub
		try {
			/***********在服务器端显示客户端的信息**********/
			//获取客户端的IP地址
			String ip = client.getInetAddress().getHostAddress();
			System.out.println(ip + "........connected");

			/**********获取客户端的信息，返回给客户端*********/
			//获取连接上的输入流clientin,和输出流backmessage,并包装
			BufferedReader clientin = new BufferedReader(new InputStreamReader(client.getInputStream()));
			PrintWriter backmessage = new PrintWriter(client.getOutputStream(),true);

			String line;
			//采用循环不断从Socket中读取客户端发送过来的数据
			while((line = clientin.readLine()) != null){
				//String text = new String(buf,0,len);
				backmessage.println(line);
			}

			//关闭获取到的客户端
			client.close();

		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
}
```

## 客户端
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;

public class TcpClient {

	public static void main(String[] args) {
		try {
			// 1.创建Socket，输入服务端主机ip地址
			Socket s = new Socket("192.168.2.110", 8888);

			/******** 准备客户端传输的信息 ************/
			// 2.获取socket对象中的输出流
			PrintWriter bufw = new PrintWriter(s.getOutputStream(), true);

			// 3.准备键盘录入的信息
			BufferedReader bufr = new BufferedReader(new InputStreamReader(System.in));
			BufferedReader message = new BufferedReader(new InputStreamReader(s.getInputStream()));

			// 4.将输入读入的字符串发送到Server
			//char[] buf = new char[1024];
			//int len;
			String line;
			while ((line = bufr.readLine()) != null) {
				if("over".equals(line))
					break;
					//bufw.println(new String(buf,0,len));
				bufw.println(line);

				/******** 用于接受服务端传回的信息 ************/
				// 输入读入一字符串
				String result;
				result = message.readLine();
				System.out.println("服务端: " + result);
			}

			// 5.关闭
			s.close();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
}
```
