### 转换表的引擎

*	alter table
*	导入与导出
*	创建于查询（create 和 select）



## MySQL基准测试

针对系统状态的一种压力测试，用于掌握系统的行为

*区别于真实压力测试，基准测试的强度更低，不想真实情况下的数据量那么大，变化多端*

*	系统测试
*	组件测试

测试指标：

*	吞吐量
*	响应时间或者延迟
*	并发性
*	可扩展性

*基准测试应运行足够长的时间*

*每一次测试中，修改的参数应该尽可能的少*

测试类型：

*	集成式测试
	*	集成式测试工具

		*	ab
		*	http_load
		*	JMeter

*	单组件式测试
	*	单组件式测试工具

		*	sysbench（一款多线程系统压测工具）等

_ _ _

### BenchMark函数

用来测试某条指令的执行速度
_ _ _

### sysbench

sysbench是一个模块化，跨平台。多线程的基准测试工具。可以执行多种类型的基准测试，它不仅设计用来测试数据库的性能，也可以测试运行数据库的服务器的性能

主要包括以下几种方式的测试：

*	CPU性能
*	file I/O性能
*	程序调度性能
*	线程性能
*	数据库性能（OLPT测试）

#### sysbench的CPU基准测试
~~~
sysbench --test=cpu （参数）
~~~

#### sysbench的文件I/O基准测试
~~~
sysbench --test=fileio
~~~

*测试完成后，运行清除操作删除第一步生成的测试文件*
~~~
sysbench --test=fileio （参数） cleanup
~~~

### sysbench的OLTP基准测试

**模拟一个简单的事务处理系统的工作负载**

_ _ _

### dbt2 TPC-C测试

>TPC-C benchmark是Transaction Processing Council制定测试数据库benchmark标准，它是基于批发供应商（也称之为company）的模型
>
>TPC-C被广泛用于CS环境下，用于测试客户端服务器组成系统的整体性能
>
>TPC-C的值可以反映整个系统的性价比，TPCC测试系统每分钟处理的任务数,单位为tpm,(transactions per minute)。系统的总体价格(单位为美元)除以TPCC值,就可以衡量出系统的性价比,系统的性价比值越小,系统的性价比越好

****
