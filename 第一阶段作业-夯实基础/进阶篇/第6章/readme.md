# 第六次章节
## 网路基础知识
### 网络基础知识（1）
- 每个计算设备上行都有若干个网卡
- 每个网卡上都有（全球唯一）单独的硬件地址 MAC地址（Media Access Control Address）

### 网络基础知识（2）
- IP地址：每个网卡/机器都有一个或者多个IP地址
	- IPV4:192.168.0.100 每段从0到255
	- IPV6:128bit长，分成8段 每段4个16进制数以：连接 （四个0也可以以空格表示）

- 查询:Windows平台ipconfig Linux/Mac平台ifconfig

### 网络基础知识（3）
- port: 端口，0-65535
	- 0~1023 OS已经占用了 80是web 23是telnet
	- 1204~65535 一般程序可以使用（为了防止冲突）

- 两台机器通讯就是在IP+Port上进行的
- 在Windows/Linux/Mac上都可以通过netstat-an来查询

### 网络基础知识（4）
- 保留ip: 127.0.0.1本机
- 公网（万维网、互联网）和内网（局域网）
	- 网络是分层的
	- 最外层的是公网、互联网
	- 底下的每层都是内网
	- ip地址可以在每个层次的网重用
	- tracert看当前机器和目标机器的访问中继（相当于要通过多少条路）

### 网络基础知识（5）
- 通讯协议：TCP和UDP
- TCP
	- 传输控制协议 面向**连接**的协议
	- 两台机器的**可靠无差错**的数据传输
	- **双向**字节流传递

- UDP
	- 用户数据报协议 面向**无连接**协议
	- **不保证可靠**的数据传输
	- 速度快 也可以在较差的网络下使用

## UDP 编程
### UDP（１）
-　计算机通讯：数据都是从一个IP的port出发**（发送方）**，运输到另一个IP的port**（接收方）**
- UDP：无连接状态的通讯协议
	- 发送方发送消息 如果接收方刚好在目的地 则可以接收 如果不在 那么这个消息就丢失了
	- 发送方也无法得知是否发送成功
	- UDP的好处就是简单 节省 经济

### UDP（2）
- DatagramSocket：通讯的数据管道
	- send和receive方法
	- （可选 多网卡）绑定一个IP和Port

- DatagramPacket
	- 集装箱：封装数据
	- 地址标签：目的地IP+Port

- 实例
	- 无主次之分
	- 接收方必须早于发送方执行

## TCP网络编程
### TCP（1）
- TCP协议：有连接 保证可靠的无误差通讯
	1. 服务器：创建一个ServerSocket 等待连接
	2. 客户机：创建一个Socket 连接到服务器
	3. 服务器：ServerSocket接收到连接 **创建一个Socket和客户的Socket建立专线连接** 后续服务器和客户机的对话（这一对Socket）会在一个单独的线程（服务器端）上运行
		- 软件服务器有两个要求
			1. 他能够实现一定的功能
			2. 他必须在一个公开地址上对外提供服务 

	4. 服务器的ServerSocket继续等待连接 返回到1.处

### TCP（2）
- 图解 由于无法插入图片 不展示


### TCP（3）
- ServerSocket：服务器码头
	- 需要绑定port
	- 如果有多块网卡 需要绑定一个IP地址
	- 如果不指定IP地址 那么默认就是本机

- Socket：运输通道
	- 客户端需要绑定服务端的地址和Port
	- 客户端向Socket输入流写入数据 送到服务端
	- 客户端从Socket输出流取服务器端过来的数据
	- 服务端反之

- 客户端的输出流就是服务端的输入流

### TCP（4）
- 服务端等待相应的时候**处于阻塞状态**
- 服务端可以**同时相应多个客户端**
- 服务端每次接受到一个客户端 就启动一个独立的线程与之相对应
- 客户端或者服务端都可以选择关闭这对Socket通道

