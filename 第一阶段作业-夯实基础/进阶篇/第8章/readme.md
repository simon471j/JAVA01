# 第七章节
## RMI
- 全称：远程方法调用
- 以前写的程序都是在单虚拟机JVM上运行的
- 两个位于不同JVM虚拟机的Java程序互相请求访问

### 多虚拟机JVM的程序运行
- 启动多个main程序 这些程序可以部署在多个机器、虚拟机上
- 多个**进程**可通过网路互相**传递消息**进行协作
- **进程通过RMI可调用另一个机器上的Java函数**

### RMI的参数和返回值
- （自动化）传递远程对象（实现Remote接口）
	- 当一个远程对象的引用从一个JVM传递到另一个JVM 该远程对象的发送者和接收者将持有同一个实体对象的引用 这个引用并非是一个内存位置，而是由网络地址和该远程对象的唯一标识符构成的

- （自动化）传递可序列化对象（实现Serializable）
	- JVM中的一个对象经过序列化之后的字节，通过网络，其副本传递到另一个JVM中 并重新还原为一个Java对象 **每一个JVM上都拥有自己的对象**

![](https://thumbnail7.baidupcs.com/thumbnail/578b61a79m6879f94a0a7f6a4cefed0f?fid=2722863950-250528-811459404525596&time=1622505600&rt=yt&sign=FDTAER-DCb740ccc5511e5e8fedcff06b081203-zXdoVHmKViplyuAG3UYObv31lAY%3D&expires=24h&chkv=0&chkbd=0&chkpc=&dp-logid=4075167720&dp-callid=0&size=c1536_u864&quality=90&vuk=-&ft=video&autopolicy=1)

### RMI的优点和缺点
- RMI的优点
	- 跨平台分布式对象调用
	- 完全对象支持
	- 安全策略

- RMI缺点
	- 双方必须是Java语言实现
	- 不如消息传递协作（网络编程部分）方便

### 注意
- 对象是不可以被序列化的

## JNI
- Java Native Interface
- java和本地C代码进行互相操作
	- Java调用C程序完成一些需要快速计算的功能**（常见 重点）**
	- C调用Java程序（反射）

- 在Java类中声明一个**本地方法**(用native修饰)
- 调用javac.exe编译 得到HelloNative.class

```
class HelloNative{
	public static native void greeting();
}
```
- 调用javah.exe得到包含该方法（Java_HelloNative_greeting）的头文件HelloNative.h
- 实现.c文件（对应HelloNative.h）
- 将.c和.h文件整合为共享库（DLL）文件
- 在Java类中 加载相应的共享库文件

### 总结
- 通过JNI 可以实现java调用C程序进行计算
- 采用JNI 将丧失Java跨平台性的特点

## Nashorn
- java调用JavaScript

### JavaScript
- 1995年Netscape网景公司发布
- 脚本语言（解释型语言）
	- 便于快速变更 可以修改运行时的程序行为
	- 支持程序用户的定制化

- 可用于Web前端和后端开发（全栈）
- JS基本教程：W3CSchool

### java调用JS
- 脚本引擎 ScriptEngine
- Nashorn JDK8自带的JS解释器（JDK6/7是Rhino解释器）

`ScriptEngine engine = new ScriptEngineManager().getEngineByName("nashorn")`

- 主要方法
	- eval 执行一段js脚本 .eval(String str), eval(Reader reader)
	- put 设置一个变量
	- get 获取一个变量
	- createBindings 创建一个Bindings
	- setBindings 设置脚本变量的使用范围

## Jython
### Python语言
- 1989年由Guido van Rossum设计发明
- 解释型脚本语言
- 可用于科学计算软件/Web/桌面软件开发

### Jython
- 曾用名（JPython）
	- Jython是Python语言在Java平台上面的实现
	- Jython是在JVM上实现的 Python 由Java编写
	- **Jython将Python源码编译成JVM字节码** 由JVM执行对应的字节码 因此能很好的与JVM集成 **Jython并不是Java和Python的连接器**
	- 需要添加依赖

- 关键类
	- PythonInterpreter
	- exec 执行语句
	- set 设置变量值
	- get 获取变量值
	- execfile执行一个python文件

- PyObject
- PyFunction

### 总结
- Jython适合用来编写小应用程序 增加程序动态性

## Java调用Web Service
- Web Service
	- 由万维网联盟提出
	- 消除语言差异 平台差异 协议差异 数据结构差异 成为不同构建模型和异构系统之间的集成技术
	- Web Service是为实现跨网络操作而设计的软件系统，提供了相关的操作接口 其他应用可以使用SOAP消息 以预先指定的方式来与Web Service进行交互
	- 服务提供者
	- 服务注册中心
	- 服务请求者

- 调用wsimport所产生的客户端中间代码
- 提供相应参数
- 获取返回结果

![](https://thumbnail0.baidupcs.com/thumbnail/896ef6ca3t3cf355d33b64c1bb492f58?fid=2722863950-250528-250204214150024&rt=pr&sign=FDTAER-DCb740ccc5511e5e8fedcff06b081203-lV4AmPLVLVamz%2b8eH7hoFx9cuZo%3d&expires=8h&chkbd=0&chkv=0&dp-logid=55000228550678173&dp-callid=0&time=1622520000&size=c1536_u864&quality=90&vuk=2722863950&ft=image&autopolicy=1)

![](https://thumbnail0.baidupcs.com/thumbnail/2fc86bb0ej67b4efcf97c3036f27ccc3?fid=2722863950-250528-919671860352659&rt=pr&sign=FDTAER-DCb740ccc5511e5e8fedcff06b081203-rUY3ywfuamPvI%2bvc5wEOIk%2bs8IA%3d&expires=8h&chkbd=0&chkv=0&dp-logid=55000228550678173&dp-callid=0&time=1622520000&size=c1536_u864&quality=90&vuk=2722863950&ft=image&autopolicy=1)

- 其他调用WS的方法
	- Axis/Axis2(axis.apache.org)
	- 采用URLConnection访问Web Service
	- 采用HttpClient访问Web Service

## java调用命令行
### 命令行
- 很多程序没有源码 但是可以执行
	- 命令行的执行方式
	- 可以带输入参数 可以有输出结果

### Runtime
- java提供Runtime类
	- exec以独立的进程执行命令command 并返回Process句柄
	- 当独立进程启动之后 需要处理该进程的输出流/错误流
		- 调用Process.getInputStream可以获取进程的输出流
		- 调用Process.getErrorStream可以获取进程的错误输出流

	- 调用Process.waitFor等待目标进程的中止（当前进程会阻塞）

## Code


- 客户端
![](http://m.qpic.cn/psc?/V52eQ02X1BMeBe145GYm44PR403TVauc/ruAMsa53pVQWN7FLK88i5oFFXwBDLcuMjxetncwwQrAVGh1bpI4aLlgjS9YYRwRKLh1qEMhRWCj82TewbGvX.scIjw7FFH2QygZYmCk4pOk!/mnull&bo=oQFDAAAAAAADB8E!&rf=photolist&t=5)

```
package Server;

import javax.naming.Context;
import javax.naming.InitialContext;
import javax.naming.NamingException;
import java.rmi.RemoteException;
import java.util.Scanner;

public class HelloWorldClient {



    public static void main(String[] args) throws NamingException {
        Context context = new InitialContext();
        String url = "rmi://127.0.0.1:8080/helloworld";
        IHelloWorld helloWorldImpl = (IHelloWorld) context.lookup(url);
        System.out.println("请输入指令：如输入javac HelloWorld，就通过RMI方法请求服务端的编译HelloWorld功能；" +
                "如输入java HelloWorld，就通过RMI方法请求服务端的执HelloWorld功能");
        Scanner scanner = new Scanner(System.in);
        String cmd;
        while (true) {
            cmd = scanner.nextLine();
            if (cmd.equals("javac HelloWorld"))
                break;
            else if (cmd.equals("java HelloWorld"))
                break;
            else
                System.out.println("wrong input, please retry");

        }
        try {
            helloWorldImpl.printHelloWorld(cmd);
        } catch (RemoteException e) {
            e.printStackTrace();
            System.out.println("方法执行异常");
        }
    }
}

```

```
package Server;

import java.rmi.Remote;
import java.rmi.RemoteException;

public interface IHelloWorld extends Remote {
    public void printHelloWorld(String cmd) throws RemoteException;
}
```
- 服务端
- ![](http://m.qpic.cn/psc?/V52eQ02X1BMeBe145GYm44PR403TVauc/ruAMsa53pVQWN7FLK88i5oFFXwBDLcuMjxetncwwQrCt6LDoi2bWusyKEq2tPn7l*sKtwG005UBhOgwhUhiz5yg5grHZ4wm2WMNN6XGD*nM!/mnull&bo=jQGvAAAAAAADBwE!&rf=photolist&t=5)

```
package Server;

import java.io.*;
import java.rmi.RemoteException;
import java.rmi.server.UnicastRemoteObject;

public class IHelloWorldImpl extends UnicastRemoteObject implements IHelloWorld {
    protected IHelloWorldImpl() throws RemoteException {

    }

    @Override
    public void printHelloWorld(String cmd) {
        String compileFilePath = "javac D:/IDEA工程文件/chapter_8_server/src/main/java/Server/HelloWorld.java";
        String runFilePath = "java D:/IDEA工程文件/chapter_8_server/src/main/java/Server/HelloWorld";
        if (cmd.equals("javac HelloWorld"))
            cmd = compileFilePath;
        else if (cmd.equals("java HelloWorld"))
            cmd = runFilePath;
        Process process;
        try {
            process = Runtime.getRuntime().exec(cmd);
            System.out.println("执行成功");
        } catch (IOException e) {
            e.printStackTrace();
            try {
                process = Runtime.getRuntime().exec(cmd);
                InputStream inputStream = process.getErrorStream();
                InputStreamReader inputStreamReader = new InputStreamReader(inputStream);
                BufferedReader bufferedReader = new BufferedReader(inputStreamReader);
                String content;
                while ((content = bufferedReader.readLine()) != null)
                    System.out.println(content);
            } catch (IOException ioException) {
                ioException.printStackTrace();
            }
        }
    }
}

```

```
package Server;

import java.io.*;

public class HelloWorld {
    public static void main(String[] args) throws IOException {
        System.out.println("helloworld");
    }
}

```

```
package Server;

import java.net.MalformedURLException;
import java.rmi.Naming;
import java.rmi.RemoteException;
import java.rmi.registry.LocateRegistry;

public class HelloWorldServer {
    public static void main(String[] args) throws RemoteException, MalformedURLException {
        IHelloWorldImpl helloWorldImpl = new IHelloWorldImpl();
        System.out.println("对象生成");
        LocateRegistry.createRegistry(8080);
        Naming.rebind("rmi://127.0.0.1:8080/helloworld",helloWorldImpl);
        System.out.println("在8080端口注册 等待客户端");
    }
}

```

```
package Server;

import java.rmi.Remote;
import java.rmi.RemoteException;

public interface IHelloWorld extends Remote {
    public void printHelloWorld(String cmd) throws RemoteException;
}

```
### 注意：客户端和服务端包名要一致 不然会出现以下错误
    java.lang.ClassNotFoundException: XXX (no security manager: RMI class loader disabled)