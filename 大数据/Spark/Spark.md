Overview

## 1. Getting Start

配置一个 CodeSpaces 上的 Spark local 模式的运行环境。

- for idea
- WordCount 测试
- 添加zeppelin 作为笔记本
- 使用 jetBrains big data plugin 操作 zeepelin

 



Spark 三种部署模式

- local

- spark on Yarn 

	Yarn作为集群管理器，这时候Spark就是一个应用程序，在Yarn集群上运行。

	这时候，可以把spark二进制包放在hdfs上，本地只有一个spark-submit客户端即可，在spark-submit命令中使用 --archives 指定spark二进制文件的位置，如果避免加参数，可以写在客户端配置文件 `spark-defaults.conf` 中，spark-submit 提交时候会读取这个配置。

- Standalone 模式 ： 自己实现集群管理器。由master + worker 构成的 spark集群。



## 2. Spark-shell

Spark 的REPL， Driver启动在本地。



## 3. RDD

#### 3.1 RDD 序列化



#### 3.2 RDD 依赖

RDD经过一系列的 transformer 算子转换，RDD的依赖关系构成DAG图，从RDD的分区依赖关系看，分为窄依赖和宽依赖，窄依赖是父子RDD分区存在一对一关系（严格定义是：每个父RDD分区最多只能被子RDD的一个分区使用） ，宽依赖就是父子RDD分区存在一对多的关系，注意，这里的一对一，一对多是站在父RDD角度上讲的，也就是说，父RDD多个分区对应子RDD的一个分区，这也是一对一关系。



#### 3.3 RDD缓存

##### cache

后续RDD重算重算时可以直接用该cache的数据。

一些有shuffle的算子会自动cache，但还是建议显式 cache。

Q: 什么时候会重算？

- 一个RDD多次使用，每次使用这个RDD都要重算
- 节点故障，数据丢失
- 内存不足，被LRU机制清理了。

Q:怎么看到cache的效果

wordcount

```
    sc.textFile("input/word.txt")
      .flatMap(_.split(" "))
      .map((_, 1))
      .cache()            // cahche
      .reduceByKey(_ + _)
      .collect()
      .foreach(println)
```





<img src="https://images-note-wang.oss-cn-beijing.aliyuncs.com/note/java/image-20230803155934778.png" alt="image-20230803155934778" style="zoom: 50%;" />





##### CheckPoint 

注意，这个不是缓存的概念。主要用在数据处理链路过长，截断RDD的依赖链，以避免在重算时重新计算整个依赖链。

在每次checkPoint 之前 cache一下，避免重复计算。





#### 3.4 RDD Partition

只有key-value类型的RDD才有分区器的概念。

Spark 自带分区方式

- Hash分区
- Range分区




## 4. Spark任务调度和执行

主要是讲spark 对每个要执行的 Application 的抽象，包括DAG的划分，任务的切分和包装。

##### 4.1 总体流程

![image-20230803135552379](https://images-note-wang.oss-cn-beijing.aliyuncs.com/note/java/image-20230803135552379.png)



RDD任务切分中间分为：Application、Job、Stage和Task

（1）Application：初始化一个SparkContext即生成一个Application；

（2）Job：一个Action算子就会生成一个Job；

（3）Stage：Stage等于宽依赖的个数加1；

（4）Task：一个Stage阶段中，最后一个RDD的分区个数就是Task的个数。

注意：Application->Job->Stage->Task每一层都是1对n的关系。 



##### 4.2 spark Application 运行时

提交 Application后， 它会启动一个**Driver程序**来运行`main`函数并在集群上创建多个 **executor**。Driver程序包含应用程序的控制流程，并负责在集群上分发任务。

Executor是分布在集群工作节点上的分布式代理，负责执行任务并将结果返回给driver程序。同时启动一个 Web UI 监控Application。

Application 提交上去后，会被切分成 一个或者多个 **Job**。每个Job都是由一个**Action**算子触发生成的。

随后，沿着Action算子的血缘从后往前退，划分宽窄依赖，每个stage的边界都是一个Shuffle操作。

接着，每个Stage 会根据最后一个RDD的分区数确定task数量，通常一个分区对应一个Task，这些task会打包成一个TaskSet 提交给集群上 Executor 执行。

在Executor 上每个task都会启动一个线程计算，由于每个task都是独立的，所以一个stage的task并行执行。

因此 ： task是 Executor最小的计算单元，TaskSet（Stage）是一个调度阶段。



##### 共享变量

**https://spark.apache.org/docs/latest/rdd-programming-guide.html#shared-variables**

简单理解就是跨节点的分布式变量。



## 5. DataFrame & DataSet

带有schema的RDD，思想就是数据是有结构的，有类型的。数据结构就是表（hive表，带schema），类型就是 有行，有列，列上还有类型。

`DataFrame` 是特殊泛型的 `DataSet<Row>` 

一样有 血缘，分区，cache，的概念



#### DataFrame SQL

- 直接在内存中将数据映射成带 Schema的表， API createOrReplaceTempView
- 特殊的，模拟SQL操作的DSL的语法。
- 直接支持UDF，UDTF，UDAF。既有SQL的便利，又有编程语言的灵活。

例如：这是个例子： todo

这个差个，UDTF，UDAF的例子



#### IO

##### 读写文件

- 用Dataframe API

	[Data Sources - Spark 3.4.1 Documentation (apache.org)](https://spark.apache.org/docs/3.4.1/sql-data-sources.html)

- 转为RDD 写

	主要直接写hdfs路径上。

##### JDBC

- MySQL ， GP ， Oracle 等。

##### hive

先HDFS路径下，然后映射成表。





