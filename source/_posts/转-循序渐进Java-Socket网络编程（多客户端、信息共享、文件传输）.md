title: (转)循序渐进Java Socket网络编程（多客户端、信息共享、文件传输）
categories: Java
tags:
  - Java
  - 多线程
date: 2016-03-01 14:57:31
---
前言：在最近一个即将结束的项目中使用到了Socket编程，用于调用另一系统进行处理并返回数据。故把Socket的基础知识总结梳理一遍。

# 一、TCP/IP协议

　　既然是网络编程，涉及几个系统之间的交互，那么首先要考虑的是如何准确的定位到网络上的一台或几台主机，另一个是如何进行可靠高效的数据传输。这里就要使用到TCP/IP协议。

　　TCP/IP协议（传输控制协议）由网络层的IP协议和传输层的TCP协议组成。IP层负责网络主机的定位，数据传输的路由，由IP地址可以唯一的确定Internet上的一台主机。TCP层负责面向应用的可靠的或非可靠的数据传输机制，这是网络编程的主要对象。

# 二、TCP与UDP

　　TCP是一种面向连接的保证可靠传输的协议。通过TCP协议传输，得到的是一个顺序的无差错的数据流。发送方和接收方的成对的两个socket之间必须建立连接，以便在TCP协议的基础上进行通信，当一个socket（通常都是server socket）等待建立连接时，另一个socket可以要求进行连接，一旦这两个socket连接起来，它们就可以进行双向数据传输，双方都可以进行发送或接收操作。

　　UDP是一种面向无连接的协议，每个数据报都是一个独立的信息，包括完整的源地址或目的地址，它在网络上以任何可能的路径传往目的地，因此能否到达目的地，到达目的地的时间以及内容的正确性都是不能被保证的。

## TCP与UDP区别：

### TCP特点：

　　1、TCP是面向连接的协议，通过三次握手建立连接，通讯完成时要拆除连接，由于TCP是面向连接协议，所以只能用于点对点的通讯。而且建立连接也需要消耗时间和开销。

　　2、TCP传输数据无大小限制，进行大数据传输。

　　3、TCP是一个可靠的协议，它能保证接收方能够完整正确地接收到发送方发送的全部数据。

### UDP特点：

　　1、UDP是面向无连接的通讯协议，UDP数据包括目的端口号和源端口号信息，由于通讯不需要连接，所以可以实现广播发送。

　　2、UDP传输数据时有大小限制，每个被传输的数据报必须限定在64KB之内。

　　3、UDP是一个不可靠的协议，发送方所发送的数据报并不一定以相同的次序到达接收方。

## TCP与UDP应用：

　　1、TCP在网络通信上有极强的生命力，例如远程连接（Telnet）和文件传输（FTP）都需要不定长度的数据被可靠地传输。但是可靠的传输是要付出代价的，对数据内容正确性的检验必然占用计算机的处理时间和网络的带宽，因此TCP传输的效率不如UDP高。

　　2，UDP操作简单，而且仅需要较少的监护，因此通常用于局域网高可靠性的分散系统中client/server应用程序。例如视频会议系统，并不要求音频视频数据绝对的正确，只要保证连贯性就可以了，这种情况下显然使用UDP会更合理一些。

# 三、Socket是什么

　　Socket通常也称作"套接字"，用于描述IP地址和端口，是一个通信链的句柄。网络上的两个程序通过一个双向的通讯连接实现数据的交换，这个双向链路的一端称为一个Socket，一个Socket由一个IP地址和一个端口号唯一确定。应用程序通常通过"套接字"向网络发出请求或者应答网络请求。 Socket是TCP/IP协议的一个十分流行的编程界面，但是，Socket所支持的协议种类也不光TCP/IP一种，因此两者之间是没有必然联系的。在Java环境下，Socket编程主要是指基于TCP/IP协议的网络编程。

　　Socket通讯过程：服务端监听某个端口是否有连接请求，客户端向服务端发送连接请求，服务端收到连接请求向客户端发出接收消息，这样一个连接就建立起来了。客户端和服务端都可以相互发送消息与对方进行通讯。

