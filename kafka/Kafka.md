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
- `Controller`：管理整个 `broker` 集群 ，类似于 hadoop namenode。由zk选举而来。
  - **领导选举** :  负责分区的leader副本（负责处理所有读写操作的副本）的选举。
  - **集群管理**：controller负责处理所有与分区和副本的变化相关的事件。当某个broker 下线，同步其上的分区副本到其他broker
  - **元数据管理**：所有关于主题、分区和副本位置的元数据都由controller来管理

# 3. 生产消息

Producer 流程

- 构建配置
- 构建 Producer
- 构造 Record
- 发送 Record



##### 构造配置

- bootstrap.servers， key.serializer，value.serializer

##### 构造 Record

- 发送到分区：指定分区，或者指定分区器，或者指定消息的 key，都不指定默认采取 粘性分区策略。
- 

##### 发送 Record

- 同步发送，异步发送

- 批次发送：批次是和分区进行绑定的。也就是说发往同一个分区的数据会进行合并，形成一个批次。

- ACK

  - `acks=0`：生产者不会等待来自服务器的任何确认，因此即使消息在传输过程中丢失，生产者也不会知道，也不会重试。

  - `acks=1`（默认值）：生产者会等待leader副本确认消息已写入其日志，但不等待所有follower副本的确认。

  - `acks=all`（或`acks=-1`）：生产者会等待所有同步副本（leader和所有活跃的follower）确认。

- 数据幂等性

  由于 `ACK`  (主要是 ack=1或all) 和 `重试策略` ，可能导致数据重复和数据乱序。

  | 配置项                                  | 配置值      | 说明                                             |
  | --------------------------------------- | ----------- | ------------------------------------------------ |
  | `enable.idempotence`                    | `true`      | 开启幂等性                                       |
  | `max.in.flight.requests.per.connection` | `小于等于5` | 每个连接的在途请求数，不能大于5，取值范围为[1,5] |
  | `acks`                                  | `all(-1)`   | 确认应答，固定值，不能修改                       |
  | `retries`                               | `>0`        | 重试次数，推荐使用Int最大值                      |

  原理：对每个消息分配唯一ID，然后缓存最近5个bath_size 的消息，查重。

  这种策略只能**单分区幂等性**， 无法实现跨会话幂等（producer 重启后幂等不等保证）

- 数据事务

  使用事务的原子性保证真正数据的真正写入。

  利用 唯一ID和2PC（两阶段提交）来保证跨分区和跨主题的原子性

  

##### 数据传输语义

| 传输语义      | 说明                                                         | 示例             |
| ------------- | ------------------------------------------------------------ | ---------------- |
| at most once  | 最多一次：不管是否能接收到，数据最多只传一次。这样数据可能会丢失。 | Socket， ACK=0   |
| at least once | 最少一次：消息不会丢失，如果接收不到，那么就继续发，所以会发送多次，直到收到为止，有可能出现数据重复 | ACK=1            |
| Exactly once  | 精准一次：消息只会一次，不会丢，也不会重复。                 | 幂等+事务+ACK=-1 |



# 4. 消费消息

Consumer 流程

- 构造配置
- 构造Consumer
- 订阅主题
- 消费数据



##### 消费者组

对消费者横向伸缩，实现并发读 Topic。

- 每个消费者都必须有自己的 Consumer Group，消费者组由 Group Coordinator 管理。
- 一个消费者组可以读多个Topic
- 消费者拉取分区策略：一个分区只能被同一个消费者读，一个消费者可以读多个分区，消费者数量超过分区数量，多余消费者无法消费数据。
- 分区分配策略
  - **Range**（默认策略）：这种策略将每个主题的分区范围平均分配给每个消费者。例如，如果一个主题有12个分区，有3个消费者，那么每个消费者将消费4个分区。
  - **Round Robin**：这种策略通过轮流分配每个主题的每个分区给消费者，以尽可能均匀地分配负载。
  - **Sticky Assignor**：这种策略旨在最小化由于消费者重新平衡导致的分区再分配。它尝试保持消费者目前已有的分区分配，只在必要时进行调整，从而减少不必要的数据重新加载。



##### offset

**1. 从哪开始消费？**

**默认行为**

**`auto.offset.reset`** 参数控制了消费者在找不到有效偏移量（无历史记录）或当前偏移量无效（如已被删除）时的行为。这个设置可以有以下几种值：

- `latest`：这是默认值。消费者将从新产生的消息开始读取，即只读取自消费者启动后到达的消息。
- `earliest`：消费者会从每个分区的可用最低偏移量开始读取，即从该分区存储的最旧的消息开始。
- `none`：如果找不到消费者的初始偏移量，将抛出异常，停止消费。

**指定偏移量**

1. **手动指定偏移量**：在消费者代码中，你可以明确设置消费者从特定偏移量开始读取。这通常通过消费者API进行，如在消费者订阅主题后显式地设置偏移量。

2. **使用`seek`方法**：在消费者订阅主题并开始轮询数据之前，可以使用`seek(TopicPartition, long)`方法指定每个分区的起始偏移量。

3. **配置`auto.offset.reset`**：如前所述，通过设置这个参数为`earliest`，新的消费者会自动从存储的最旧的数据开始消费。

**提交偏移量**

防止的消费者重启后的重复消费，Kafka优先使用提交的偏移量进行消费。可以自动提交和手动提交

- 手动提交

  禁止 auto.offset.reset=false，有异步提交和同步提交两种



##### 消费者事务

Consumer的是事务性弱，依赖客户端将数据消费过程和偏移量提交过程进行原子性绑定

# 5. 数据存储

- ISR策略
- 数据保留策略
- 数据索引文件
  - 顺序索引（offset）文件
  - 时间索引文件
