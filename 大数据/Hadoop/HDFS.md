# 一、概述

分布式文件系统，适用于一次写入，多次读出的场景

特点：多副本，处理大数据（文件多，文件大），但数据访问延迟高（秒级），不支持并发写入和文件的随机修改

architecture: master-slave 架构

1.  namenode(NN) : 管理元数据，处理客户端读写请求
    
2.  datanode(DN) : 存储block
    
3.  Client ： file split ， 提供 类shell ， 和java api 方式访问HDFS，实现增删改查
    
4.  secondary namenode : 并非热备，辅助NN完成一些操作，HA集群没有这个组件
    

> 元数据:
> 文件名，块信息的映射，副本策略，文件目录结构

hdfs 以block为单位读取的，每个block块大小是128M

# 二、HDFS 操作

### hdfs shell

`hadoop fs -操作` 等价于 `hdfs dfs -操作`


### HDFS java API

增删改查操作，看文档

# 三、原理

##### 文件读写流程

属于hdfs的具体实现，todo

##### NameNode维护元数据更新

1.  fsimage :HDFS 文件系统的永久性检查点 （checkPoint），可以理解成内存镜像
    
2.  edits ： 记录hdfs的最近每一步的操作


todo：

1.  hadoop 读写流程的官方文档
    