## Socket的基本工作过程包含以下四个步骤：

>　　1、创建Socket；

>　　2、打开连接到Socket的输入输出流；

>　　3、按照一定的协议对Socket进行读写操作；

>　　4、关闭Socket。



# 四、Java中的Socket

　　在java.net包下有两个类：Socket和ServerSocket。ServerSocket用于服务器端，Socket是建立网络连接时使用的。在连接成功时，应用程序两端都会产生一个Socket实例，操作这个实例，完成所需的会话。对于一个网络连接来说，套接字是平等的，并没有差别，不因为在服务器端或在客户端而产生不同级别。不管是Socket还是ServerSocket它们的工作都是通过SocketImpl类及其子类完成的。

### 几个常用的构造方法：
> Socket(InetAddress address,int port);//创建一个流套接字并将其连接到指定 IP 地址的指定端口号
> Socket(String host,int port);//创建一个流套接字并将其连接到指定主机上的指定端口号
> Socket(InetAddress address,int port, InetAddress localAddr,int localPort);//创建一个套接字并将其连接到指定远程地址上的指定远程端口

> Socket(String host,int port, InetAddress localAddr,int localPort);//创建一个套接字并将其连接到指定远程主机上的指定远程端口
> Socket(SocketImpl impl);//使用用户指定的 SocketImpl 创建一个未连接 Socket

> ServerSocket(int port);//创建绑定到特定端口的服务器套接字
> ServerSocket(int port,int backlog);//利用指定的 backlog 创建服务器套接字并将其绑定到指定的本地端口号
> ServerSocket(int port,int backlog, InetAddress bindAddr);//使用指定的端口、侦听 backlog 和要绑定到的本地 IP地址创建服务器


构造方法的参数中，address、host和port分别是双向连接中另一方的IP地址、主机名和端 口号，stream指明socket是流socket还是数据报socket，localPort表示本地主机的端口号，localAddr和bindAddr是本地机器的地址（ServerSocket的主机地址），impl是socket的父类，既可以用来创建serverSocket又可以用来创建Socket。count则表示服务端所能支持的最大连接数。

`注意：`必须小心选择端口号。每一个端口提供一种特定的服务，只有给出正确的端口，才 能获得相应的服务。0~1023的端口号为系统所保留，例如http服务的端口号为80,telnet服务的端口号为21,ftp服务的端口号为23, 所以我们在选择端口号时，最好选择一个大于1023的数以防止发生冲突。

### 几个重要的Socket方法：
> public InputStream getInputStream();//方法获得网络连接输入，同时返回一个IutputStream对象实例
> public OutputStream getOutputStream();//方法连接的另一端将得到输入，同时返回一个OutputStream对象实例
> public Socket accept();//用于产生"阻塞"，直到接受到一个连接，并且返回一个客户端的Socket对象实例。


`阻塞`是一个术语，它使程序运行暂时"停留"在这个地方，直到一个会话产生，然后程序继续；通常``阻塞``是由循环产生的。

注意：其中getInputStream和getOutputStream方法均会产生一个IOException，它必须被捕获，因为它们返回的流对象，通常都会被另一个流对象使用。

# 五、基本的Client/Server程序

以下是一个基本的客户端/服务器端程序代码。主要实现了服务器端一直监听某个端口，等待客户端连接请求。客户端根据IP地址和端口号连接服务器端，从键盘上输入一行信息，发送到服务器端，然后接收服务器端返回的信息，最后结束会话。这个程序一次只能接受一个客户连接。

ps：这个小例子写好后，服务端一直接收不到消息，调试了好长时间，才发现误使用了PrintWriter的print()方法，而BufferedReader的readLine()方法一直没有遇到换行，所以一直等待读取。我晕死~~@_@

客户端程序：

