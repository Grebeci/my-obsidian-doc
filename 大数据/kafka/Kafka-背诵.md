 
 #### Overview

传统的消息队列的主要应用场景包括：**缓存/消峰**、**解耦**和**异步通信。**

缓冲/削峰 ：协调生产者和消费者的处理速度

解耦： 

异步通信： 



**Kafka** ： Kafka是一个开源的分布式事件流平台。

**事件**： 事件记录了世界上或您的业务中 "发生了一些事情 "的事实。在文档中，它也被称为记录或消息。向 Kafka 读取或写入数据时，是以事件的形式进行的。从概念上讲，一个事件有一个键、值、时间戳和可选的元数据头。下面是一个事件示例：

#### Kafka 结构

结构： C/S架构

servers: 一个或者多个服务器集群形式运行，可以跨多个数据中心和云区域。

clients： 客户端。



#### 核心概念

**Producers** 

**consumers** 

**topics** : 一个主题就是一张表，表中有分区。

**partitioned** 主题是分区的，partition 分布在broker中，partition 使用副本机制，每个分区的多个副本选出一个leader，其他都是 follower 。producer 和 consumers 操作对象都是leader。

注意leader和broker没关系，每个broker都可能是leader，换句话说，leader有很多台。

consumers group （CG）： 每个消费者组和Topic对应，每个consumer消费一个或者多个分区，一个分区只能被一个消费者（同组）消费。



#### 消息传递语义

AI-prompt : 解释下Kafka消息传递的语义

下面是AI总结的，

```
在Kafka中，消息传递语义指的是生产者、消费者和代理（broker）之间如何共享消息。有三种类型的消息共享：

- 最多一次（At most once）：消息只会被传递一次，如果发生系统故障，消息可能会丢失且不会重新传递。
- 至少一次（At least once）：这意味着消息会被传递一次或多次。如果发生系统故障，消息永远不会丢失，但它们可能会被传递多次。
- 正好一次（Exactly once）：这是首选行为，每条消息只会被传递一次。即使系统的某些部分发生故障，消息也永远不会丢失或被读取两次

这些语义在延迟和消息持久性之间提供了权衡。您需要根据应用程序上下文选择合适的语义。
```

重点参考 `Confluence kafka ` 文档 

[Message Delivery Guarantees for Apache Kafka | Confluent Documentation ~ Apache Kafka 的消息交付保证 | Confluent 文档](https://docs.confluent.io/kafka/design/delivery-semantics.html)

[Message Delivery Semantics](https://kafka.apache.org/20/documentation.html#semantics)



这个语义需要生产者和消费者分别保证，分别是

[Producer delivery](https://docs.confluent.io/kafka/design/delivery-semantics.html#producer-delivery)

[Consumer receipt](https://docs.confluent.io/kafka/design/delivery-semantics.html#consumer-receipt)



**对应的配置**

最多一次（At most once） ： Producer ACK=1

至少一次（At least once  ： Producer ACK=-1 + 分区副本数 >= 2 + ISR里应答的最小副本数>=2  (ISR: In-Sync Replica 是指与领导副本同步的副本集合)

精准一次（Exactly once） ：  Producer :  幂等和事务

**Kafka如何判断幂等性：**

具有 `<PID,Partition SeqNumber>`  相同主键提交时，Broker只会持久化一条。

三元组 PID是生产者ID，partition 是分区号， Sequence Number 单调递增的。

这里的幂等失败重试不会重复，如果生产者因为某些原因（例如网络原因）未能收到broker确认，这时候触发重试会保证幂等，因为SeqNumber 不会增加。但是如果是因为业务的重复数据，这是要求Kafka去重，目前只能在应用层面实现去重。



#### 消费消息

分区，消费者组，都是为了并发性。

消费者组消费一个topic，一个组内多个消费者是为什么并发消费。



##### 原理

**Kafka consumer 怎么消费消息，如何知道消费到哪里了，Broker怎么把 每个consumer进度存下来**

在Kafka中，消费者提交它们的消费偏移量（consumer offsets）是通过向特殊的内部主题`__consumer_offsets`发送消息来实现的。这个主题由多个分区组成，每个分区都有一个领导副本和多个追随副本。消费者组中的每个消费者都会将其消费偏移量发送到`__consumer_offsets`主题的一个特定分区。

因此，当消费者提交它们的消费偏移量时，它们会将消息发送到`__consumer_offsets`主题的一个特定分区的领导副本。这个领导副本负责将消息写入磁盘并将其复制到追随副本。





##### 分区分配策略

每种策略有两部分，初始分配策略，有新消费者加入或者消费者挂掉后选择的分配策略

Range

RangeRobin

Sticky



##### 维护Offset

这个是Kafka的核心的设计

**存储进度**：消费者提交 offset 到内部主题 `__consumer_offsets`，`__consumer_offsets`主题中存储的是每个消费者组中每个主题的每个分区的最新消费偏移量。`__consumer_offsets` 的数据结构是 key-value key是 (group.id,topic,partition)，value 是 offset



**提交**

- 自动提交，有参数控制
- 手动提交，有同步提交和异步提交



指定消费位置

- 指定offset 配置：`auto.offset.reset = earliest | latest | none`  ， 也可以用API seek指定
- 指定时间：先拿到指定时间的offset，然后指定offset



##### 消费者事务

Kafka consumer端 精准一次性消费：核心就是将消费过程和提交offset做原子绑定。



#### 精准一次

确保生产者和消费者消息传递具有精准一次性消费的语义，分为生产者和消费者两端的实现

- kafka producer 端的精准一次靠ack和幂等
- Kafka consumer端 ：核心就是将消费过程和提交offset做原子绑定，主要解决就是数据重复消费，数据丢失（漏数据）的情况。
	- [kafka如何实现精准一次性消费？ - 掘金 (juejin.cn)](https://juejin.cn/post/7002169135908012062) -参考 【问题解决】
		- 如果数据处理最终存入事务性数据库，那就把offset一并存入数据库中，然后用数据库事务保证
		- 如果数据处理实现了幂等（ES），那重复问题已经解决。这时候只需要保证不丢就可以，即手动提交偏移量。



#### 部署

需要 Zookeeper 和 Kafka






#### 命令操作

```bash

```

#### 监控



