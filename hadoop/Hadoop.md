
# 一、hadoop 是什么

1） hadoop 是一个由Apache基金会开发的分布式系统基础架构

2） 主要解决海量数据的存储核海量数据的分析计算问题

3） 广义上讲hadoop通常是一个更广泛的概念-- Hadoop生态圈

##### 起源于 三篇论文

GFS -> HDFS : 解决了一般的存储问题

[[Google-File-System中文版_1.0.pdf]]]_



MapReduce -> : 解决了计算问题

[[Google-MapReduce中文版_1.0.pdf]]_



BigTable -> : 表式的存储问题

[[Google-Bigtable中文版_1.0.pdf]]

# 二、Hadoop组成

MapReduce （计算）

Yarn （资源调度）

HDFS （数据存储）

## 2.1 组件

### 1. HDFS 

1） `NameNode(Nn)` : 存储文件的元数据 （索引）

元数据（文件名，文件目录结构，文件属性[生成时间、副本数、文件权限]，以及每个文件的块列表和块所在的 `DataNode`

2） DataNode (Dn) ： 本地文件系统存储文件快数据，以及块数据的校验和。

3） Secondary NameNode (2nn) : 用来监控`HDFS`状态的辅助后台程序，每隔一段时间获取HDFS的元数据的快照。

### 2. MapReduce 

运行时架构
MapTask
ReduceTask
MrAppmaster

### 3. Yarn

1） ResourceManager (RM) : 管理整个集群的资源

 1. 处理客户请求  
 1. 监控NodeManager  
 1. 启动或监控ApplicationMaster  
 1. 资源的分配和调度   

2） NodeManager （NM）: 管理单个节点的资源

1.  管理单个节点的资源
    
2.  处理来自ResourceManager的命令
    
3.  处理来自ApplicationMaster的命令
    

3） Application Master （AM） ：

1.  负责数据切分
    
2.  为应用程序申请资源并分配给内部的任务
    
3.  任务的监控与容错
    

4）Container

Container 是 yarn的资源抽象，他封装了某个节点的多个维度资源

## 2.1 Hadoop 目录

hadoop目录结构(只列举部分)
```
hadoop-3.1.3  
 |-- bin  
 |   |-- hadoop  
 |   |-- hdfs  
 |   |-- mapred  
 |   |-- yarn  
 |-- etc  
 |   `-- hadoop  
 |       |-- capacity-scheduler.xml  
 |       |-- core-site.xml  
 |       |-- hadoop-env.sh  
 |       |-- hdfs-site.xml  
 |       |-- httpfs-site.xml  
 |       |-- log4j.properties  
 |       |-- mapred-env.sh  
 |       |-- mapred-site.xml  
 |       |-- workers  
 |       |-- yarn-env.sh  
 |       `-- yarn-site.xml  
 |-- include  
 |-- lib  
 |   `-- native  
 |-- sbin  
 |   |-- hadoop-daemon.sh  
 |   |-- hadoop-daemons.sh  
 |   |-- start-all.sh  
 |   |-- start-dfs.sh  
 |   |-- start-yarn.sh  
 |   |-- stop-all.sh  
 |   |-- stop-balancer.sh  
 |   |-- stop-dfs.sh  
 |   |-- stop-secure-dns.sh  
 |   |-- stop-yarn.sh  
 |   |-- workers.sh  
 |   |-- yarn-daemon.sh  
 |   `-- yarn-daemons.sh  
 `-- share  
     |-- doc  
     |   `-- hadoop  
     `-- hadoop  
         |-- client  
         |-- common  
         |-- hdfs  
         |-- mapreduce  
         |-- tools  
         `-- yarn
```

**重要目录**

（1）bin目录：存放对Hadoop相关服务（hdfs，yarn，mapred）进行操作的脚本

（2）etc目录：Hadoop的配置文件目录，存放Hadoop的配置文件

（3）lib目录：存放Hadoop的本地库（对数据进行压缩解压缩功能） （linux 本地库文件，由于效率原因）

（4）sbin目录：存放启动或停止Hadoop相关服务的脚本

（5）share目录：存放Hadoop的依赖jar包、文档 （hadoop 的本体）

> bin : 可执行文件
> 
> sbin ： 可执行脚本
> 
> etc: 配置文件


1. 常用端口号

	hadoop3.x 
		HDFS NameNode 内部通常端口：8020/9000/9820
		HDFS NameNode 对用户的查询端口：9870
		Yarn查看任务运行情况的：8088
		历史服务器：19888
2. 常用的配置文件
 core-site.xml  hdfs-site.xml  yarn-site.xml  mapred-site.xml workers



高可用

[(64条消息) Hadoop的SecondaryNameNode和HA（高可用）区别_secondnamenode和ha_andyguan01_2的博客-CSDN博客](https://blog.csdn.net/andyguan01_2/article/details/88696239)