```java
package sock;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;

public class SocketClient {
    public static void main(String[] args) {
        try {
            /** 创建Socket*/
            // 创建一个流套接字并将其连接到指定 IP 地址的指定端口号(本处是本机)
            Socket socket =new Socket("127.0.0.1",2013);
            // 60s超时
            socket.setSoTimeout(60000);

            /** 发送客户端准备传输的信息 */
            // 由Socket对象得到输出流，并构造PrintWriter对象
            PrintWriter printWriter =new PrintWriter(socket.getOutputStream(),true);
            // 将输入读入的字符串输出到Server
            BufferedReader sysBuff =new BufferedReader(new InputStreamReader(System.in));
            printWriter.println(sysBuff.readLine());
            // 刷新输出流，使Server马上收到该字符串
            printWriter.flush();

            /** 用于获取服务端传输来的信息 */
            // 由Socket对象得到输入流，并构造相应的BufferedReader对象
            BufferedReader bufferedReader =new BufferedReader(new InputStreamReader(socket.getInputStream()));
            // 输入读入一字符串
            String result = bufferedReader.readLine();
            System.out.println("Server say : " + result);

            /** 关闭Socket*/
            printWriter.close();
            bufferedReader.close();
            socket.close();
        }catch (Exception e) {
            System.out.println("Exception:" + e);
        }
    }
}
```


服务器端程序：

```java
package sock;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;

public class SocketServer {
    public static void main(String[] args) {
        try {
            /** 创建ServerSocket*/
            // 创建一个ServerSocket在端口2013监听客户请求
            ServerSocket serverSocket =new ServerSocket(2013);
            while (true) {
                // 侦听并接受到此Socket的连接,请求到来则产生一个Socket对象，并继续执行
                Socket socket = serverSocket.accept();

                /** 获取客户端传来的信息 */
                // 由Socket对象得到输入流，并构造相应的BufferedReader对象
                BufferedReader bufferedReader =new BufferedReader(new InputStreamReader(socket.getInputStream()));
                // 获取从客户端读入的字符串
                String result = bufferedReader.readLine();
                System.out.println("Client say : " + result);

                /** 发送服务端准备传输的 */
                // 由Socket对象得到输出流，并构造PrintWriter对象
                PrintWriter printWriter =new PrintWriter(socket.getOutputStream());
                printWriter.print("hello Client, I am Server!");
                printWriter.flush();

                /** 关闭Socket*/
                printWriter.close();
                bufferedReader.close();
                socket.close();
            }
        }catch (Exception e) {
            System.out.println("Exception:" + e);
        }finally{
//          serverSocket.close();
        }
    }
}
```

# 六、多客户端连接服务器

上面的服务器端程序一次只能连接一个客户端，这在实际应用中显然是不可能的。通常的网络环境是多个客户端连接到某个主机进行通讯，所以我们要对上面的程序进行改造。

设计思路：服务器端主程序监听某一个端口，客户端发起连接请求，服务器端主程序接收请求，同时构造一个线程类，用于接管会话。当一个Socket会话产生后，这个会话就会交给线程进行处理，主程序继续进行监听。

下面的实现程序流程是：客户端和服务器建立连接，客户端发送消息，服务端根据消息进行处理并返回消息，若客户端申请关闭，则服务器关闭此连接，双方通讯结束。

客户端程序：

```java
package sock;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;

public class SocketClient {
    public static void main(String[] args) {
        try {
            Socket socket =new Socket("127.0.0.1",2013);
            socket.setSoTimeout(60000);

            PrintWriter printWriter =new PrintWriter(socket.getOutputStream(),true);
            BufferedReader bufferedReader =new BufferedReader(new InputStreamReader(socket.getInputStream()));

            String result ="";
<<<<<<< HEAD
            //当读取到服务器端返回的信息没有“bye”，则继续发送内容给服务端
=======
>>>>>>> ac08ee1f28d47f6b8673c9a0d27b15a38fae66be
            while(result.indexOf("bye") == -1){
                BufferedReader sysBuff =new BufferedReader(new InputStreamReader(System.in));
                printWriter.println(sysBuff.readLine());
                printWriter.flush();

                result = bufferedReader.readLine();
                System.out.println("Server say : " + result);
            }

            printWriter.close();
            bufferedReader.close();
            socket.close();
        }catch (Exception e) {
            System.out.println("Exception:" + e);
        }
    }
}
```

