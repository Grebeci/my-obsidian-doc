# 1.Apache Kafka 是什么？

Apach Kafka 是一款 **分布式流处理框架**，用于实时构建流处理应用。在大数据上，用于构建 **实时数据管道** 和 **流式应用程序** 的工具；在后端上，作为 **消息队列**中间件使用。

> 消息队列作用：缓存，异步，削峰，解耦

- 基于生产者-消费者模型。
- 基于发布/订阅（Publish/Subscribe）模型



#### Kafka 配置

kafka 端口 ：9092

目录结构和关键的配置文件

```
`-- kafka
    |-- bin
    |   |-- zookeeper-server-start.sh
    |   |-- zookeeper-server-stop.sh
    |   `-- zookeeper-shell.sh
    |-- config
    |   |-- connect-console-sink.properties
    |   |-- connect-console-source.properties
    |   |-- connect-distributed.properties
    |   |-- connect-file-sink.properties
    |   |-- connect-file-source.properties
    |   |-- connect-log4j.properties
    |   |-- connect-mirror-maker.properties
    |   |-- connect-standalone.properties
    |   |-- consumer.properties
    |   |-- kraft
    |   |-- log4j.properties
    |   |-- producer.properties
    |   |-- server.properties
    |   |-- tools-log4j.properties
    |   |-- trogdor.conf
    |   `-- zookeeper.properties  # zookeeper 配置文件
    |-- libs
```



`zookeeper.properties `配置项

```
# ZooKeeper数据存储位置
dataDir=/tmp/zookeeper

# kafka 内部zk客户端
clientPort=2181
```

server.properties

```
# 服务器监听端口，listeners是kafka集群内部的端口，advertised.listeners是广播给客户端 broker端口，如果没有，就用listeners
# 这两个选项不用记忆。
#listeners=PLAINTEXT://:9092
#advertised.listeners=PLAINTEXT://your.host.name:9092

# kafka 数据 存放位置
log.dirs=/tmp/kafka-logs

# kafka节点数字标识，集群内具有唯一性
broker.id=1

# ZooKeeper软件连接地址，2181为默认的ZK端口号 /kafka 为ZK的管理节点
zookeeper.connect=localhost:2181/kafka
```



# 2. Key Concept

参考官方文档：[Main Concepts and Terminology](https://kafka.apache.org/documentation/#intro_concepts_and_terms)  介绍了 Event， Producer , Consumer ,Topic , Partition 的概念。

- `Topic` : 可以类比文件夹，数据库的表概念。
- `Partition` : 类比hive表的分区，或者文件系统的文件。

**运行时架构**

- `broker` :  数据节点的角色。也可以称为 Kafka Server
- 











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

