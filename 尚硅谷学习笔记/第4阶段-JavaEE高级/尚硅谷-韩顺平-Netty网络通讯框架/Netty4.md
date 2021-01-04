# Netty4

> 视频: https://www.bilibili.com/video/BV1DJ411m7NR
>
> 说明: 
>
> - Netty是当前非常流行的网络通讯框架，当程序对网络数据处理时，需要保证高并发和高可靠，底层就可以用Netty支撑
> - 本套课程详细讲解了Netty核心技术点，同时进行底层机制和源码剖析，并编写了大量的应用实例。通过学习可以快速掌握Netty的底层实现机制，熟练运用Netty解决网络高并发问题。

## 第 1 章: Netty介绍和应用场景

### 1.1 本课程学习要求 

1. 本课程不适用于 0 基础的学员 
2. 要求已经掌握了 Java 编程， 主要技术构成： Java OOP 编程、Java 多线程编程、Java IO 编程 、Java 网络编程、常用的 Java设计模式(比如 观察者模式 ，命令模式，职责链模式 )、常用的数据结构(比如 链表) 
3. 本课程的 <<Netty 核心源码剖析章节>> 要求学员最好有项目开发和阅读源码的经历

### 1.2 Netty 的介绍

1. Netty 是由 JBOSS 提供的一个 Java开源框架，现为 Github 上的独立项目。[github地址](https://github.com/netty/netty)

2. Netty 是一个**异步**的、**基于事件驱动**的网络应用框架，用以快速开发高性能、高可靠性的网络 IO 程序。

   ![image-20201229152516444](assets/image-20201229152516444.png)

3. Netty 主要针对在 TCP 协议下，面向 Clients 端的高并发应用，或者 Peer-to-Peer 场景下的大量数据持续传输的应用。

4. Netty 本质是一个 NIO 框架，适用于服务器通讯相关的多种应用场景

   ![image-20201229155012488](assets/image-20201229155012488.png)

5. 要透彻理解 Netty ， 需要先学习 NIO ， 这样我们才能阅读 Netty 的源码。



### 1.3 Netty 的应用场景

#### 1.3.1 互联网行业  

1. 互联网行业：在分布式系统中，各个节点之间需要远程服务调用，高性能的 RPC 框架必不可少，Netty 作为 异步高性能的通信框架，往往作为基础通信组件被这些 RPC 框架使用。 
2. 典型的应用有：阿里分布式服务框架 Dubbo 的 RPC 框架使用 Dubbo 协议进行节点间通信，Dubbo 协议默 认使用 Netty 作为基础通信组件，用于实现各进程节点之间的内部通信 

#### 1.3.2 游戏行业

1. 无论是手游服务端还是大型的网络游戏，Java 语言得到了越来越广泛的应用
2. Netty 作为高性能的基础通信组件，提供了 TCP/UDP 和 HTTP 协议栈，方便定制和开发私有协议栈，账号登录服务器 
3. 地图服务器之间可以方便的通过 Netty 进行高性能的通信

#### 1.3.3 大数据领域 

1. 经典的 Hadoop 的高性能通信和序列化组件 Avro 的 RPC 框架，默认采用 Netty 进行跨界点通信

2. 它的 Netty Service 基于 Netty 框架二次封装实现。

   ![image-20201229142814562](assets/image-20201229142814562.png)

#### 1.3.4 其它开源项目使用到 Netty 

网址: https://netty.io/wiki/related-projects.html



### 1.4 Netty 的学习参考资料

- [Netty IN ACTION](https://book.douban.com/subject/27038538/): 作者参与了nety4的内核开发,实战方面可以使用这本书
- [Netty 权威指南](https://book.douban.com/subject/26373138/): 基于Netty5的,原理和理论方面的东西比较透彻,但是netty5已经被废弃了

![image-20201229142901822](assets/image-20201229142901822.png)

---

## 第 2 章: JavaBIO编程

*BIO就是阻塞IO,java的原生IO知识*

### 2.1 I/O模型

1. I/O 模型简单的理解：就是用什么样的通道进行数据的发送和接收，很大程度上决定了程序通信的性能

2. Java 共支持 3 种网络编程模型/IO 模式：BIO、NIO、AIO

3. Java BIO ： **同步并阻塞(传统阻塞型)** ，服务器实现模式为一个连接一个线程，即客户端有连接请求时服务器端就需要启动一个线程进行处理，如果这个连接不做任何事情会造成不必要的线程开销 【简单示意图】 

   **一个线程维护一个连接(socket)**

   ![image-20201229162243940](assets/image-20201229162243940.png)

4. Java NIO ： **同步非阻塞**，服务器实现模式为一个线程处理多个请求(连接/客户端)，即客户端发送的连接请求都会注册到多路复用器上，多路复用器轮询到连接有 I/O 请求就进行处理 【简单示意图】

   **一个线程维护多个连接(socket)**

   ![image-20201229143142107](assets/image-20201229143142107.png)

5. Java AIO(NIO.2) ： **异步非阻塞**，AIO 引入异步通道的概念，采用了 Proactor 模式，简化了程序编写，有效的请求才启动线程，它的特点是先由操作系统完成后才通知服务端程序启动线程去处理，一般适用于连接数较 多且连接时间较长的应用 【了解】

6. 我们依次展开讲解 

### 2.2 BIO、NIO、AIO 适用场景分析

1. <u>BIO 方式适用于连接数目比较小且固定的架构</u>，这种方式对服务器资源要求比较高，并发局限于应用中，JDK1.4 以前的唯一选择，但程序简单易理解。
2. <u>NIO 方式适用于连接数目多且连接比较短（轻操作）的架构</u>，比如聊天服务器，弹幕系统，服务器间通讯等。 编程比较复杂，JDK1.4 开始支持。netty框架就是基于NIO的
3. <u>AIO 方式使用于连接数目多且连接比较长（重操作）的架构</u>，比如相册服务器，充分调用 OS 参与并发操作，编程比较复杂，JDK7 开始支持。但是AIO目前还没有得到广泛的应用

### 2.3 Java BIO 基本介绍

1. **Java BIO** 就是传统的 **Java IO** **编程**，其相关的类和接口在 java.io
2. BIO(blocking I/O) ： 同步阻塞，服务器实现模式为一个连接一个线程，即客户端有连接请求时服务器端就需要启动一个线程进行处理，如果这个连接不做任何事情会造成不必要的线程开销，可以通过线程池机制改善(实现多个客户连接服务器)。 【后有应用实例】
3. BIO 方式适用于连接数目比较小且固定的架构，这种方式对服务器资源要求比较高，并发局限于应用中，JDK1.4 以前的唯一选择，程序简单易理解

### 2.4 Java BIO工作机制

![image-20201229143332119](assets/image-20201229143332119.png)

**对BIO编程流程的回顾梳理:**

1. 服务器端启动一个 ServerSocket 
2. 客户端启动 Socket 对服务器进行通信，默认情况下服务器端需要对每个客户 建立一个线程与之通讯
3. 客户端发出请求后, 先咨询服务器是否有线程响应，如果没有则会等待，或者被拒绝
4. 如果有响应，客户端线程会等待请求结束后，在继续执行

### 2.5 Java BIO 应用实例 🚩

**实例说明:** 

1. 使用 BIO 模型编写一个服务器端，监听 6666 端口，当有客户端连接时，就启动一个线程与之通讯。
2. 要求使用线程池机制改善，可以连接多个客户端.
3. 服务器端可以接收客户端发送的数据(telnet 方式即可)。

**代码演示:**

![image-20201229164618376](assets/image-20201229164618376.png)

```java
package com.atguigu.bio; 
import java.io.InputStream; 
import java.net.ServerSocket;
import java.net.Socket; 
import java.util.concurrent.ExecutorService; 
import java.util.concurrent.Executors;
public class BIOServer {
    public static void main(String[] args) throws Exception { 
        //线程池机制
        //思路 
        //1. 创建一个线程池 
        //2. 如果有客户端连接，就创建一个线程，与之通讯(单独写一个方法)
        ExecutorService newCachedThreadPool = Executors.newCachedThreadPool(); //创建ServerSocket 
        ServerSocket serverSocket = new ServerSocket(6666); 
        System.out.println("服务器启动了"); 
        while (true) { 
            System.out.println(" 线 程 信 息 id =" + Thread.currentThread().getId() + " 名 字 =" + Thread.currentThread().getName()); 
            //监听，等待客户端连接 
            System.out.println("等待连接...."); 
            final Socket socket = serverSocket.accept();
            System.out.println("连接到一个客户端");
            //就创建一个线程，与之通讯(单独写一个方法) 
            newCachedThreadPool.execute(new Runnable() { 
                public void run() { //我们重写 
                    //可以和客户端通讯 
                    handler(socket);
                }});
        } 
    }
    //编写一个 handler 方法，和客户端通讯 
    public static void handler(Socket socket) {
        try { 
            System.out.println(" 线 程 信 息 id =" + Thread.currentThread().getId() + " 名 字 =" + Thread.currentThread().getName());
            byte[] bytes = new byte[1024];
            //通过 socket 获取输入流 
            InputStream inputStream = socket.getInputStream();
            //循环的读取客户端发送的数据 
            while (true) { 
                System.out.println(" 线 程 信 息 id =" + Thread.currentThread().getId() + " 名 字 =" + Thread.currentThread().getName()); 
                System.out.println("read...."); int read = inputStream.read(bytes); 
                if(read != -1) {
                    System.out.println(new String(bytes, 0, read )); //输出客户端发送的数据 
                } else {
                    break; 
                } 
            } 
        }catch (Exception e) {
            e.printStackTrace();
        }finally {
            System.out.println("关闭和 client 的连接"); 
            try { socket.close(); 
                }catch (Exception e) { 
                e.printStackTrace(); 
            }
        } 
    }
}
```



### 2.6 Java BIO问题分析

1. 每个请求都需要创建独立的线程，与对应的客户端进行数据 Read，业务处理，数据 Write 。
2. 当并发数较大时，需要创建大量线程来处理连接，系统资源占用较大。 
3. 连接建立后，如果当前线程暂时没有数据可读，则线程就阻塞在 Read 操作上，造成线程资源浪费







---

## 第 3 章: Java NIO编程

### 3.1 Java NIO基本介绍

1. Java NIO 全称 **java non-blocking IO**，是指 JDK 提供的新 API。从 JDK1.4 开始，Java 提供了一系列改进的输入/输出的新特性，被统称为 NIO(即 New IO)，是同步非阻塞的
2. NIO 相关类都被放在 **java.nio** 包及子包下，并且对原 java.io 包中的很多类进行改写。【基本案例】
3. NIO 有三大核心部分：**Channel(通道)，Buffer(缓冲区), Selector(选择器)**
4. **NIO** **是 面向缓冲区** ，**或者面向 块 编程的**。数据读取到一个它稍后处理的缓冲区，需要时可在缓冲区中前后移动，这就增加了处理过程中的灵活性，使用它可以提供非阻塞式的高伸缩性网络 
5. Java NIO 的非阻塞模式，使一个线程从某通道发送请求或者读取数据，但是它仅能得到目前可用的数据，如果 目前没有数据可用时，就什么都不会获取，而不是保持线程阻塞，所以直至数据变的可以读取之前，该线程可 以继续做其他的事情。 非阻塞写也是如此，一个线程请求写入一些数据到某通道，但不需要等待它完全写入， 这个线程同时可以去做别的事情。【后面有案例说明】
6. 通俗理解：NIO 是可以做到用一个线程来处理多个操作的。假设有 10000 个请求过来,根据实际情况，可以分配 50 或者 100 个线程来处理。不像之前的阻塞 IO 那样，非得分配 10000 个。
7. HTTP2.0 使用了多路复用的技术，做到同一个连接并发处理多个请求，而且并发请求的数量比 HTTP1.1 大了好 几个数量级 

**案例说明 NIO 的 Buffer** 

```java
package com.atguigu.nio; import java.nio.IntBuffer;
public class BasicBuffer { public static void main(String[] args) { //举例说明 Buffer 的使用 (简单说明) //创建一个 Buffer, 大小为 5, 即可以存放 5 个 int IntBuffer intBuffer = IntBuffer.allocate(5); //向 buffer 存放数据 // intBuffer.put(10); // intBuffer.put(11); // intBuffer.put(12); // intBuffer.put(13); // intBuffer.put(14); for(int i = 0; i < intBuffer.capacity(); i++) { intBuffer.put( i * 2); }//如何从 buffer 读取数据 //将 buffer 转换，读写切换(!!!) intBuffer.flip(); while (intBuffer.hasRemaining()) { System.out.println(intBuffer.get()); } }}
```



### 3.2 NIO和BIO的比较

1. BIO 以流的方式处理数据,而 NIO 以块的方式处理数据,块 I/O 的效率比流 I/O 高很多
2. BIO 是阻塞的，NIO 则是非阻塞的
3. BIO 基于字节流和字符流进行操作，而 NIO 基于 Channel(通道)和 Buffer(缓冲区)进行操作，数据总是从通道 读取到缓冲区中，或者从缓冲区写入到通道中。Selector(选择器)用于监听多个通道的事件（比如：连接请求， 数据到达等），因此使用单个线程就可以监听多个客户端通道 

### 3.3 NIO三大核心原理示意图

一张图描述 NIO 的 Selector 、 Channel 和 Buffer 的关系

Selector 、 Channel 和 Buffer 的关系图(简单版)

![image-20201229144630037](assets/image-20201229144630037.png)

关系图的说明:

1. 每个 channel 都会对应一个 Buffer 
2. Selector 对应一个线程， 一个线程对应多个 channel(连接) 
3. 该图反应了有三个 channel 注册到 该 selector //程序
4. 程序切换到哪个 channel 是有事件决定的, Event 就是一个重要的概念 
5. Selector 会根据不同的事件，在各个通道上切换 
6. Buffer 就是一个内存块 ， 底层是有一个数组
7. 数据的读取写入是通过 Buffer, 这个和 BIO , BIO 中要么是输入流，或者是 输出流, 不能双向，但是 NIO 的 Buffer 是可以读也可以写, 需要 flip 方法切换 channel 是双向的, 可以返回底层操作系统的情况, 比如 Linux ， 底层的操作系统通道就是双向的



### 3.4 缓冲区(Buffer)

#### 3.4.1 基本介绍

缓冲区（Buffer）：缓冲区本质上是一个可以读写数据的内存块，可以理解成是一个容器对象(含数组)，该对 象提供了一组方法，可以更轻松地使用内存块，，缓冲区对象内置了一些机制，能够跟踪和记录缓冲区的状态变化情况。Channel 提供从文件、网络读取数据的渠道，但是读取或写入的数据都必须经由 Buffer，如图: 【后面举 例说明】

![image-20201229144833805](assets/image-20201229144833805.png)





#### 3.4.2 Buffer类及其子类

1. 在 NIO 中，Buffer 是一个顶层父类，它是一个抽象类, 类的层级关系图:
2. Buffer 类定义了所有的缓冲区都具有的四个属性来提供关于其所包含的数据元素的信息:
3. Buffer 类相关方法一览

#### 3.4.3 ByteBuffer

从前面可以看出对于 Java 中的基本数据类型(boolean 除外)，都有一个 Buffer 类型与之相对应，最常用的自然是 ByteBuffer 类（二进制数据），该类的主要方法如下：

```java

```



### 3.5. 通道(Channel)

### 3.6 基本介绍



### 3.7 Selector(选择器)



### 3.8 NIO非阻塞网络编程原理分析图



### 3.9 NIO非阻塞网络编程快速入门



### 3.10 SelectionKey



### 3.11 ServerSocketChannel



### 3.12 SocketChannel



### 3.13 NIO网络编程应用实例-群聊系统



### 3.14 NIO与零拷贝



### 3.15 Java AIO 基本介绍



### 3.16 BIO NIO AIO 对比表



---

## 第 4 章: Netty概述



### 4.1 原生 NIO 存在的问题 

1. NIO 的类库和 API 繁杂，使用麻烦：需要熟练掌握 Selector、ServerSocketChannel、SocketChannel、ByteBuffer 等。
2. 需要具备其他的额外技能：要熟悉 Java 多线程编程，因为 NIO 编程涉及到 Reactor 模式，你必须对多线程 和网络编程非常熟悉，才能编写出高质量的 NIO 程序。 
3. 开发工作量和难度都非常大：例如客户端面临断连重连、网络闪断、半包读写、失败缓存、网络拥塞和异常流 的处理等等。
4. JDK NIO 的 Bug：例如臭名昭著的 Epoll Bug，它会导致 Selector 空轮询，最终导致 CPU 100%。直到 JDK 1.7 版本该问题仍旧存在，没有被根本解决。

### 4.2 Netty 官网说明

官网：https://netty.io/ 

Netty is an asynchronous event-driven network application framework 

for rapid development of maintainable high performance protocol servers & clients

![image-20201229145548677](assets/image-20201229145548677.png)

### 4.3 Netty 的优点

Netty 对 JDK 自带的 NIO 的 API 进行了封装，解决了上述问题。

1. 设计优雅：适用于各种传输类型的统一 API 阻塞和非阻塞 Socket；基于灵活且可扩展的事件模型，可以清晰 地分离关注点；高度可定制的线程模型 - 单线程，一个或多个线程池.
2. 使用方便：详细记录的 Javadoc，用户指南和示例；没有其他依赖项，JDK 5（Netty 3.x）或 6（Netty 4.x）就 足够了。
3. 高性能、吞吐量更高：延迟更低；减少资源消耗；最小化不必要的内存复制。
4. 安全：完整的 SSL/TLS 和 StartTLS 支持。
5. 社区活跃、不断更新：社区活跃，版本迭代周期短，发现的 Bug 可以被及时修复，同时，更多的新功能会被 加入

### 4.4 Netty 版本说明

1. netty 版本分为 netty3.x 和 netty4.x、netty5.x 
2. 因为 Netty5 出现重大 bug，已经被官网废弃了，目前推荐使用的是 Netty4.x 的稳定版本 
3. 目前在官网可下载的版本 netty3.x netty4.0.x 和 netty4.1.x 
4. 在本套课程中，我们讲解 Netty4.1.x 版本 
5. netty 下载地址： https://bintray.com/netty/downloads/netty/















---



## 第 5 章:  Netty高性能架构设计

### 5.1 线程模型基本介绍

### 5.2 传统阻塞 I/O 服务模型

### 5.3 Reactor 模式

### 5.4 单 Reactor 单线程 

### 5.5 单 Reactor 多线程

### 5.6 主从 Reactor 多线程

### 5.7 Reactor 模式小结

### 5.8 Netty 模型

### 5.9 异步模型

### 5.10 快速入门实例-HTTP 服务







---

## 第 6 章: Netty核心模块组件



### 6.1 Bootstrap、ServerBootstrap

### 6.2 Future、ChannelFuture 

### 6.3 Channel

### 6.4 Selector 

### 6.5 ChannelHandler 及其实现类

### 6.6 Pipeline 和 ChannelPipeline

### 6.7 ChannelHandlerContext

### 6.8 ChannelOption

### 6.9 EventLoopGroup 和其实现类 NioEventLoopGroup

### 6.10 Unpooled 类

### 6.11 Netty 应用实例-群聊系统

### 6.12 Netty 心跳检测机制案例

### 6.13 Netty 通过 WebSocket 编程实现服务器和客户端长连接











---

## 第 7 章: Google-Protobuf

### 7.1 编码和解码的基本介绍

### 7.2 Netty 本身的编码解码的机制和问题分析

### 7.3 Protobuf

### 7.4 Protobuf 快速入门实例

### 7.5 Protobuf 快速入门实例 2 







---

## 第 8 章: Netty编码器和handler的调用机制

### 8.1 基本说明 

### 8.2 编码解码器

### 8.3 解码器-ByteToMessageDecoder

### 8.4 Netty 的 handler 链的调用机制 

### 8.5 解码器-ReplayingDecoder

### 8.6 其它编解码器

### 8.7 Log4j 整合到 Netty









---

## 第 9 章: TCP粘包和拆包及解决方案

### 9.1 TCP 粘包和拆包基本介绍

### 9.2 TCP 粘包和拆包现象实例

### 9.3 TCP 粘包和拆包解决方案

### 9.4 看一个具体的实例









---

## 第 10 章: Netty核心源码剖析

### 10.1 基本说明

### 10.2  Netty 启动过程源码剖析

### 10.3 Netty 接受请求过程源码剖析

### 10.4 Pipeline Handler HandlerContext 创建源码剖析

### 10.5 ChannelPipeline 调度 handler 的源码剖析

### 10.6 Netty 心跳(heartbeat)服务源码剖析 

### 10.7 Netty 核心组件 EventLoop 源码剖析 

### 10.8 handler 中加入线程池和 Context 中添加线程池的源码剖析









---

## 第 11 章: 用Netty自己实现dubb-RPC

### 11.1 RPC 基本介绍

### 11.2 RPC 调用流程图

### 11.3 RPC 调用流程说明

### 11.4 自己实现 dubbo RPC(基于 Netty)