服务器端程序：
```java
package sock;
import java.io.*;
import java.net.*;

public class Server extends ServerSocket {
    private static final int SERVER_PORT =2013;

    public Server()throws IOException {
        super(SERVER_PORT);

        try {
            while (true) {
                Socket socket = accept();
<<<<<<< HEAD
                new CreateServerThread(socket);//当有请求时，启动一个线程处理
=======
                new CreateServerThread(socket);//当有请求时，启一个线程处理
>>>>>>> ac08ee1f28d47f6b8673c9a0d27b15a38fae66be
            }
        }catch (IOException e) {
        }finally {
            close();
        }
    }

    //线程类
    class CreateServerThread extends Thread {
        private Socket client;
        private BufferedReader bufferedReader;
        private PrintWriter printWriter;

<<<<<<< HEAD
        //构造函数
=======
>>>>>>> ac08ee1f28d47f6b8673c9a0d27b15a38fae66be
        public CreateServerThread(Socket s)throws IOException {
            client = s;

            bufferedReader =new BufferedReader(new InputStreamReader(client.getInputStream()));

            printWriter =new PrintWriter(client.getOutputStream(),true);
<<<<<<< HEAD
            System.out.println("Client(" + getName() +") come in...");//getName()是获取线程的名称，省略了

            //启动线程
=======
            System.out.println("Client(" + getName() +") come in...");

>>>>>>> ac08ee1f28d47f6b8673c9a0d27b15a38fae66be
            start();
        }

        public void run() {
            try {
                String line = bufferedReader.readLine();

<<<<<<< HEAD
                //如果读取到的信息不是bye，继续读取
=======
>>>>>>> ac08ee1f28d47f6b8673c9a0d27b15a38fae66be
                while (!line.equals("bye")) {
                    printWriter.println("continue, Client(" + getName() +")!");
                    line = bufferedReader.readLine();
                    System.out.println("Client(" + getName() +") say: " + line);
                }
<<<<<<< HEAD
                //如果接受到客户端发来的“bye”，返回信息给客户端
=======
>>>>>>> ac08ee1f28d47f6b8673c9a0d27b15a38fae66be
                printWriter.println("bye, Client(" + getName() +")!");

                System.out.println("Client(" + getName() +") exit!");
                printWriter.close();
                bufferedReader.close();
                client.close();
            }catch (IOException e) {
            }
        }
    }

    public static void main(String[] args)throws IOException {
        new Server();
    }
}
```

# 七、信息共享

以上虽然实现了多个客户端和服务器连接，但是仍然是消息在一个客户端和服务器之间相互传播。现在我们要实现信息共享，即服务器可以向多个客户端发送广播消息，客户端也可以向其他客户端发送消息。类似于聊天室的那种功能，实现信息能在多个客户端之间共享。

设计思路：客户端循环可以不停输入向服务器发送消息，并且启一个线程，专门用来监听服务器端发来的消息并打印输出。服务器端启动时，启动一个监听何时需要向客户端发送消息的线程。每次接受客户端连接请求，都启一个线程进行处理，并且将客户端信息存放到公共集合中。当客户端发送消息时，服务器端将消息顺序存入队列中，当需要输出时，从队列中取出广播到各客户端处。客户端输入showuser命令可以查看在线用户列表，输入bye向服务器端申请退出连接。

PS：以下代码在测试时发现了一个中文乱码小问题，当文件设置UTF-8编码时，无论怎样在代码中设置输入流编码都不起作用，输入中文仍然会乱码。把文件设置为GBK编码后，不用在代码中设置输入流编码都能正常显示传输中文。