## HTTP编程
### 网页访问
- 网页是特殊的网络服务
	- 在浏览器输入URL地址
	- 浏览器将连接到远程服务器上（IP+80Port）
	- 请求下载一个HTML文件下来 放到本地临时文件夹中
	- 在浏览器上显示出来

### HTTP
- 超文本传输协议
- 用于从WWW服务器传输超文本到本地浏览器的传输协议
- 1990年问世 1997年发布版本1.1 2015年发布版本2.0
- 资源文件采用**HTML**编写 以**URL**形式对外提供

### HTML
- HTML
	- 超文本标记语言
	- [http://www.w3school.com.cn/html/index.asp](http://www.w3school.com.cn/html/index.asp "标准语法")
	- 表单form

- 访问方式
	- **GET：从服务器获取资源到客户端**
	- **POST：从客户端向服务器发送数据**
	- PUT:上传文件
	- DELETE：删除文件
	- HEAD：报文头部
	- OPTIONS：询问支持的方法
	- TRACE：追踪路径
	- CONNECT：用隧道协议连接代理

- Java HTTP编程（java.net包）
	- 支持模拟成浏览器的方式取访问网页
	- URL　代表一个资源　
	- URLConnection
		- 获取资源的连接器
		- 根据URL的openConnection（）方法获得URLConnection
		- connect方法 建立和资源的联系通道
		- getInputStream方法 获取资源的内容

### HTTPClient
- JDK HTTP Client (JDK自带 从9开始)
- Apache HttpComponents的HttpClient(Apache出品)

#### JDK HttpClient
- JDK 9新增 JDK 10更新 JDK　11正式发布
-　java.net.http包
-　取代HTTP/1.1和HTTP/2
-　实现大部分HTTP方法
-　主要类
	-　HttpClient
	-　HttpRequest
	-　HttpResponse

#### HttpComponents
- hc.apache.org Apache出品
- 从HttpClient进化而来
- 是一个集成的Java HTTP工具包
	- 实现所有HTTP方法：get/post/put/delete
	- 支持自动转向
	- 支持https协议
	- 支持代理服务器等

### Code

> 请编写一个群聊的程序，包括服务端程序和客户端程序。 
> 服务端功能：收到某客户端的信息，将消息在控制台输出，然后，发给其他另外的客户端。
> 客户端功能：每隔5秒发送一条信息给服务端。然后接收服务器转发过来的消息，并在控制台输出。

- 服务器

```
import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.Executors;
import java.util.concurrent.ThreadPoolExecutor;

public class TcpServer {
    public static void main(String[] args) throws IOException, InterruptedException {
        ServerSocket serverSocket = new ServerSocket(8080);
        ThreadPoolExecutor executor = (ThreadPoolExecutor) Executors.newFixedThreadPool(10);
        List<Socket> socketList = new ArrayList<>();
        int serverNumber = 0;
        while (true) {
            Thread.sleep(1000);
            Socket socket = serverSocket.accept();
            socketList.add(socket);
            serverNumber++;
            System.out.println("客户端" + serverNumber + "进入了");
            executor.submit(new Client(socket,socketList,serverNumber));
        }
    }
}
```

- 客户端一号


```
import java.io.*;
import java.net.InetAddress;
import java.net.Socket;


public class TcpClient1 {
    public static void main(String[] args) throws IOException, InterruptedException {
        Socket socket = new Socket(InetAddress.getByName("127.0.0.1"), 8080);
        //输入部分
        InputStreamReader inputStreamReader = new InputStreamReader(socket.getInputStream());

        BufferedReader bufferedReader = new BufferedReader(inputStreamReader);


        //输出部分
        OutputStream outputStream = socket.getOutputStream();
        DataOutputStream dataOutputStream = new DataOutputStream(outputStream);
        new Thread(new Receiver(bufferedReader)).start();


        //输入内容 然后输出到服务器
        for (int i = 0; i < 10; i++) {
            Thread.sleep(1000);
            String content = "TcpClient1说: " + i;
            dataOutputStream.write(content.getBytes());
            dataOutputStream.write("\n".getBytes());
        }
        dataOutputStream.writeBytes("quit\n");
        bufferedReader.close();
        socket.close();
        outputStream.close();
        dataOutputStream.close();


    }
}

```

- 客户端二号

```
import java.io.*;
import java.net.InetAddress;
import java.net.Socket;

public class TcpClient2 {
    public static void main(String[] args) throws IOException, InterruptedException {
        Socket socket = new Socket(InetAddress.getByName("127.0.0.1"), 8080);
        //输入部分
        InputStreamReader inputStreamReader = new InputStreamReader(socket.getInputStream());

        BufferedReader bufferedReader = new BufferedReader(inputStreamReader);


        //输出部分
        OutputStream outputStream = socket.getOutputStream();
        DataOutputStream dataOutputStream = new DataOutputStream(outputStream);
        new Thread(new Receiver(bufferedReader)).start();


        //输入内容 然后输出到服务器
        for (int i = 0; i < 10; i++) {
            Thread.sleep(1000);
            String content = "TcpClient2说: " + i;
            dataOutputStream.write(content.getBytes());
            dataOutputStream.write("\n".getBytes());
        }
        dataOutputStream.writeBytes("quit\n");
        bufferedReader.close();
        socket.close();
        outputStream.close();
        dataOutputStream.close();


    }
}

```

- 客户端的消息接收

```
import java.io.BufferedReader;
import java.io.IOException;

public class Receiver implements Runnable {
    BufferedReader bufferedReader;

    public Receiver(BufferedReader bufferedReader) {
        this.bufferedReader = bufferedReader;
    }

    @Override
    public void run() {
        while (true) {
            try {
                String content = bufferedReader.readLine();
                if (content != null)
                    System.out.println(content);
                else
                    break;
            } catch (IOException e) {
                break;
            }
        }
    }
}

```

- 每次来一个客户端 服务端就启动一个socket与之对应

```
import java.io.*;
import java.net.Socket;
import java.util.List;

class Client implements Runnable {
    List<Socket> socketList;
    int serverNumber;
    Socket socket;
    private OutputStream outputStream;
    private DataOutputStream dataOutputStream;

    public Client(Socket socket, List<Socket> socketList, int serverNumber) {
        this.socketList = socketList;
        this.serverNumber = serverNumber;
        this.socket = socket;
    }

    @Override
    public void run() {
        System.out.println(serverNumber + "号成功接入");
        while (true) {
            try {
//                接收来自客户端的消息
                InputStreamReader inputStreamReader = new InputStreamReader(socket.getInputStream());
                BufferedReader bufferedReader = new BufferedReader(inputStreamReader);
                String content = bufferedReader.readLine();
//                在服务器的控制台上输出客户端的消息
                System.out.println(content);

//                将来自客户端的消息转发给各个客户端
                for (Socket socket : socketList) {
                    try {
                        outputStream = socket.getOutputStream();
                        dataOutputStream = new DataOutputStream(outputStream);
                        dataOutputStream.write((content + "\n").getBytes());
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
                if (content.equalsIgnoreCase("quit")) {
                    socketList.remove(this.socket);
                    System.out.println(serverNumber + "退出");
                    if (socketList.size() == 0) {
                        bufferedReader.close();
                        outputStream.close();
                        dataOutputStream.close();
                        break;
                    }
                }
            } catch (IOException e) {
                break;
            }


        }
    }
}
```

# 第六章节（续）
## NIO
### NIO（1）
- Non-Blocking I/O
- 提供非阻塞通讯等方式
- 避免同步I/O通讯效率过低
- 一个线程可以管理多个连接
- 减少线程多的压力

### NIO（2）
- JDK1.4引入 1.7升级NIO 2.0（包括了AIO）
- 主要在java.nio包中
- 主要类
	- Buffer缓存区
	- Channel通道
	- Selector多路选择器

### NIO（3）
- 通讯顺序示意（可以有多个通道（加粗部分））
- Server->ServerSocketChannel->**Selector->Buffer->SocketChannel->SocketChannel->Buffer->Selector->Client**

### NIO（4）
- Buffer缓冲区 一个可以读写的内存区域
	- **ByteBuffer,CharBuffer**,DoubleBuffer,IntBuffer,LongBuffer,ShortBuffer**(StringBuffer不是Buffer缓冲区)**
- Buffer中flip的用法：[https://blog.csdn.net/u013096088/article/details/78638245](https://blog.csdn.net/u013096088/article/details/78638245 "链接")
- 四个主要属性
	- capacity容量
	- position读写位置
	- limit界限
	- mark标记

### NIO（5）
- 全双工的 支持读和写（而Stream流是单向的）
- 支持异步读写 
- 和Buffer配合 提高效率
- **ServerSocketChannel服务器TCPSocket接入通道 接收客户端**
- **SocketChannel TCPSocket通道 可支持阻塞/非阻塞通讯**
- DatagramChannel UDP通道
- FileChannel文件通道
- Selector多路选择器
	- 每隔一段时间 不断轮询注册在其上的Channel
	- 如果有一个Channel有接入、读、写操作 就会被轮询出来
	- 根据SelectionKey可以获取相应的Channel 进行后续的IO操作
	- 避免过多的线程
	- SelectionKey四种类型
		- OP_CONNECT
		- OP_ACCEPT
		- OP_READ
		- OP_WRITE

## AIO
- Asynchronous I/O 异步I/O
- JDK1.7引入 主要在java.nio包中
- 异步I/O 采用回调的方法进行处理读写操作
- 主要类
	- AsynchronousServerSocketChannel 服务器接受请求通道
		- bind 绑定在某一个端口 accept接收客户端请求

	- AsynchronousSocketChannelSocket通讯通道
		- read读数据 write写数据

	- CompletionHanlder
		- completed操作完成之后异步调用方法 failed操作失败之后后异步调用方法

### 三种IO的区别
 &nbsp;|BIO|NIO|AIO
---|---|---|---
阻塞方式|阻塞|非阻塞|非阻塞
同步方式|同步|同步|异步
编程难度|简单|较难|困难
客户机、服务器线程对比|1：1|N：1|N：1
性能|低|高|高

## Netty
### Netty(1)
- Netty,http://netty.io
- 一个非阻塞的客户端-服务端网络通讯框架
- 基于异步事件驱动模型
- 简化了JavaTCP和UDP的编程
- 支持HTTP/2 SSL等多种协议
- 支持多种数据格式　如JSON等

### Netty(2)
- 关键技术
	- 通道Channel
		- ServerSocketChannel/NioServerSocketChannel
		- SocketChannel/NioSocketChannel

	- 事件EventLoop
		- 为每个通道定义一个EventLoop 处理所有IO事件
		- EventLoop注册事件
		- EventLoop将事件派发给ChannelHandler
		- EventLoop安排进一步操作

### Netty(3)
- 关键技术
	- 事件
		- 事件按照数据流进行分类
		- 入站事件：连接激活、数据读取……
		- 出战事件：打开到远程连接、写数据……

	- 事件处理ChannelHandler
		- Channel通道发生数据或状态改变
		- EventLoop会将事件分类 并调用ChannelHandler的回调函数
		- 程序员需要实现ChannelHandler内的回调函数
		- ChannelInboundHandler/ChannelOutboundHandler

### Netty(4)
- 关键技术
	- ChannelHandler工作模式：责任链
		- 责任链模式
			- 将请求的接收者连成一条链
			- 在链上传递请求 直到有一个接收者处理该请求
			- 避免请求和接收者的耦合

		- ChannelHandler可以有多个 依次进行调用
		- ChannelPipeline作为容器 承载多个ChannelHandler

	- ByteBuf
		- 强大的字节容器 提供丰富的API进行操作

### Netty(5)
- 进一步阅读书籍
	- 《Netty实战》2017年出版
	- 《Netty权威指南》2015年出版

### Netty(6)
- Mina
	- Apache Mina,[http://mina.apache.org/](http://mina.apache.org/)
	- NIO框架库
	- 事件驱动的异步网络通讯
	- 和Netty的区别
	- [https://stackoverflow.com/questions/1637752/netty-vs-apache-mina](https://stackoverflow.com/questions/1637752/netty-vs-apache-mina)

## 邮件基础知识
### 邮件基本知识（1）
- 邮件：一封信 包括发件人、收件人、文本、图片、附件等
- 邮件客户端
- 邮件服务端
	- 发送邮件服务器
	- 接收邮件服务器

### 邮件基本知识（2）
- 邮件客户端
	- Foxmail
	- OutLook(Windows MS)
	- Thunderbird(Linux)

- 邮件服务端
	- Microsoft Exchange Server
	- IBM Lotus Notes
	- SendMail Qmail James

### 邮件基础知识（3）
- 主要协议（发送端口25 接收端口110）
	- 发送 SMTP
	- 接收 Pop3
	- 接收 IMAP
		- 摘要浏览
		- 选择下载附件
		- 多文件夹
		- 网络硬盘

### 邮件基本知识（4）
- 邮件格式
	- RFC822邮件格式：邮件头 文字邮件正文
	- MIME　图片、音频、视频等等
- 邮件编码
	- 纯英文邮件编码 ASCII编码 7Bit
	- 8Bit编码
	- BASE64 6个bit A-Za-z0-9+/
	- Quoted-printable 对每个非ASCII字符用“=”加上这个字符的十六位进制编码

### 邮件基本知识（5）
- 邮件中继：通过别人的邮箱服务器（中继服务器）将你的邮箱系统的邮件送到目标地址
- 垃圾邮件（spam）
- 邮件和邮件服务器的安全
- 邮件防火墙 垃圾邮件判定

## Java Mail编程
### java mail 服务器配置
- 邮件服务器支持
	- 需要在邮件服务内设置  可以查看相关邮件帮助
	- 需要知道pop服务器和smtp服务器信息
- java mail 工具包
	- https://javaee.github.io/javamail 目前1.6.2
	- 添加maven dependency

- 关键类
	- Session 邮件会话 和HttpSession不同
	- Store 邮件存储空间
	- Folder 邮件文件夹
	- Message 电子邮件
	- Address 邮件地址
	- Transport 发送协议类


## Code
- ***题目***

> 请基于NIO(第6章第六节的NIO，非AIO)编写一个群聊的程序，包括服务端程序和客户端程序。 



> 服务端功能：只用一个线程，收到某客户端的信息，将消息在控制台输出，然后，发给其他另外的客户端。



> 客户端功能：每隔5秒发送一条信息给服务端。然后接收服务器转发过来的消息，并在控制台输出。

- ***Server***


```
import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.SelectionKey;
import java.nio.channels.Selector;
import java.nio.channels.ServerSocketChannel;
import java.nio.channels.SocketChannel;
import java.nio.charset.StandardCharsets;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;
import java.util.Set;

public class NioServer {

    private static List<SocketChannel> socketChannelList;

    public static void main(String[] args) {
        int port = 8080;
        Selector selector = null;
        ServerSocketChannel serverSocketChannel = null;
        socketChannelList = new ArrayList<>();

        try {
            selector = Selector.open();
            serverSocketChannel = ServerSocketChannel.open();
            serverSocketChannel.configureBlocking(false);
            serverSocketChannel.socket().bind(new InetSocketAddress(port), 1024);
            serverSocketChannel.register(selector, SelectionKey.OP_ACCEPT);
            System.out.println("服务端已经在8080端口守候了");
        } catch (Exception e) {
            e.printStackTrace();
            System.exit(1);
        }

        while (true) {
            try {
                selector.select();
                Set<SelectionKey> selectionKey = selector.selectedKeys();
                Iterator<SelectionKey> selectionKeyIterator = selectionKey.iterator();
                SelectionKey key = null;
                while (selectionKeyIterator.hasNext()) {
                    key = selectionKeyIterator.next();
                    selectionKeyIterator.remove();
                    try {
                        handleInput(selector, key);
                    } catch (Exception e) {
                        if (key != null) {
                            key.cancel();
                            if (key.channel() != null) {
                                key.channel().close();
                            }
                        }
                    }
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    private static void handleInput(Selector selector, SelectionKey key) throws IOException {
        if (key.isValid()) {
            System.out.println("the selectionKey is valid");
            if (key.isAcceptable()) {
                ServerSocketChannel serverSocketChannel = (ServerSocketChannel) key.channel();
                SocketChannel socketChannel = serverSocketChannel.accept();
                System.out.println("server connected with the client successfully");
                //添加到集合里面 方便以后群发
                if (!socketChannelList.contains(socketChannel))
                    socketChannelList.add(socketChannel);
                System.out.println("one socketChannel has been added");
                socketChannel.configureBlocking(false);
                socketChannel.register(selector, SelectionKey.OP_READ);

            }
            if (key.isReadable()) {
                System.out.println("the selectionKey now is readable");
                SocketChannel socketChannel1 = (SocketChannel) key.channel();
                ByteBuffer readBuffer = ByteBuffer.allocate(1024);
                int requestCode = socketChannel1.read(readBuffer);
                if (requestCode > 0) {
                    readBuffer.flip();
                    byte[] bytes = new byte[readBuffer.remaining()];
                    readBuffer.get(bytes);
                    String content = new String(bytes, StandardCharsets.UTF_8);
                    System.out.println(content);
                    write(socketChannelList, content);
                } else if (requestCode < 0) {
                    key.cancel();
                    System.out.println("the key has been canceled");
                    socketChannel1.close();
                }
            }
        }
    }

    private static void write(List<SocketChannel> socketChannelList, String content) throws IOException {
        if (content != null && content.trim().length() > 0) {
            byte[] bytes = ("服务器转发：" + content).getBytes(StandardCharsets.UTF_8);
            for (SocketChannel socketChannel : socketChannelList) {
                //踩了个大坑 writeBuffer使用了write方法之后里面的数据会被清空 如果writeBuffer的生成放在了foreach外面 那么第二次及以后在foreach里面输出的为空
                ByteBuffer writeBuffer = ByteBuffer.allocate(bytes.length);
                writeBuffer.put(bytes);
                writeBuffer.flip();
                socketChannel.write(writeBuffer);

            }
        }
    }
}

```
- ***Client1***


```
import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.SelectionKey;
import java.nio.channels.Selector;
import java.nio.channels.SocketChannel;
import java.nio.charset.StandardCharsets;
import java.util.Iterator;
import java.util.Set;

public class NioClient1 {
    public static void main(String[] args) {
        String host = "127.0.0.1";
        int port = 8080;
        Selector selector = null;
        SocketChannel socketChannel = null;
        try {
            selector = Selector.open();
            socketChannel = SocketChannel.open();
            socketChannel.configureBlocking(false);
            if (socketChannel.connect(new InetSocketAddress(host, port))) {
                socketChannel.register(selector, SelectionKey.OP_READ);
                System.out.println("connected successfully at the first road");
                write(socketChannel);
            } else {
                socketChannel.register(selector, SelectionKey.OP_CONNECT);
            }

        } catch (IOException e) {
            e.printStackTrace();
            System.exit(1);
        }
        while (true) {
            try {
                selector.select(1000);
                Set<SelectionKey> selectionKeys = selector.selectedKeys();
                Iterator<SelectionKey> selectionKeyIterator = selectionKeys.iterator();
                SelectionKey key = null;
                while (selectionKeyIterator.hasNext()) {
                    key = selectionKeyIterator.next();
                    selectionKeyIterator.remove();

                    try {
                        readInput(selector, key);
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }

    private static void readInput(Selector selector, SelectionKey key) throws IOException {
        if (key.isValid()) {
            SocketChannel socketChannel = (SocketChannel) key.channel();
            if (key.isConnectable()) {
                if (socketChannel.finishConnect()) {
                    socketChannel.register(selector, SelectionKey.OP_READ);
                    System.out.println("now the client1 is readable");
                }
            }
            if (key.isReadable()) {
                System.out.println("ready to read the input content from the server");
                ByteBuffer readBuffer = ByteBuffer.allocate(1024);
                int requestCode = socketChannel.read(readBuffer);
                if (requestCode > 0) {
                    readBuffer.flip();
                    byte[] bytes = new byte[readBuffer.remaining()];
                    readBuffer.get(bytes);
                    String content = new String(bytes, "UTF-8");
                    System.out.println(content);
                } else if (requestCode < 0) {
                    key.cancel();
                    socketChannel.close();
                }
            }
            write(socketChannel);

        }
    }

    private static void write(SocketChannel socketChannel) {
        try {

            String content = "注意：这是来自客户端1的消息";
            byte[] bytes = content.getBytes();
            ByteBuffer writeBuffer = ByteBuffer.allocate(bytes.length);
            writeBuffer.put(bytes);
            writeBuffer.flip();
            socketChannel.write(writeBuffer);
            System.out.println("a message is sent to the server");
            Thread.sleep(5000);

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```

- ***Client2***

```
import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.SelectionKey;
import java.nio.channels.Selector;
import java.nio.channels.SocketChannel;
import java.nio.charset.StandardCharsets;
import java.util.Iterator;
import java.util.Set;

public class NioClient2 {
    public static void main(String[] args) {
        String host = "127.0.0.1";
        int port = 8080;
        Selector selector = null;
        SocketChannel socketChannel = null;
        try {
            selector = Selector.open();
            socketChannel = SocketChannel.open();
            socketChannel.configureBlocking(false);
            if (socketChannel.connect(new InetSocketAddress(host, port))) {
                socketChannel.register(selector, SelectionKey.OP_READ);
                System.out.println("connected successfully at the first road");
                write(socketChannel);
            } else {
                socketChannel.register(selector, SelectionKey.OP_CONNECT);
            }

        } catch (IOException e) {
            e.printStackTrace();
            System.exit(1);
        }
        while (true) {
            try {
                selector.select(1000);
                Set<SelectionKey> selectionKeys = selector.selectedKeys();
                Iterator<SelectionKey> selectionKeyIterator = selectionKeys.iterator();
                SelectionKey key = null;
                while (selectionKeyIterator.hasNext()) {
                    key = selectionKeyIterator.next();
                    selectionKeyIterator.remove();

                    try {
                        readInput(selector, key);
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }

    private static void readInput(Selector selector, SelectionKey key) throws IOException {
        if (key.isValid()) {
            SocketChannel socketChannel = (SocketChannel) key.channel();
            if (key.isConnectable()) {
                if (socketChannel.finishConnect()) {
                    socketChannel.register(selector, SelectionKey.OP_READ);
                    System.out.println("now the key of client2 is readable");
                }
            }
            if (key.isReadable()) {
                System.out.println("client2 is ready to read the input content from the server");
                ByteBuffer readBuffer = ByteBuffer.allocate(1024);
                int requestCode = socketChannel.read(readBuffer);
                if (requestCode > 0) {
                    readBuffer.flip();
                    byte[] bytes = new byte[readBuffer.remaining()];
                    readBuffer.get(bytes);
                    String content = new String(bytes, "UTF-8");
                    System.out.println(content);
                } else if (requestCode < 0) {
                    key.cancel();
                    socketChannel.close();
                }
            }
            write(socketChannel);

        }
    }

    private static void write(SocketChannel socketChannel) {
        try {

            String content = "注意：这是来自客户端2的消息";
            byte[] bytes = content.getBytes();
            ByteBuffer writeBuffer = ByteBuffer.allocate(bytes.length);
            writeBuffer.put(bytes);
            writeBuffer.flip();
            socketChannel.write(writeBuffer);
            System.out.println("a message is sent to the server from client2");
            Thread.sleep(5000);

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```