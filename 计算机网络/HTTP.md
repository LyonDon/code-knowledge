*HTTP是无状态协议*

HTTP状态码
|类别|原因短语
---|---|---
1XX|Information|接收的请求正在处理
2XX|Success|请求正常处理完毕
3XX|Redirection|需进行附加操作完成请求
4XX|Client Error|服务器无法处理请求
5XX|Server Error|服务器处理请求出错

*HTTP+加密+认证+完整性保护=HTTPS*

可以理解为HTTP加上SSL（安全套接层）或者TLS（传输层安全）
SSL和TLS在传输层对网路连接进行加密
SSL会比一般的HTTP通信慢2到10倍，主要原因在于通信慢以及消耗CPU和内存的资源


### HTML（超文本标记语言）
是构建web页面的主要语言
~~~html
//conference
<html>
<head>
<title>T公司研讨会介绍</title>
<body>
<h1>公司研讨会介绍</h1>
<ul>
	<li>研讨会编号：001
    	<ul>
        	<li>应用程序脆弱性诊断讲座</li>
        </ul>
    </li>
</ul>
</body>
</head>
</html>
~~~

### CSS（层叠样式表）
可以指定如何展现HTML内的各种元素，属于样式表标准之一

~~~CSS
.logo{
	paddding:20px;
    text_align:center;
}
~~~

### XML（可扩展标记语言）

一种按应用目标进行扩展的通用标记语言
与html一样，使用标签够成树形结构，可自定义扩展标签

~~~xml
<研讨会 编号=“001” 主题=“Web应用程序脆弱性诊断讲座”>
	<类别>安全</类别>
    <概要>为深入研讨Web应用脆弱性诊断的必要</概要>
</研讨会>
~~~

### RSS，Atom和FEED
[简单介绍](http://wenzhixin.net.cn/2013/11/08/rss_atom_feed_php)

### URI（统一资源标识符）与URL（统一资源定位符）
URI包括URL
URL是指浏览web页面时输入的网页地址
URI是由某个协议方案（30种左右）表示的资源的定位标识符

### 告知服务器意图的HTTP方法
*	GET：获取资源
*	POST传输实体主体
*	PUT：传输文件
*	HEAD：获得报文首部
*	DELETE：删除文件
*	TRACE：追踪路径
*	OPINIONS：询问支持的方法
*	CONNECT:要求用隧道协议连接代理
*	LINK：建立和资源之间的联系
*	UNLINK：断开连接关系

### 持久连接
只要任意一端没有明确提出断开连接，则保持TCP连接状态
*管线化：可以一次连续发送多个请求*

Cookie：客户端向服务器发送请求，服务器在响应中添加Cookie后返回响应。在接下来客户端要发送请求时，就在请求中添加Cookie后发送，服务器会对比之前的记录，得到之前的状态信息



### Session，Cookie，Cache对比
Session|Cookie|Cache
--|--|--
由服务器维持的一个服务端的存储空间，可以以Cookie或URL重写的方式来传递|由服务器创建，保存在客户端的存储空间，客户端不需要理解Cookie的内容，实现Session的一种机制|服务器端的缓存
抽象状态|实际存在|
维持一个会话的核心就是客户端的唯一标识，即 session id||

### Http与Https

**http是无状态协议，就是说http不会记录之前的连接状态，每次发送数据都是新的。https就相当于加上了SSL\TLS的http协议**

http|https
--|--
http以明文方式发送消息，不提供任何数据加密|https在http下加入了SSL\TLS安全套接字层（SecureSocketLayer）协议，依靠证书验证服务器的身份，并且为服务器与浏览器间的消息传递加密
80端口|443端口
连接简单，无状态|需要申请证书，加密传输，数据完整性验证

>TLS是SSL的升级版	

https优点：

*	能够验证服务器和客户端的有效性，避免发送数据到错误的接收方
*	使用http+SSL\TLS的方式，通过证书验证，加密传输，验证数据完整性的方式，增加了安全性，防止数据被篡改
*	虽然还存在中间人获取根授权机构或者加密算法进行破解的可能，但是大大提高了中间人攻击的成本

https缺点：

*	比较费时，相比起http的TCP三次握手，https由于SSL的引入，每次连接建立时，握手次数增加了9次
*	要钱，证书颁发机构。手动创建证书需要客户端认证

HTTP报文分为两种：

请求报文|响应报文
:--:|:--:
请求头|响应头
方法，URL和版本|版本，状态码和短语