客户端代码：
```java
package sock;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;

public class SocketClient extends Socket{

    private static final String SERVER_IP ="127.0.0.1";
    private static final int SERVER_PORT =2013;

    private Socket client;
    private PrintWriter out;
    private BufferedReader in;

    /**
     * 与服务器连接，并输入发送消息
     */
    public SocketClient()throws Exception{
        super(SERVER_IP, SERVER_PORT);
        client =this;
        out =new PrintWriter(this.getOutputStream(),true);
        in =new BufferedReader(new InputStreamReader(this.getInputStream()));
        new readLineThread();

        while(true){
            in =new BufferedReader(new InputStreamReader(System.in));
            String input = in.readLine();
            out.println(input);
        }
    }

    /**
     * 用于监听服务器端向客户端发送消息线程类
     */
    class readLineThread extends Thread{

        private BufferedReader buff;
        public readLineThread(){
            try {
                buff =new BufferedReader(new InputStreamReader(client.getInputStream()));
                start();
            }catch (Exception e) {
            }
        }

        @Override
        public void run() {
            try {
                while(true){
                    String result = buff.readLine();
                    if("byeClient".equals(result)){//客户端申请退出，服务端返回确认退出
                        break;
                    }else{//输出服务端发送消息
                        System.out.println(result);
                    }
                }
                in.close();
                out.close();
                client.close();
            }catch (Exception e) {
            }
        }
    }

    public static void main(String[] args) {
        try {
            new SocketClient();//启动客户端
        }catch (Exception e) {
        }
    }
}
```

服务器端代码：
```java
package sock;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;


public class Server extends ServerSocket{

    private static final int SERVER_PORT =2013;

    private static boolean isPrint =false;//是否输出消息标志
    private static List user_list =new ArrayList();//登录用户集合
    private static List<ServerThread> thread_list =new ArrayList<ServerThread>();//服务器已启用线程集合
    private static LinkedList<String> message_list =new LinkedList<String>();//存放消息队列

    /**
     * 创建服务端Socket,创建向客户端发送消息线程,监听客户端请求并处理
     */
    public Server()throws IOException{
        super(SERVER_PORT);//创建ServerSocket
        new PrintOutThread();//创建向客户端发送消息线程

        try {
            while(true){//监听客户端请求，启个线程处理
                Socket socket = accept();
                new ServerThread(socket);
            }
        }catch (Exception e) {
        }finally{
            close();
        }
    }

    /**
     * 监听是否有输出消息请求线程类,向客户端发送消息
     */
    class PrintOutThread extends Thread{

        public PrintOutThread(){
            start();
        }

        @Override
        public void run() {
            while(true){
                if(isPrint){//将缓存在队列中的消息按顺序发送到各客户端，并从队列中清除。
                    String message = message_list.getFirst();
                    for (ServerThread thread : thread_list) {
                        thread.sendMessage(message);
                    }
                    message_list.removeFirst();
                    isPrint = message_list.size() >0 ?true :false;
                }
            }
        }
    }

    /**
     * 服务器线程类
     */
    class ServerThread extends Thread{
        private Socket client;
        private PrintWriter out;
        private BufferedReader in;
        private String name;

        public ServerThread(Socket s)throws IOException{
            client = s;
            out =new PrintWriter(client.getOutputStream(),true);
            in =new BufferedReader(new InputStreamReader(client.getInputStream()));
            in.readLine();
            out.println("成功连上聊天室,请输入你的名字：");
            start();
        }

        @Override
        public void run() {
            try {
                int flag =0;
                String line = in.readLine();
                while(!"bye".equals(line)){
                    //查看在线用户列表
                    if ("showuser".equals(line)) {
                        out.println(this.listOnlineUsers());
                        line = in.readLine();
                    }
                    //第一次进入，保存名字
                    if(flag++ ==0){
                        name = line;
                        user_list.add(name);
                        thread_list.add(this);
                        out.println(name +"你好,可以开始聊天了...");
                        this.pushMessage("Client<" + name +">进入聊天室...");
                    }else{
                        this.pushMessage("Client<" + name +"> say : " + line);
                    }
                    line = in.readLine();
                }
                out.println("byeClient");
            }catch (Exception e) {
                e.printStackTrace();
            }finally{//用户退出聊天室
                try {
                    client.close();
                }catch (IOException e) {
                    e.printStackTrace();
                }
                thread_list.remove(this);
                user_list.remove(name);
                pushMessage("Client<" + name +">退出了聊天室");
            }
        }

        //放入消息队列末尾，准备发送给客户端
        private void pushMessage(String msg){
            message_list.addLast(msg);
            isPrint =true;
        }

        //向客户端发送一条消息
        private void sendMessage(String msg){
            out.println(msg);
        }

        //统计在线用户列表
        private String listOnlineUsers() {
            String s ="--- 在线用户列表 ---\015\012";
            for (int i =0; i < user_list.size(); i++) {
                s +="[" + user_list.get(i) +"]\015\012";
            }
            s +="--------------------";
            return s;
        }
    }

    public static void main(String[] args)throws IOException {
        new Server();//启动服务端
    }
}
```

