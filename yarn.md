# yarn 是什么

集群的资源调度平台



### 运行时架构

作业的提交流程

### yarn调度器

目前，Hadoop作业调度器主要有三种：FIFO、容量（Capacity Scheduler）和公平（Fair Scheduler）



### Yarn 操作

#### 1. webUI
yarn web 界面 http://mycluster:8088

查看日志

#### 2. shell 


#### 3.restAPI

一、Hadoop入门
	1、常用端口号
	hadoop3.x 
		HDFS NameNode 内部通常端口：8020/9000/9820
		HDFS NameNode 对用户的查询端口：9870
		Yarn查看任务运行情况的：8088
		历史服务器：19888
	hadoop2.x 
		HDFS NameNode 内部通常端口：8020/9000
		HDFS NameNode 对用户的查询端口：50070
		Yarn查看任务运行情况的：8088
		历史服务器：19888
	2、常用的配置文件
	3.x core-site.xml  hdfs-site.xml  yarn-site.xml  mapred-site.xml workers
	2.x core-site.xml  hdfs-site.xml  yarn-site.xml  mapred-site.xml slaves
	
二、HDFS
	1、HDFS文件块大小（面试重点）
		硬盘读写速度
		在企业中  一般128m（中小公司）   256m （大公司）
	2、HDFS的Shell操作（开发重点）
	3、HDFS的读写流程（面试重点）
三、Map Reduce
	1、InputFormat
		1）默认的是TextInputformat  kv  key偏移量，v :一行内容
		2）处理小文件CombineTextInputFormat 把多个文件合并到一起统一切片
	2、Mapper 
		setup()初始化；  map()用户的业务逻辑； clearup() 关闭资源；
	3、分区
		默认分区HashPartitioner ，默认按照key的hash值%numreducetask个数
		自定义分区
	4、排序
		1）部分排序  每个输出的文件内部有序。
		2）全排序：  一个reduce ,对所有数据大排序。
		3）二次排序：  自定义排序范畴， 实现 writableCompare接口， 重写compareTo方法
			总流量倒序  按照上行流量 正序
	5、Combiner 
		前提：不影响最终的业务逻辑（求和 没问题   求平均值）
		提前聚合map  => 解决数据倾斜的一个方法
	6、Reducer
		用户的业务逻辑；
		setup()初始化；reduce()用户的业务逻辑； clearup() 关闭资源；
	7、OutputFormat
		1）默认TextOutputFormat  按行输出到文件
		2）自定义
四、Yarn
	1、Yarn的工作机制（面试题）
		
	2、Yarn的调度器
		1）FIFO/容量/公平
		2）apache 默认调度器  容量； CDH默认调度器 公平
		3）公平/容量默认一个default ，需要创建多队列
		4）中小企业：hive  spark flink  mr  
		5）中大企业：业务模块：登录/注册/购物车/营销
		6）好处：解耦  降低风险  11.11  6.18  降级使用
		7）每个调度器特点：
			相同点：支持多队列，可以借资源，支持多用户
			不同点：容量调度器：优先满足先进来的任务执行
					公平调度器，在队列里面的任务公平享有队列资源
		8）生产环境怎么选：
			中小企业，对并发度要求不高，选择容量
			中大企业，对并发度要求比较高，选择公平。
	3、开发需要重点掌握：
		1）队列运行原理	
		2）Yarn常用命令
		3）核心参数配置
		4）配置容量调度器和公平调度器。
		5）tool接口使用。















		

	
	
	
	
	
	
	
	
	
	
	