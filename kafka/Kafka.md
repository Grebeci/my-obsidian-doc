# 1.Apache Kafka 是什么？

Apach Kafka 是一款**分布式流处理框架**，用于实时构建流处理应用。

















#### key concepts

官网的 [Introduction](https://kafka.apache.org/intro) 部分 介绍了 Event， Producer , Consumer ,Topic , Partition 的概念，没有 `consumer group` 概念。

下面是总结的

**Producers** 

**consumers** 

**topics** : 一个主题就是一张表，表中有分区。

**partitioned** 主题是分区的，partition 分布在broker中，partition 使用副本机制，每个分区的多个副本选出一个leader，其他都是 follower 。producer 和 consumers 操作对象都是leader。

注意leader和broker没关系，每个broker都可能是leader，换句话说，leader有很多台。

consumers group （CG）： 每个消费者组和Topic对应，每个consumer消费一个或者多个分区，一个分区只能被一个消费者（同组）消费。

> 这个总结结合尚硅谷文档看。






#### Getting Start 

##### 部署

依赖 zookeeper ， 理解`broker.id`的含义。启动集群。

Kafka是一个分布式系统，由服务器和客户端组成，通过高性能TCP网络协议进行通信。它可以部署在本地和云环境中的裸机硬件、虚拟机和容器上

Kafka作为一个或多个服务器的集群运行，可以跨越多个数据中心或云区域。