# 八、文件传输
客户端向服务器端传送文件，服务端可获取文件名用于保存，获取文件大小计算传输进度，比较简单，直接贴代码。

客户端代码：
```java
package sock;

import java.io.DataOutputStream;
import java.io.File;
import java.io.FileInputStream;
import java.net.Socket;

/**
 * 客户端
 */
public class Client extends Socket{

    private static final String SERVER_IP ="127.0.0.1";
    private static final int SERVER_PORT =2013;

    private Socket client;
    private FileInputStream fis;
    private DataOutputStream dos;

    public Client(){
        try {
            try {
                client =new Socket(SERVER_IP, SERVER_PORT);
                //向服务端传送文件
                File file =new File("c:/test.doc");
                fis =new FileInputStream(file);
                dos =new DataOutputStream(client.getOutputStream());

                //文件名和长度
                dos.writeUTF(file.getName());
                dos.flush();
                dos.writeLong(file.length());
                dos.flush();

                //传输文件
                byte[] sendBytes =new byte[1024];
                int length =0;
                while((length = fis.read(sendBytes,0, sendBytes.length)) >0){
                    dos.write(sendBytes,0, length);
                    dos.flush();
                }
            }catch (Exception e) {
                e.printStackTrace();
            }finally{
                if(fis !=null)
                    fis.close();
                if(dos !=null)
                    dos.close();
                client.close();
            }
        }catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static void main(String[] args)throws Exception {
        new Client();
    }
}
```

服务器端代码：
```java
package sock;
import java.io.DataInputStream;
import java.io.File;
import java.io.FileOutputStream;
import java.net.ServerSocket;
import java.net.Socket;

/**
 * 服务器
 */
public class Server extends ServerSocket{

    private static final int PORT =2013;

    private ServerSocket server;
    private Socket client;
    private DataInputStream dis;
    private FileOutputStream fos;

    public Server()throws Exception{
        try {
            try {
                server =new ServerSocket(PORT);

                while(true){
                    client = server.accept();

                    dis =new DataInputStream(client.getInputStream());
                    //文件名和长度
                    String fileName = dis.readUTF();
                    long fileLength = dis.readLong();
                    fos =new FileOutputStream(new File("d:/" + fileName));

                    byte[] sendBytes =new byte[1024];
                    int transLen =0;
                    System.out.println("----开始接收文件<" + fileName +">,文件大小为<" + fileLength +">----");
                    while(true){
                        int read =0;
                        read = dis.read(sendBytes);
                        if(read == -1)
                            break;
                        transLen += read;
                        System.out.println("接收文件进度" +100 * transLen/fileLength +"%...");
                        fos.write(sendBytes,0, read);
                        fos.flush();
                    }
                    System.out.println("----接收文件<" + fileName +">成功-------");
                    client.close();
                }
            }catch (Exception e) {
                e.printStackTrace();
            }finally {
                if(dis !=null)
                    dis.close();
                if(fos !=null)
                    fos.close();
                server.close();
            }
        }catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static void main(String[] args)throws Exception {
        new Server();
    }
}
```


来源：[循序渐进Java Socket网络编程（多客户端、信息共享、文件传输）](http://my.oschina.net/leejun2005/blog/104955)
