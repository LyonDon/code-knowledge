
[TOC]

## Redis是什么

并不是简单的持久化key-value存储的存储器。而是涉及到五种数据结构。只有一种是键值对的结构，通过这些实现数据的高速存取的存储方式

*注意：redis不是一种普遍使用的存储概念，只适用于部分情况*

### 数据库

redis中也有数据库的概念，不同于传统意义上的数据库，redis数据库使用数字来为数据库编号，默认数据库编号为0，通过select语句能实现数据库的切换，如select1：切换到1号数据库。

### 命令，关键字和值（command，key and value）

*	key：用来标识数据块的一段字符串
*	value：关键字的实际值，redis并不关心它到底是什么

key和value是redis的最基本的概念

*key是redis的一切，value什么都不是（也可以理解为可以是任何东西），redis不能通过值来查询*  

### 查询

### 存储器和持久化

redis是一种持久化的存储器内存储，会将所有数据存储在存储器中，并每隔一段时间到磁盘中建立数据库的快照（snapshot）；除此之外，redis还可以运行在附加模式。即单一关键字的改变，redis可能会在磁盘中进行append-only操作

### 小结

*	redis是一种超快速的存取方式，（毕竟所有数据都在存储器中），一般可以实现每秒数万到数十万次的操作  

*	使用redis时，不同于sql程序尽量避免与数据库的频繁交互（为了节省时间），与redis服务器的频繁交互显得微不足道，相比起redis结构节省的时间来说

***

## 数据结构(5种)

### 字符串
~~~
get
set
del
strlen
getrange
……
~~~

字符串最常用的案例是存储对象和计数，由于通过关键字获取值的速度很快，所以字符串常被用来缓存数据

### 散列（Hashes）
~~~
hget
hset
hdel
hkeys
hgetall
……
~~~

散列数据结构和字符串数据结构比较相似，但是多了一个域，关键字为hget，hset
若是同时添加获取多个域；关键字为hmget，hmset

相比于字符串，散列可以更为细致的对用户进行操作，而不只是序列化一个对象。例如，可以添加，更新删除部分用户片段

*注意：Hashes和字符串两者之间的关键字是不通用的，比如set建立的对象，无法用hget来获取*

### 列表
~~~
lpush
ltrim
……
~~~
ltrim的具体构成是LTRIM Key start stop，即建立一个从start到stop长度的列表

*因为这里的key保存的是一个列表，理论上列表可以保存任意个值，这里就是将start到stop之外的值全部删除*

列表可以用来存储用户日志，过往活动等

### 集合
~~~
sadd
sismember
~~~
集合一般用来保存没有重复元素的数据，同时也提供了并集等相关操作

*	对集合进行操作的场合
*	需要对数据进行跟踪和标记的场合

### 分类集合
~~~
zadd
zcount
zrevrank
zrank
~~~

相比于普通集合数据结构，增加了标记（score）的概念，标记提供了排序和秩划分的功能

主要用于一些基于整数排序，且能以标记（score）来进行操作的

***

## 使用数据结构

### 大O表示法

O(1)<O(logn)<O(n)<O(log(N)+M)<O(N+M*log(M))

redis中，最大的时间复杂度是sort命令的O（N+M*log(M)）

### 仿多关键字查询

使用域作为二级索引，然后去引用单个用户对象

### 引用与索引

redis的值和值间的引用和索引的操作，如管理，更新，删除等，需要手动的进行。与关系型数据库一样，索引需要一定的额外开销，需要对额外的数据关系进行查找和维护。

### 数据交互和流水线

与服务器的频繁交互是redis的一种常见情况，比如sadd方法，可以同时添加好几个元素

流水线同样是redis的一大特征。一般情况下，客户端发送一个请求到服务器时，需要获得服务器的响应之后才能发送之后的请求。redis支持一次发送多个请求，而不需要等待redis的响应

### 事务

redis的每一条命令都具有原子性，因为redis是单线程运行的
但是可以同时运行多个redis客户端，这可能会造成错误

redis事务功能保证：

*	每组事务一定是按顺序执行的
*	事务中的操作是按原子方式执行的
*	事务中的命令要么全部执行，要么全部不执行

### 关键字反模式

keys命令：需要一个模式，然后查找匹配所有关键字

更多时候使用的是散列模式，比如：
		
        hset：bugs:1233 1"{id:1 account:1233 subject:'...'}"
		hget：bugs:1233 1
        hdel:bugs:1233 1
        ......
       

***

## 超越数据结构

### 使用期限（Expiration）

redis允许标记一个关键字的使用期限：

*	expire创建使用期限

*	ttl查询使用期限

*	persist删除使用期限

### 发布和订阅（Publication and Subscriptions）

redis支持发布和订阅模式

~~~
subscribe warning 可以订阅频道
publish warning “it‘s over 9000” 可以发布信息
unsubscribe warning 取消订阅
punsubscribe warning：*取消监听一组频道
~~~

### 监控和延迟日志（Monitor and Slow Log）

~~~
monitor命令：可以实时查看redis客户端的命令（慎用，因为除了调试和监控之外没有其他用处）
slowlog命令：可以查看执行时间超过一定毫秒数的命令，可以通过设置slowlog get length，来控制命令的个数。
			不过slowlog默认会检索之前存储的1024条命令
~~~

### 排序（Sort）

~~~
sadd users：book kee sna sadsa nil
sort users:book limit 0 3 desc alpha
//表示对set进行排序，字母表顺序，降序，限制元素为3个
~~~

***

## 管理

*	redis.conf文件配置
	[配置](https://github.com/antirez/redis/raw/2.4.6/redis.conf)
    通过修改url中的版本号获取相对应的版本配置文件

*	config set个别设置
*	config get显示一个设置值

### 验证

使用requirepass设置密码

使用rename-command CONFIG设置指令关键字替代值

### 大小限制
set list sortedsets 大小都是亿万级别

### 复制

*	可以在配置文件中设置slaveof关键字实现复制功能

*	可以将数据拷贝到不同的服务器上
*	复制没有自动故障恢复功能

### 备份文件

*	可以对快照进行处理

*	也可以是使用Slave实例进行处理

### 缩放和redis集群

通过分发关键字到多个redis实例中，实现真正的缩放

***
# 总结

*	优：redis体现了一种简易的数据处理方式，它剥离掉了复杂的处理逻辑和额外的数据结构，关注与对数据的操作层面，并且可以有效地使用于不同的系统之中

*	缺：对于一些异常和突发状况，redis缺乏有效的安全性和可用性机制

## Jedis

Redis官方推荐的java连接工具