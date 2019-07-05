Java I/O 大概可以分为以下几类：

*	磁盘操作：File
*	字节操作：InputStream和OutputStream
*	字符操作：Reader和Writer
*	对象操作：Serializable
*	网络操作：Socket
*	新的输入/输出：NIO

## 磁盘操作

File类可以用于表示文件和目录的信息，但是它不表示文件的内容

## 字节操作

### 实现文件复制

使用FileInputStream

### 装饰者模式

以InputStream为例：

*	InputStream 是抽象组件
*	FileInputStream 是 InputStream 的子类，属于具体组件，提供了字节流的输入操作
*	FilterInputStream 属于抽象装饰者，装饰者用于装饰组件，为组件提供额外的功能。例如 BufferedInputStream 为 FileInputStream 提供缓存的功能

## 字符操作

### 编解码

*	InputStreamReader：实现从字节流解码成字符流
*	OutPutStreamWriter：实现字符流编码成字节流

## 对象操作

*	序列化
	*	序列化`ObjectOutputStream.writeObject()`

	*	反序列化`ObjectInputStream.readObject()`

Serializable：序列化的类都需要实现此接口，只是一个类，没有任何方法需要实现。但是不实现的话会报错
transient：使一些属性不被序列化

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

## NIO

一种同步非阻塞的I/O模型

*	BIO阻塞IO，传统的IO。如果没有获得数据，就会一直阻塞。获得数据之后，会返回数据

*	NIO在有数据时会将数据读到内存，然后传送给用户（可能会阻塞）。没有数据则立即返回0，永远不会阻塞

*	AIO最新的IO，在NIO的基础之上，在数据向内存传送的过程中，也是使用异步的方式，不会阻塞

### 通道和缓冲区

*通道和缓冲区是NIO中的核心对象*

*	通道（channel）是对一般IO中流的模拟。发送给通道或者从通道中读取都需要通过缓冲区



### 优化线程模型

NIO由之前的阻塞读写变成了单线程轮询事件。除了轮询过程中是阻塞的（必要），剩余的IO都是纯CPU操作，速度很快，没有必要开启多线程。线程数的降低减少了创建，切换和销毁线程的系统开销，提高了系统性能

单线程的处理IO的效率确实高。但是，现在的CPU都是多核。因此，多个IO线程无疑效率会更高

通过分析可知，服务器端，我们需要的线程主要是以下几类：

*	事件分发线程：单线程选择就绪的事件
*	IO处理线程：一般开启CPU核个线程就行
*	业务线程：根据实际情况来决定

客户端：
一般大的BIO通过连接池来实现，建立n个连接，当某个连接被占用时，还有其他连接可以使用。但是与服务器端相似，连接数受制于线程数，线程很贵，创建，切换，销毁都会销毁系统资源