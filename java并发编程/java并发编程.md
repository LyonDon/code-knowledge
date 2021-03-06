[0.简介](#0)

[0.1中断](#0.1)

[1.线程join yeild wait sleep的区别](#1)

[2.守护线程](#2)

[3.线程的启动与终止](#3)

[3.1.线程封闭](#3.1)

[3.2.创建线程](#3.2)

[4.volatile关键字](#4)

[5.synchronized关键字](#5)

[6.双重锁检查与延迟初始化](#6)

[7.线程间通信](#7)

[8.线程池，连接池和对象池](#8)

[9.ReentrantLock（重入锁）](#9)

[10.ReentrantReadWriteLock（读写锁）](#10)

[11.Condition接口](#11)

[12.ConcurrentHashMap](#12)

[13.ConcurrentLinkedQueue](#13)

[14.阻塞队列](#14)

[15.Fork&Join框架](#15)

[16.工作窃取算法](#16)

[17.原子操作类型](#17)

[18.CAS算法](#18)

[19.CountDownLatch](#19)

[20.同步屏障(CyclicBarrier)](#20)

[21.Semaphore(信号量)](#21)

[22.Exchanger](#22)

[23.线程池](#23)

[24.Executor框架](#24)

[25.进程通信与线程通信](#25)


## <h2 id='0'>简介</h2>

*	线程的优势：

	*	发挥多处理器的能力
		*	单处理器中，一个处理器同时只能执行一个线程，但是如果是多线程的话，就可以同时运行。即便是单处理器，也可以在等待IO的时候运行其他线程

	*	建模的简单性
	*	异步事件的简化处理

	*	响应更灵敏的用户界面

*	线程的问题：

	*	安全性问题：数据的干扰等

	*	活跃性问题：死锁等
	*	性能问题：频繁的上下文切换等	

***

<h2 id='0.1'>中断</h2>

对于中断的处理：

*	传递InterruptedException

*	恢复中断

***

## <h2 id='1'>线程join yeild wait sleep的区别</h2>

*	sleep：thread的方法，会让线程进入休眠状态并释放CPU，但是不会释放锁

*	yeild：让出cpu调度，相当于通知就绪态中的其他线程。我已经执行完了，可以让其他具有相同优先级的线程去使用cpu了，但是没有实际的机制保障

*	wait:执行该方法的线程会释放锁，相当于将线程放入锁池中去竞争锁

*	join:一种特殊的wait,调用join即使一个线程阻塞并等待另一个线程执行完成。即如果一个线程A调用了thread.join()方法，则A需要等待thread执行完成才能从join()方法中返回


**join的逻辑和“等待/通知”很相似,都是加锁，循环，终止。前导线程终止时，调用自身的notifyAll方法，通知处于等待状态的线程**

wait，notify，notifyAll

![线程状态转换图.png](.\线程状态转换图.png)

***

## <h2 id='2'>守护线程</h2>

daemon线程：服务于用户（普通）线程，当JVM中只剩下守护线程时，JVM关闭

*	守护线程的典型应用场景是GC
*	setDaemon（）必须在start（）之前，否则会出现IllegalThreadException异常
*	java线程池会将守护线程转化为用户线程

***

## <h2 id='3'>线程的启动与终止</h2>
*	线程的启动：通过start（）方法实现，start方法就是将当前线程同步告知Java虚拟机
*	线程的终止：

	*	中断：中断是一种异常的终止线程的情况，通过中断标志位来实现
	*	exit（）：可以使线程正常结束
	*	stop（）：是比较危险的方法，会导致线程释放所有锁，包括子线程的锁

***


##	<h2 id='3.1'>线程封闭</h2>

表示线程内的数据完全不共享，也就不需要同步

*	Ad-hoc线程封闭（脆弱）

	>维护线程封闭性的职责完全由程序来完成

*	栈封闭
	>在栈封闭中，只有局部变量可以访问对象

*	ThreadLocal类

	>使线程中的某个值与保存该值的变量关联起来

`部分的线程安全，不能保证整体的线程安全`


***

## <h2 id='3.2'>创建线程</h2>

创建线程的三种方式：

*	实现Runnable接口
*	实现Callable接口
*	继承Thread类

~~~java
//使用Runnable实现创建一个线程
public class MyRunnable implements Runnable{

	@Override
	public void run() {

	}

	public static void main(String[] args) {
		MyRunnable runnable=new MyRunnable();
		Thread t=new Thread(runnable);
		t.start();
	}
}
~~~
~~~java
//使用Callable创建一个线程
class MyCallable implements Callable<Integer>{

	@Override
	public Integer call() throws Exception {
		return null;
	}

	public static void main(String[] args) {
		MyCallable callable=new MyCallable();
		//使用FutureTask对Callable的返回值进行封装
		FutureTask<Integer> task=new FutureTask<Integer>(callable);
		Thread thread=new Thread(task);
		thread.start();
	}
}
~~~

~~~java
//使用继承Thread的方式创建一个线程
class MyThread extends Thread{
	public void run() {
	}

	public static void main(String[] args) {
		MyThread thread=new MyThread();
		thread.start();
	}
}
~~~

实现接口vs继承Thread类

实现接口的方式会更好：

*	java不支持多继承，实现接口可以多个
*	不需要继承整个Thread类，开销太大

无限制创建线程的不足：

*	线程的生命周期开销非常高
*	资源消耗

## <h2 id='4'>volatile关键字</h2>

如果一个字段被声明为volatile类型的话，则JMM保证其在不同的线程下看到的这个变量时一致的

volatile变量的两大特性

*	保证此变量对于所有线程的可见性
*	禁止指令重排序优化（实现有序性）

	*	底层实现：指令重排无法越过内存屏障

**但是,volatile并不保证原子性操作。这使得虽然volatile变量不存在一致性问题，但是在并发环境下依然不安全**

**通过LOCK指令实现，访问volatile变量时不会执行加锁操作**

*	LOCK可以在一个线程对于变量进行修改时，将其值写入到系统缓存中，同时使其他线程种缓存该变量地址的数据无效

*	每个线程都有一个本地缓存（JMM抽象概念），同时系统还有一个主内存（系统缓存）（共享内存）

#### Volatile的局限性

*	volatile的语义不足以确保递增操作的原子性（即：确保可见性但是不确保原子性）

#### volatile变量规则

*	仅当一个变量参与到包含其他状态变量的不变性条件时，才可以声明为volatile类型

***

**发布与逸出**

>发布：使域内的对象能够在域外使用
>逸出：不应该发布的对象被发布

**不可变性**
>不变性：满足同步的另一种方法是使用不可变对象，不可变对象从头到尾是不能被修改的
>
>只有满足以下条件的对象才能称为不可变对象
>>1.对象创建后就不能修改
>>2.对象的所有域都是final类型
>>3.对象是正确创建的（创建过程中，this引用没有逸出）

>final域
>>final域的状态是不可修改的，而且可以保证初始化的安全性。
>>除非需要某个域是可变的，否则应该在声明时就加上final

***

## <h2 id='5'>synchronized关键字</h2>

>通过 “ 一个变量在同一时刻只允许一个线程对其进行lock操作 ”来实现有序性

synchronized关键字的实现是通过monitorenter和monitorexit指令来实现的


![synchronized.png](.\synchronized.png)


Synchronized|Lock
:--:|:--:
是java内置的关键字|是接口
在代码块结束运行之后，或者中断之后就会自动释放锁|需要在finally块中手动释放锁
获取锁的时候，若失败则会一直等待|获取锁的方式较多：lock（），trylock（）等
无法中断|通过lockInterruptibly（）提供非阻塞的锁
非公平锁|非公平锁，但是可以通过fair参数指定为公平锁

**tips**：私有的锁对象可以将所封装起来，使得客户代码不能轻易的获得锁。但是客户代码可以通过共有方法获得锁，这样的话容易产生活跃性问题
>活跃性：并发程序能及时执行的能力


***

##	<h2 id='6'>双重锁检查与延迟初始化</h2>
对象的初始化不一定是在其创建的同时完成的，延迟初始化是为降低系统启动时间，初始化所消耗的时间。只有在真正需要的时候才对其进行加载

双重锁检查：在延迟初始化过程中，实际的初始化语句（在内存中会被看做三行代码，这里可能存在重排序），即A线程先将instance指向某一内存地址，但是还未初始化。这时，B线程检查（instance==null？），由于A线程的操作，B线程检查为假，则访问instance引用的对象。然而，这时的instance还未被初始化！

***

## <h2 id='7'>线程间通信</h2>

*通过共享内存实现*

*	等待\通知机制
	*	等待者：获取对象锁，判断条件（为真，执行逻辑；为假，调用wait（），等待notify（）（这里还要检查条件））
	*	通知者：获取对象锁，改变条件，调用notify（）通知等待者

*	管道
	*	PipeWriter
	*	PipeReader
***

## <h2 id='8'>线程池，连接池与对象池</h2>
**线程池**

*	主要是因为thread的创建与撤销需要占用系统内存。java ee服务器在启动时，会创建许多可供使用的线程，需要的时候直接取用就行。处理过程中将任务添加到队列，然后在创建线程后自动启动这些任务

**连接池**

*	主要是数据库创建连接需要消耗时间，为了提高获取数据库数据的效率。一旦数据库连接建立后，不同的访问请求可以复用这些已经建立的数据库连接
>对于数据库的操作必须要经历数据库连接，打开数据库，存取数据，关闭连接等几个操作

**对象池**

*	主要用于服务器端的开发，降低创建每个对象的系统开销，提高服务器性能。创建一个对象池，将一些对象缓存在这个对象池中，如果需要使用，则从对象池中取出。用完之后扔回对象池

***

## <h2 id='9'>ReentrantLock（重入锁）</h2>
支持重入的锁，就是可以被一个线程重复多次获取的锁。
不同于synchronized的隐式重入，ReentrantLock是在调用Lock（）方法时，已经获取锁的线程能够再次调用Lock（）方法而不被阻塞

*	公平锁：先申请先获取的锁（防止饥饿情况的发生）
*	非公平锁：反之

比较：

|ReentrantLock|synchronized
--|--|--
锁的实现|JDK实现|JVM实现
性能|大致相同（由于java8中对于synchronized的优化，引入了自旋锁等概念）
等待可中断|可以|不能
公平锁|多数公平，可以不公平|非公平
锁绑定多个条件|可以绑定多个Condition对象

>自旋锁：是一个线程在请求共享资源的锁时进入忙循环一段时间，如果在这段时间内能够获得锁，就可以避免进入阻塞状态
>
>偏向锁：就是偏向于第一个获取锁的线程。当此线程获取锁，以后该线程进入对应的同步块就不需要再进行同步操作。偏向锁状态的解除发生在其他线程竞争锁时

***

## <h2 id='10'>ReentrantReadWriteLock（读写锁）</h2>
维护了一对锁，读锁和写锁

使用int型来维护读写的状态：**高16位表示读，低16位表示写**
多数情况下读操作是多于写操作的，读写锁会优于排他锁，提高吞吐量和并发性

***

## <h2 id='11'>Condition接口</h2>

*	Object的监视器方法与Synchronized关键字配合，实现等待\通知模式
*	Condition接口通过与Lock配合实现等待\通知模式
	*	具体来说，是用lock（）先获得锁，然后通过await（），signal（）的方式，类似于synchronized的wait（），notify（）实现等待\通知。但是await（）可以释放锁

**Condition实例的获取是通过绑定lock，调用lock（）方法实现的**

>condition实例只是一些普通的对象，其也可以作为synchronized的目标，也可以调用自身的wait，notify方法。在使用condition实例的时候，要注意避免这种使用方式


***
## <h2 id='12'>ConcurrentHashMap</h2>

**HashMap和HashTable在单线程条件下一样的，只有在多线程条件下，HashTable线程安全，因为使用了Synchronized关键字来实现，但是效率很低。HashMap在多线程的情况下entry链表可能会形成环形的结构，进而产生死锁**

这时就可以使用ConcurrentHashMap，通过将容器内的数据分段，为每一段数据分配一个锁，这样在访问不同段数据的时候，其他的段就不会被影响
*	使用Segment分段锁，要点在于定位到Segment的数组的位置,这里使用的是再散列法


![ConcurrentHashMap结构.png](.\ConcurrentHashMap结构.png)

[ConcurrentHashMap结构](https://github.com/LyonDon/code-knowledge/blob/master/Photo/ConcurrentHashMap%E7%BB%93%E6%9E%84.png)
**操作**

*	get():区别于一般的get（）在于，不需要加锁，因为将count，size等都定义为volatile型的（happens-before原则）

	>happens-before：如果一个操作happens-before另一个操作，则其执行结果对另一个操作可见，且其执行顺序在另一个操作之前;若两个操作之间存在happens-before关系，则两者之间可以重排序，只要不影响happens-before规则即可

*	时间的先后顺序和happens-before原则没有太大的关系，衡量并发安全问题时以happens-before原则为基准

*	put()：扩容问题

	*	只对单个segment进行扩容，而不是像HashMap一样对整个容器扩容。方法是：在插入元素之前，判断Segment中的HashEntry数组是否达到容量。若是达到的话，就创建一个两倍于当前容量的数组，使用再散列法将元素插入进去

*	size():两次判断是否需要加锁，若是在统计过程中，容器的count发生变化，则加锁

### Java8中对ConcurrentHashMap的改进

*	抛弃Segment，使用transient volatile HashTable<K,V>[] table来保存数据，通过数组上的每个元素实现对数据的加锁

*	将Java7中的 数组+单向链表 的是存储方式改为 数组+单项链表+红黑树 的方式，主要作用是当表长度较长时，使用红黑树可以降低查找的时间复杂度，由O（n）降为O（logn）

***

## <h2 id='13'>ConcurrentLinkedQueue</h2>

*写操作开销普遍大于读操作*

*	head节点
*	tail节点（并不总是尾节点）

**入队（通过CAS算法）**
多线程情况下，一个线程获取tail进行入队，但这时可能会有其他线程出现插队，需要阻塞之前的入队操作

1.	定位尾节点
2.	将尾节点的next指向入队节点

***

## <h2 id='14'>阻塞队列</h2>
支持两种附加操作的队列

*	队列空时阻塞元素的获取
*	队列满时阻塞元素的添加

***
## <h2 id='15'>Fork&Join框架</h2>

1.	将大任务通过fork分解为子任务
2.	将子任务通过join合并为大任务的结果

**实现原理**
通过doJoin（）方法得到当前的任务状态来判断返回的结果

*	已完成（NORMAL）
*	被取消（CANCELLED）
*	信号（SIGNAL）
*	出现异常（EXCEPTIONAL）

判断当前任务状态，若为已完成则直接返回任务执行结果。否则就从任务队列中获取任务并执行。若出现异常则将任务状态置为EXCEPTIONAL

***

## <h2 id='16'>工作窃取算法</h2>

线程执行完任务后，为了避免空闲，会从其他线程获取任务来执行。使用双端队列（Deque&BlockingQeque）来实现，避免冲突。

*	提高了线程并发的执行效率
*	但是存在线程中只有一个资源时出现冲突，同时创建多个线程和队列会消耗系统资源
***

## <h2 id='17'>原子操作类型</h2>

底层通过volatile和CAS实现。volatile保证可见性和禁止指令重排序，CAS保证原子性

*	原子更新类
	*	AtomicInteger
    *	AtomicLong
    *	AtomicBoolean
*	原子更新数组（复制了一个数组在Atomic……中，更改其的值不影响原始数组的元素）
	*	AtomicIntegerArray
	*	AtomicLongArray
	*	AtomicReferenceArray
*	原子更新引用类型（处过基本数据类型的其他类型，包括对象和数组等）
	*	AtomicReference
*	原子更新字段类
	*	AtomicIntegerFieldUpdater
	*	AtomicLongFieldUpdater
	*	AtomicStampedReference

***

## <h2 id='18'>CAS算法</h2>
是一种无锁算法，比较原始值是否等于预期值，若相等，则更新为更新值。否则重试或者放弃
~~~java
compareAndSwap(int usual,int want,int update)
compareAndSet(int usual,int want,int update)
~~~
这一类的算法

存在问题：

*	若果CAS不是基于内核的原子操作的话，则可能出现ABA问题：就是更新值的过程中，值被其他线程更改为更新的值，则此线程无法判断新值是不是CAS操作得到的。（可以通过设置版本号来处理，在变量前面追加版本号）。JDK1.5中提供了一个AtomicStampedReference来解决ABA，询问预期和标志位是否相同

*	循环时间开销大：当CAS失败时，会一直进行尝试。若CAS一直不成功，可能会给CPU带来很大的开销

*	只能保证一个共享变量的原子操作：多个变量共享操作时，CAS无法保证原子性，这时可以用锁来保证原子性

***

## <h2 id='19'>CountDownLatch</h2>
支持一个或多个线程等待其他线程完成工作

~~~java
public CountdownLatch(int count)

public void await() throws InterruptedException { };\
//调用await()方法的线程会被挂起，它会等待直到count值为0才继续执行

public boolean await(long timeout, TimeUnit unit) throws InterruptedException { };  
//和await()类似，只不过等待一定的时间后count值还没变为0的话就会继续执行

public void countDown() { };
//将count值减1
~~~

~~~java
public class CountDownLatchTest {
	public static void main(String[] args) throws InterruptedException {
		final int totalThread = 10;

		CountDownLatch countDownLatch = new CountDownLatch(totalThread);
		ExecutorService service = Executors.newCachedThreadPool();
		for (int i = 0; i < totalThread; i++) {
			service.execute(() -> {

					System.out.println("run");
					
					countDownLatch.countDown();
				});
			Thread.sleep(1000);
		}
		countDownLatch.await();
		System.out.println("end");
		service.shutdown();
	}
}
~~~


>	通过join（）实现。原理是不停的查询join线程是否存活，若是的话就让当前线程一直等待下去

***

## <h2 id='20'>同步屏障（CyclicBarrier）</h2>
设置一个屏障，同时接受n个线程到达屏障并阻塞，只有最后一个线程到达的时候，屏障才会打开，被阻塞的线程才能继续运行

构造方法：

~~~java
public CyclicBarrier(int parties)
public CyclicBarrier(int parties，Runnable barrierApplication)//后一个参数是屏障到达后的执行线程
~~~

CountDownLatch|CyclicBarrier
--|--
计数器只能使用一次|支持计数器的重置，可以多次使用

**CountdownLatch相当于是许多个线程都在执行，一个执行完了，等另一个。最后大家都执行完了，再继续接下来的操作**

**CyclicBarrier相当于是多个线程正在执行，这时有一个屏障，到达屏障的线程都要阻塞。等到最后一个线程到达后，屏障才会打开，被阻塞的线程才能继续执行**

***

## <h2 id='21'>Semaphore（信号量）</h2>

控制同时访问资源的线程数量

通过acquire（）和release（）两个方法获取和释放信号量

#### 实现

*	公平策略

~~~java
protected int tryAcquireShared(int acquires){
	for(;;){
    	if(hasQueuedPredecessors()){
        	return -1;
        }
        int available=getState();
        int remaining=available-acquires;
        if(remaining<0||compareAndSetState(available,remaining)){
        	return remaining;
        }
    }
}
~~~

~~~java
protected int tryReleaseShared(int releases){
	for(;;){
    	int current=getState();
        int next=current+releases;
        if(next<current){
        	throw execption;
        }
        if(compareAndSetState(current,next)){
        	return remaining;
        }
    }
}
~~~

*	非公平策略

~~~java
final int nonfairTryAcquireShared(int acquires){
	for(;;){
    	int available=getState();
        int remaining=available-acquires;
         if(remaining<0||compareAndSetState(available,remaining)){
        	return remaining;
        }
    }
}
~~~

~~~java
protected int tryReleaseShared(int releases){
	for(;;){
    	int current=getState();
        int next=current+releases;
        if(next<current){
        	throw execption;
        }
        if(compareAndSetState(current,next)){
        	return remaining;
        }
    }
}
~~~

***
## <h2 id='22'>Exchanger</h2>
提供一个同步点供**两个**线程进行交换，若线程A先执行exchange（），则其会等待第二个线程执行exchange（）方法。当两个线程都到达同步点时，数据交换发生

Exchanger在使用的时候应该配合Atomic实现原子化

***

## <h2 id='23'>线程池</h2>
线程池执行任务的步骤

~~~java
if（当前线程num<核心线程）
	创建新线程执行任务（需要获取全局锁）
else
	查看阻塞队列中的线程数
    if 不会溢出
    	将任务加入阻塞队列
    else
    	查看最大线程数是否已满
        if 未满
        	创建新线程（需要获取全局锁）
        else
        	调用线程创建失败执行函数
~~~

向线程池提交任务：

*	submit（）：无返回值的任务
*	execute（）：需要返回值的任务（返回一个future对象）

关闭线程池：

*	shutdown（）& shutdownNow（）

***

## <h2 id='24'>Executor框架</h2>

#### 两级调度模型

*	上层：由Executor将任务分配到各个线程

*	下层：有系统内核将线程分配到CPU执行

#### Executor框架成员

##### ThreadPoolExecutor（由工厂类Executors创建）

*	SingleThreadExecutor:创建使用单个线程的Executor，适用于线程的顺序执行（核心线程池容量为一个线程，使用无界队列，LinkedBlockingQueue）

*	FixedThreadPool：创建使用固定线程数的Executor，适用于负载较重的服务器（使用无界队列，LinkedBlockingQueue）

*	CachedThreadPool：创建根据需要创建Thread的Pool，适用于负载较轻，随时创建线程的情况（核心线程池容量为0，使用Synchronized Queue(容量为0的阻塞队列，使用空闲线程等待时间后poll来执行),最大线程数为Integer.MAX_VALUE）

##### ScheduledThreadPoolExecutor（由工厂类Executors创建）

*	ScheduledThreadPoolExecutor：具有多个线程的Executor
*	SingleThreadScheduledExecutor：具有单个线程的Executor
*	实现


![ScheduledThreadPoolExecutor1.png](.\ScheduledThreadPoolExecutor1.png)

*	DelayQueue的take（）实现（通过priority队列实现）
	
    	1.	获取Lock
    	2.	获取周期任务
    	3.	释放Lock

*	DelayQueue的add（）实现（通过priority队列实现）
    
    	1.	获取Lock
    	2.	添加任务
    	3.	释放Lock

[Executor,Executors和ExecutorService的区别](https://yemengying.com/2017/03/17/difference-between-executor-executorService/)

Executor|ExecutorService|Executors
--|--|--
java线程池的核心接口，用来并发执行提交的任务|Executor的子接口，提供了异步执行（Future）和关闭线程池的方法（shutdown（））|创建线程池的工厂类
提供execute方法用来提交任务|提供submit方法用来提交任务
execute方法没有返回值|submit方法返回一个Future对象
不能取消任务|可是使用Future.cancel方法取消任务
没有关闭线程池的方法|使用shutdown可以关闭线程池

##### Futrue接口

-	用来判断任务是否完成
-	获取任务结果
-	必要时还可以中断任务

在Future中声明了五个方法：
~~~java
boolean cancel(boolean mayInteruptIfRunning);
boolean isCanceled();
boolean isDone();
V get() throws Exception:
V get(long time,TimeUnit unit) thrwows Exception
~~~

*	用来表示一步计算的结果，当将Callable和Runnable的实现类提交给ThreadPoolExecutor时，返回的是一个FutureTask类型的对象（实现Future接口）

*	FutureTask是Future的唯一实现类

~~~java
public class FutureTask<V> implements RunnableFuture<V>{}
public interface RunnableFuture implements Runnable，Future<V>(){
	void run();
}
//FutureTask提供了两个构造器
public FutureTask(Callable<V> callable){}
public FutureTask(Runnable runnable,V result){}
~~~



##### Runnable&Callable

JDK5之后 任务分为两类，一种是实现了Runnable的类，另一种是实现了Callable的类。两个都可以被ExecutorService执行。区别在于，Runnable没有返回值，而Callable有返回值

submit（Callable x）|execute（Runnable x）
:--:|:--:
实现Callable接口|实现Runnable接口
返回一个Future对象，判断任务的执行情况|无返回值，无法判断执行情况

可以使用Executors工厂类将Runnable包装为Callable(以下为相关实现API)
~~~java
    public static Callable<Object> callable(Runnable task)
~~~
将一个Runnable和一个待返回结果包装为Callable
~~~java
public static Callable<Object> callable(Runnable task,FutureTask result)
~~~

## <h2 id='25'>进程通信&线程通信&线程同步</h2>

#### 进程通信

1.	管道（用pipe函数实现，在内存中开辟出一块缓冲区进行进程间通信，这块缓冲区称为管道）

	*	pipe构造函数（接受一个长度为2的数组，pipe[0]和pipe[1]分别作为父子进程的读端和写端）
	*	单向通信
	*	通过血缘关系（父子进程）
	*	生命周期随进程
	*	依赖于文件系统（通过文件描述符）
	*	内部提供了同步机制

2.	信号量

	*	控制同时访问资源的线程数量
	*	只能进行等待和发送两种操作

3.	消息队列（与信号量配合使用实现进程同步）（类似命名管道）

	*	链表队列，进程间通信或者同一进程不同线程间的通信方式。消息队列本身是异步的，它允许接收者在发送者发出消息很长时间之后才从消息队列中取回消息。但是这样可能引发一个问题：即接受者需要轮询消息队列来获得消息，增加了系统开销

4.	共享内存
5.	套接字

#### 线程通信

*	等待\通知机制
*	共享内存
*	CountdownLatch
*	CyclicBarrier
*	信号量

#### 线程同步

*	synchronized同步方法
*	synchronized同步代码块
*	volatile实现线程同步
*	重入锁实现线程同步
*	局部变量实现线程同步
*	使用阻塞队列实现线程同步
*	使用原子变量实现线程同步