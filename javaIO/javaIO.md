Java I/O 大概可以分为以下几类：

*	磁盘操作：File
*	字节操作：InputStream和OutputStream
*	字符操作：Reader和Writer
*	对象操作：Serializable
*	网络操作：Socket
*	新的输入/输出：NIO

## 磁盘操作

File类可以用于表示文件和目录的信息，但是它不表示文件的内容

***

## 字节操作

### 实现文件复制

使用FileInputStream

### 装饰者模式

以InputStream为例：

*	InputStream 是抽象组件
*	FileInputStream 是 InputStream 的子类，属于具体组件，提供了字节流的输入操作
*	FilterInputStream 属于抽象装饰者，装饰者用于装饰组件，为组件提供额外的功能。例如 BufferedInputStream 为 FileInputStream 提供缓存的功能

***

## 字符操作

### 编解码

*	InputStreamReader：实现从字节流解码成字符流
*	OutPutStreamWriter：实现字符流编码成字节流

***

## 对象操作

*	序列化
	*	序列化`ObjectOutputStream.writeObject()`

	*	反序列化`ObjectInputStream.readObject()`

Serializable：序列化的类都需要实现此接口，只是一个类，没有任何方法需要实现。但是不实现的话会报错
transient：使一些属性不被序列化

***

## 网络操作

*	InetAddress：IP地址

*	URL：同意资源定位符（可以直接从url读取字节流数据）

*	Sockets：使用TCP协议实现网络服务

	*	ServerSocket：服务器端类
	*	Socket：客户端类

	>客户端和服务器端通过InputStream和OutPutStream进行输入输出

![Server和Client通过Socket通信.png](.\Server和Client通过Socket通信.png)

*	Datagram：使用UDP协议实现网络服务

    *	DatagramSocket：通信类
    *	DatagramPacket：数据包类

***

## NIO

一种同步非阻塞的I/O模型。每个客户端对应一个channel，客户端将请求发送到channel，然后一个selector时刻轮询每个channel，有事件的就读取出来，存入请求队列中，然后通过线程池进行处理

*	BIO（同步阻塞IO）
	传统的IO。如果没有获得数据，就会一直阻塞。获得数据之后，会返回数据。对于每个客户端连接都会在服务器端创建一个线程来服务

*	NIO（同步非阻塞IO）
	在有数据时会将数据读到内存（buffer），然后传送给线程处理（可能会阻塞在，这里需要轮询，channel中有事件的启动一个线程处理）。没有数据则立即返回0，永远不会阻塞

*	AIO（异步非阻塞IO）
	在NIO的基础之上，在数据向内存传送的过程中，也是使用异步的方式，不会阻塞。有请求来，通知操作系统去处理，完全不阻塞。操作系统处理完之后，通过回调函数通知结果

### NIO核心组件

#### 1、通道(channels)

通道（channel）是对一般IO中流的模拟

channel和流的区别:

channel|流
:---:|:---:
channel可以读取和写入|流一般是单向的
channel可以异步读写|
向channel中的读写都要先通过buffer|

channel的类型：

*	Filechannel：针对文件的读写
*	datagramchannel：针对UDP的读写
*	Socketchannel：针对TCP的读写（客户端）
*	ServerSocketchannel:针对TCP的读写（服务端）

#### 2、缓冲区（buffers）

~~~mermaid
graph LR;
	CLient--data-->Buffer
	Buffer--data-->Channel1
    Channel1--data---Channel2
    Channel2--data-->Buffer2
    Buffer2-->Server
~~~

buffer和channel配合实现IO，buffer从channel中读取数据，向channel中写入数据
>也可以使用put方法向buffer中写数据

#### 3、选择器（selectors）

用于检查多个channel是否处于可读，可写的状态
实现了用一个线程管理多个读写的目标，避免了多线程上下文切换的开销

如何检查：

channel使用register方法注册到selector中，使用SelectorKey进行保存，保存的是一个channel和一个selector对象的注册关系

开始和停止：

*	wakeup（）
*	close（）

### 优化线程模型

NIO由之前的阻塞读写变成了单线程轮询事件。除了轮询过程中是阻塞的（必要），剩余的IO都是纯CPU操作，速度很快，没有必要开启多线程。线程数的降低减少了创建，切换和销毁线程的系统开销，提高了系统性能

单线程的处理IO的效率确实高。但是，现在的CPU都是多核。因此，多个IO线程无疑效率会更高

通过分析可知，服务器端，我们需要的线程主要是以下几类：

*	事件分发线程：单线程选择就绪的事件
*	IO处理线程：一般开启CPU核个线程就行
*	业务线程：根据实际情况来决定

客户端：
一般大的BIO通过连接池来实现，建立n个连接，当某个连接被占用时，还有其他连接可以使用。但是与服务器端相似，连接数受制于线程数，线程很贵，创建，切换，销毁都会销毁系统资源

BIO|NIO
--|--
面向流|面向缓存
阻塞|非阻塞
无|选择器


## 阻塞&非阻塞&同步&异步

**阻塞和非阻塞关注的是程序在等待IO时的状态**

*	阻塞：发出调用请求，在结果返回之前，处于挂起状态，什么都不做
*	非阻塞：发出调用请求，返回结果之前，该做什么做什么，可能会有轮询机制检查

**同步和异步关注的是通信机制的方式**

*	同步：发出调用请求，在调用结果返回之前，不会有任何返回
*	异步：发出调用请求，立即返回，但是不会有调用结果