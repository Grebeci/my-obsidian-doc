

Spark 是集群的计算引擎。有以下几个特点

速度快： 扩展MR计算模型，支持交互式查询。能够再内存中计算，即使必须再磁盘上计算，效率也高于MR。

通用性： 适用不同的计算场景（批处理，流处理，交互式查询，迭代算法）。接口丰富，便于整合其他数据处理工具。

> 大一统的软件栈：
>
> 好处： 上层高级组件都可以从下层的改进获益；运行整个软件栈的代价变小了（部署，维护，测试，支持）



组成：

spark-core : 实现了Spark的基础功能和RDD的API的定义。

> RDD 表示分布在多个计算节点上可以并行操作的元素集合，是 Spark 主要的编程抽象



Spark-SQL : 支持SQL查询数据，支持多种数据源，（Hive表，Parquet，JSON格式。 支持SQL和RDD混合编程。

MLib: ML库

GraphX : 图计算

集群管理器： 为了实现在一个和多个节点的伸缩计算，支持在各种集群调度器运行，包括Hadoop YARN， Apache Mesos ，和独立调度器（spark自带的建议调度器）。



Spark 用途：

针对数据工程师，spark 为开发用于集群并行执行程序提供了一个捷径。通过封装，spark不需要开发者关注分布式系统编程这样复杂的问题，也无需关注网络通信和程序的容错性。



在 Spark 中，我们通过对分布式数据集的操作来表达我们的计算意图，这些计算会自动地在集群上并行进行。这样的数据集被称为弹性分布式数据集
（resilient distributed dataset），简称 RDD。RDD 是 Spark 对分布式数据和计算的基本抽象。









### 核心概念



每个spark应用 使用驱动器程序（driver program） 来发起集群上的各种并行操作。driver 包含应用的main函数，并且定义集群的分布式数据集，还对这些分布式数据集做了相关操作。

driver 管理多个 executor， 并且把每个算子再executor进行并行执行

以前认为driver 执行算子外面的程序，算子和算子内部得代码分发到每个 Executor ，和这里理解得基本一致。





sparkContext ：这个对象代表对计算集群的一个连接，可以通过这个对象创建RDD。



spark API 特点，利用函数式编程特性传递函数。

` lines.filter(line=> line.contains("Python"))`

filter 基于函数得操作也会在集群上并行执行，也就是说，spark会将 `line=> line.contains("Python")` 这个函数分发到各个 Executor 上，这样我们可以在 单一得 driver端编程，并让代码自动运行在多个节点上。



#### RDD基础

弹性分布式数据集

在任何时候都能进行重算是我们为什么把RDD描述为 `弹性` 的原因，当保存RDD数据的一台机器失败时候，Spark可以用这个特性重算丢掉的分区，该过程对用户透明。


Spark 中的 RDD 就是一个不可变的分布式对象集合。每个 RDD 都被分为多个分区，这些分区运行在集群中的不同节点上。

把RDD看成存放数据的集合是不对的，可以看成封装了计算逻辑的一种数据结构，相当于每个RDD是我们转化构建出来的，记录如何计算数据的执行列表。

用户可以使用两种方法创建 RDD：读取一个外部数据集，或在驱动器程序里分发驱动器程序中的对象集合（比如 list 和 set）。





RDD支持两种类型的操作：

- 转化操作，有一个RDD生成另一个RDD，例如根据谓词匹配筛选

	```
	var filterRDD = lines.filter(line => line.contains("Scala"))
	# 生成只包含Scala字符串的新的RDD
	```

- 行动操作，行动操作会对RDD计算一个结果，并返回到Driver端，因为要关注数据量对Driver的压力。

- 行动操作则是向驱动器程序返回结果或把结果写入外部系统的操作，会触发实际的计算

	```
	lines.first()
	```



分成两类的标准：是否惰性计算。

根据Spark计算RDD的方式不同，所以分成这两类，本质上是看是否进行惰性计算。

如果对于一个特定的函数是属于转化操作还是行动操作感到困惑，你可以看看它的返回值类型：转化操作返回的是 RDD，而行动操作返回的是其他的数据类型







默认情况下，Spark 的 RDD 会在你每次对它们进行行动操作时重新计算。如果想在多个行动操作中重用同一个 RDD ，可以使用 `RDD.persist()` 让 Spark 把这个 RDD 缓存下来。在第一次对持久化的RDD计算后，Spark会把RDD的内容保存在内存中（以分区的方式存储到各个节点上），这样在之后的行动操作中，就可以重用这些数据了。





Spark RDD编程流程

1) 从外部数据源创建出RDD。
2) 使用诸如 filter 这样的算子对RDD进行转化为新的RDD。
3) 判断是否需要将中间计算结果RDD 执行 `persist()` 或者 `cache()` 操作。
4) 使用行动操作（count，first） 触发一次计算。



#### 创建RDD

1. 读取外部数据集。

	读取外部文件

	```scala
	sc.textFile(file)
	```

	

2. 在driver端对一个集合进行并行化。

	```scala
	val lines = sc.parallelize(List("pandas", "i like pandas")
	```

	使用parallelize() 方法将一个集合转为RDD，可以快速创建一个RDD，缺点是需要把数据集先放进一台机器的内存中。





#### RDD 操作



- RDD 的转化操作是返回一个新的 RDD 的操作。

	```scala
	val inputRDD = sc.textFile("log.txt")
	val errorsRDD = inputRDD.filter(line => line.contains("error"))
	```

	filter 操作不会改变已有inputRDD的数据，该操作会返回一个全新的RDD

	



通过转化操作，你从已有的 RDD 中派生出新的 RDD，Spark 会使用谱系图（lineage graph）来记录这些不同RDD 之间的依赖关系。Spark 需要用这些信息来按需计算每个 RDD，也可以依靠谱系图在持久化的 RDD 丢失部分数据时恢复所丢失的数据。



- 行动操作 ： 它们会把最终求得的结果返回到驱动器程序，或者写入外部存储系统中。

- 由于操作需要生成实际的输出，所以他会强制执行那些求值必须用的RDD转换操作。



需要注意的是，每当我们调用一个新的行动操作时，整个 RDD 都会从头开始计算。要避免这种低效的行为，用户可以将中间结果持久化，





#### 惰性求值

含义： 调用转换操作（map）不会触发执行，spark会记录操作的相关信息（血缘）。



Spark 使用惰性求值，可以合并一些操作，不同于MR程序需要组合计算逻辑，来减少MR个数。在Spark中，我们可以用连续简单的操作取代一个复杂的映射，性能不会损失多少，而且更容易管理和测试。

不要认为一个复杂的映射逻辑，可以比一连串的操作性能提升明显。

有些数据处理逻辑确实可以用一个超大SQL写，但是后期不好维护，而且也提升不了很大的性能。









#### 传递函数

可以把定义的内联函数，方法的引用，或者静态方法传递给Spark，（利用Scala 函数式编程）。

传递的函数机器引用的数据需要是可以序列化的（实现了 Serializable 接口），

> 传递函数时需要小心的一点是，Scala 会在你不经意间把函数所在的对象也序列化传出去。当你传递的对象是某个对象的成员，或者包含了对某个对象中一个字段的引用时（例如 this.field），Spark 就会把整个对象发到工作节点上，这可能比你想传递的东西大得多。
>
> ```scala
> class SearchFunctions(val query: String) {  
>     def isMatch(s: String): Boolean = {    s.contains(query)  }  
>     def getMatchesFunctionReference(rdd: RDD[String]): RDD[String] = {    
>         // 问题："isMatch"表示"this.isMatch"，因此我们要传递整个"this"
>         rdd.map(isMatch)  
>     }  
>     def getMatchesFieldReference(rdd: RDD[String]): RDD[String] = {    
>         // 问题："query"表示"this.query"，因此我们要传递整个"this"
>         rdd.map(x => x.split(query))  
>     }  
>     def getMatchesNoReference(rdd: RDD[String]): RDD[String] = {    
>         // 安全：只把我们需要的字段拿出来放入局部变量中
>         val query_ = this.query    
>         rdd.map(x => x.split(query_))  
>     }
> }
> ```
>
> 替代的方案是，只把你所需要的字段从对象中拿出来放到一个局部变量中，然后传递这个局部变量。（ 代码 8-9行 对比 12-14行 ）



如果出现了  NotSerializableException 通常是我们传递了一个不可序列化的类中的函数或者字段。





### 不同的RDD的转换

有些函数没有定义在标准RDD类中，比如数据值型算子和键值对算子。访问这些附加功能，必须保证获得了正确的RDD类。



在 Scala 中，将 RDD 转为有特定函数的 RDD（比如在 RDD[Double]上进行数值操作）是由隐式转换来自动处理的。

我们需要加上 import org.apache.spark.SparkContext._来使用这些隐式转换。

（问题：sparkContext隐式转换怎么查看）



### 持久化（缓存）

含义 ：当我们让 Spark 持久化存储一个 RDD 时，计算出RDD 的节点会分别保存它们所求出的分区数据。如果一个有持久化数据的节点发生故障，Spark 会在需要用到缓存的数据时重算丢失的数据分区。如果希望节点故障的情况不会拖累我们的执行速度，也可以把数据备份到多个节点上。

原因：由于Spark RDD是惰性求值的，每个行动算子都会重算RDD以及他的所有依赖。在迭代算法中消耗很大。



持久化级别：

缓存在JVM堆空间中。使用LRU算法。



#### 数值操作

stats()

计算count, mean, stdev, max, min)





#### 键值对操作（Pair RDD）

##### 1. 转化操作

Pair RDD 可以使用所有标准 RDD 上的可用的转化操作。

reduceByKey(func)

合并具有相同的键值。

groupByKey(func)

对具有相同键的值进行分组。

combineByKey(createCombiner, mergeValue, mergeCombiners, partitioner)

使用不同的返回类型合并具有相通键的值。

mapValues(func)

对Pair RDD 中的每个值应用一个函数而不改变键。相当于  `map{case (x, y): (x, func(y))}`

flatMapValues(func)

对每个值应用一个函数，生成0个或多个键值对。

keys() 

返回仅包含键的RDD

values() 

返回仅包含值的RDD

sortByKey() 

根据键排序的RDD



针对两个pair RDD 的转换操作

subtractByKey()

删掉RDD中键与otherRDD 中的键相同的元素



join，rightOuterJoin， leftOuterJoin,cogroup

内连接,右连接，左连接,全连接

```scala
rdd = {(1,2),(3,4),(3,6)} other = {(3,9)}
rdd.subtractByKey(other).foreach(print)
(1,2)

rdd.join(other).foreach(print)
(3,(4,9))(3,(6,9))

rdd.rightOuterJoin(other).foreach(print)
(3,(Some(4),9))(3,(Some(6),9))

rdd.leftOuterJoin(other).foreach(print)
(1,(2,None))(3,(4,Some(9)))(3,(6,Some(9)))

rdd.cogroup(other).foreach(print)
(1,(CompactBuffer(2),CompactBuffer()))(3,(CompactBuffer(4, 6),CompactBuffer(9)))
```





##### 2. 聚合操作

为数据集中的每个键进行并行的归约操作，每个归约操作会将键相同的值合并起来。因为数据集中可能有大量的键，所以 这些操作没有被实现为向用户程序返回一个值的行动操作。实际上，它会返回一个由各键和对应键归约出来的结果值组成的新的 RDD

###### reduceByKey

###### foldbyKey()

###### combineByKey()

combineByKey()会使用一个叫作 createCombiner() 的函数来创建那个键对应的累加器的初始值。需要注意的是，这一过程会在每个分区中第一次出现各个键时发生，而不是在整个 RDD 中第一次出现一个键时发生。

如果这是一个在处理当前分区之前已经遇到的键，它会使用 mergeValue() 方法将该键的累加器对应的当前值与这个新的值进行合并。
由于每个分区都是独立处理的，因此对于同一个键可以有多个累加器。如果有两个或者更多的分区都有对应同一个键的累加器，就需要使用用户提供的 mergeCombiners() 方法将各个分区的结果进行合并。

使用 **combineByKey ** 算子可以在分区预聚合，也就是Map端聚合。使用这个算子大多是为了这个收益。



熟悉 MapReduce 中的合并器（combiner）概念的读者可能已经注意到，调用 reduceByKey() 和 foldByKey() 会在为每个键计算全局的总结果之前先自动在每台机器上进行本地合并。用户不需要指定合并器。更泛化的 combineByKey() 接口可以让你自定义合并的行为。





##### 分区

对单值RDD（RDD[String]） 这样的数据讨论分区没有意义。我们可以让用户控制键值对RDD在各个节点的分布，有时候，使用可控的分区把常被一起访问的数据放到同一节点上，可以减少应用的通信开销。



每个 RDD都有固定数目的分区，分区数决定了在 RDD 上执行操作时的并行度。每个 RDD都有固定数目的分区，分区数决定了在 RDD 上执行操作时的并行度。



有时，我们希望在除分组操作和聚合操作之外的操作中也能改变 RDD 的分区。对于这样的情况，Spark 提供了 repartition()
函数。它会把数据通过网络进行混洗，并创建出新的分区集合。



查看分区

rdd.partitions.size



在分布式程序中，通信的代价是很大的，因此控制数据分布以获得最少的网络传输可以极大地提升整体性能。

只有当数据集多次在诸如连接这种基于键的操作中使用时，分区才会有帮助







#### 行动操作



countByKey

每个键对应的元素分别计数



collectAsMap() 

转为Map



lookUp(key) 

返回给定键对应的所有值。





#### 数据读取与保存

格式，数据类型

三类数据源

- 本地文件或者分布式文件系统（hadoop): Spark 可以通过Hadoop 的MR使用的inputFormat 和 outputFormat 接口访问数据。常见的文件格式和文件系统与存储系统（hdfs，S3,Cassandra, HBase）都支持这种接口。 spark 支持很多文件格式，包括文本文件、JSON、SequenceFile，以及protocol buffer。而且针对不同文件的压缩格式。

- Spark SQL中的结构化数据源

	JSON 和 Hive

- 数据库和键值存储

	JDBC源，HBase、ES



Spark 会根据文件扩展名选择对应的处理方式。这一过程是封装好的，对用户透明。



文本文件：

- 读取文件文件，可以指定 minPartitions

	path 可以是目录，也可以是文件路径。

	path支持 通配符模式。

	`def textFile(path: String, minPartitions: Int): rdd.RDD[String] `

- 读取多个文件。 方法会返回一个 pair RDD，其中键是输入文件的文件名

	`def wholeTextFiles(path: String, minPartitions: Int): rdd.RDD[(String, String)]`  



- 保存文本文件

	path ： spark 将传入的路径作为目录对待，会在那个目录上输出为多个文件。这样，Spark 就可以从多个节点上并行输出了。

	产生的文件：

	_SUCCESS : 写入成功标志文件，保证原子性

	part-00000： 实际文件 ，文件名格式  part-number

	产生的数据文件 part-xxx 个数：

	

	`def saveAsTextFile(path: String): Unit `

	

	`def saveAsTextFile(path: String, codec: Class[_ <: org.apache.hadoop.io.compress.CompressionCodec]): Unit`

	 

	

JSON

- JSON是半结构化数据格式。读取 JSON 数据的最简单的方式是将数据作为文本文件读取，然后使用 JSON 解析器来对 RDD 中的值进行映射操作。
- 也可以使用我们喜欢的 JSON 序列化库来将数据转为字符串，然后将其写出去。
- Spark SQL 方式读取。
- 使用mapPartitions重用解释器。



保存为Json

将由结构化数据组成的 RDD 转为字符串 RDD，然后使用 Spark 的文本文件API 写出去。



CSV 和 TSV



读取&写出







#### 文件系统

- local file system 

	spark 读取 local file system 的时候，需要在所有 worker node 都存在。在读取的时候每个 worker node 的task会读取文件的一部分。有一个细节是spark app 所运行的节点也要有这个file，因为需要用到file进行Partition的划分。
	如果没有放在集群的所有节点上，就需要在 Driver 端从本地读取该文件而无需使用整个集群，然后再调用用 parallelize 将内容分发到各个节点上。

- HDFS 
	Spark 和 HDFS 可以部署在同一批机器上，这样 Spark 可以利用数据分布来尽量避免一些网络开销

	

spark.addFile(path)





案例

sc.textFile

spark-shell --master local

spark-shell --master yarn

（ 有的模式默认是 local



spark-app 读取配置文件

spark-submit --files 

-- master yarn

-- deploy-mode cluster

当spark读取一个非常大的本地文件时，读入内存后分区会自动分布到多个节点上吗？ - 羊咩的回答 - 知乎 https://www.zhihu.com/question/36996853/answer/79937183

[Spark读取本地文件和HDFS文件_SunnyRivers的博客-CSDN博客_spark读取本地文件](https://blog.csdn.net/Android_xue/article/details/103904858)













Spark SQL

结构化数据： 有结构信息的数据 -- 也就是所有的数据记录都具有一致字段结构的集合。



每个记录 映射成一个 Row 对象，row对象访问是基于下标的。每个Row都有一个get方法，返回一个一般类型让我们进行类型转换。针对基本类型，有专用的get方法： getFloat getInt getLong getString



Hive

Spark SQL 支持Hive的任何表。



- Spark SQL 连接Hive 需要提供Hive的配置文件。需要将 `hive-site.xml` 复制到 ${SPARK_HOME}/conf/ 下。



JSON 

Spark SQL 可以推断出他们的类型信息，并将这些数据读取为记录。这样就可以提取JSON某些字段信息。



数据库（JDBC

spark 的所有 work node 都会连接数据库， 注意并发。





 Spark 会自动重新执行失败的或较慢的任务来应对有错误的或者比较慢的机器。例如，如果对某分区执行 map() 操作的节点失败了，Spark 会在另一个节点上重新运行该任务。即使该节点没有崩溃，而只是处理速度比别的节点慢很多，Spark 也可以抢占式地在另一个节点上启动一个“投机”（speculative）型的任务副本，如果该任务更早结束就可以直接获取结果。即使没有节点失败，Spark 有时也需要重新运行任务来获取缓存中被移除出内存的数据。因此最终结果就是同一个函数可能对同一个数据运行了多次，这取决于集群发生了什么。

#### 累加器

accumulator ：分布式共享只写变量。将工作节点中的值聚合到驱动器程序中。

Spark 会自动把闭包中所有引用到的变量发送到工作节点上。

要解决的问题： map() filter() 这样的函数使用dirver的变量（闭包）， 集群中的每个task都会得到这个变量的副本，更新这些副本值不会改变Driver的变量。



- work node 不能访问累加器的值，累加器只有在 driver 端可以访问。
- task 失败重试，重用rdd的根据血缘的重复计算，都会让累加器的值不可靠。原则是 
	- 对于要在行动操作中使用的累加器，Spark只会把每个任务对各累加器的修改应用一次。因此，如果想要一个无论在失败还是重复计算时都绝对可靠的累加器，我们必须把它放在 foreach() 这样的行动操作中。
	- 对于在 RDD 转化操作中使用的累加器，就不能保证有这种情况了。转化操作中累加器可能会发生不止一次更新。（缓存失效重算，重用RDD重算）

- 自定义累加器满足交换律和结合率。



#### 广播变量

Broadcast

分布式只读变量

- 默认发射机制专门为小任务优化；
- 每个task都会得到一份，这意味着每个节点冗余个多个副本
- 多次发送，如果多个并行操作使用同一个变量，spark会为每个操作分别发送。

注意：

- 只读变量，在work node修改只会影响本地，不会影响其他节点。
- 广播比较大的值，序列化格式很重要。



#### 基于分区的操作

减少对每个数据元素进行重复配置的工作。

- 重用数据库连接，每个分区共享一个数据库连接池，避免建立多个连接
- 重用JSON解释器。
- 避免重复创建对象



按分区执行的操作符

mapPartitions()

mapPartitionsWithIndex()

foreachPartitions()


本地模式下，Spark 驱动器程序和各执行器程序在同一个 Java 进程中运行。这是一个特例；执行器程序通常都运行在专用的进程中。



#### Spark 运行时架构 

- 云上环境： 可以在各种各样的集群管理器上运行（ hadoop yarn ， apache Mesos ），让spark能用于共享的云计算环境

spark 在分布式环境采用 master / slave 结构， 有一个节点负责中央协调（ driver) 其他工作节点（executor）， driver 和 executor 都是独立的java进程。

driver和所有的 executor 一起成为一个spark应用 （application)。

spark应用通过一个叫做 集群管理器（ Cluster Manager） 的外部服务在集群中的机器上启动。 

Cluster Manager ： spark自带的被称为独立集群管理器， Hadoop yarn ， Apache Mesos



- Dirver

	执行main方法的进程。driver一旦中止，spark应用就结束了。

	Driver的职责

	- 把用户程序转为任务（task）

		从上层上看，spark应用都遵循同样的结构，从输入源创建RDD，执行一系列的转化操作，然后用行动操作收集或者存储RDD的数据。这一系列操作组成了逻辑上的有向无环图。spark会对这个逻辑执行计划转为一系列的步骤（stage），每个步骤有多个Task组成。这些任务打包送到集群中。Task是spark最小的工作单元。

	- 为执行器节点调度任务

		executor 向 driver注册，driver 根据数据位置优先的原则分配task。driver会跟踪executor cache数据的位置，以便调度后续的任务，达到减少网络传输的目的。

		driver 会把运行时信息通过网页展示出来。

- Executor

	是一个进程，负责运行spark的task。

	作用

	- 运行task，返回driver结果
	- 通过 Block Manager 为要求缓存的RDD提供内存式的存储。RDD式直接缓存在执行器进程内，因此任务可以在运行时充分利用缓存数据加速计算

	

	

- Cluster Manager

	启动driver 和executor的组件

	

### 部署应用

spark 为各种集群管理器提供了统一的工具`spark-submit`来提交作业。



spark-submit 一般格式

`bin/spark-submit [options] <app jar | python file> [app options`



[options]

是要传给 spark-submit 的标记列表。你可以运行 `spark-submit --help` 列出所有可以接收的标记。

spark-submit 选项主要分三类

- master ：集群的url
- 调度信息：作业申请资源量
- 运行时依赖： 需要部署到所有工作节点的库和文件

<app jar | python File> 
表示包含应用入口的 JAR 包或 Python 脚本。

[app options]
是传给你的应用的选项。如果你的程序要处理传给 main() 方法的参数，它只会得到 [app options] 对应的标记，不会得到 spark-submit 的标记。



```shell
$ bin/spark-shell --help

Usage: ./bin/spark-shell [options]

Scala REPL options:
  -I <file>                   preload <file>, enforcing line-by-line interpretation

Options:
  --master MASTER_URL         spark://host:port, mesos://host:port, yarn,
                              k8s://https://host:port, or local (Default: local[*]).
  --deploy-mode DEPLOY_MODE   Whether to launch the driver program locally ("client") or
                              on one of the worker machines inside the cluster ("cluster")
                              (Default: client).
  --class CLASS_NAME          Your application's main class (for Java / Scala apps).
  --name NAME                 A name of your application.
  --jars JARS                 Comma-separated list of jars to include on the driver
                              and executor classpaths.
  --packages                  Comma-separated list of maven coordinates of jars to include
                              on the driver and executor classpaths. Will search the local
                              maven repo, then maven central and any additional remote
                              repositories given by --repositories. The format for the
                              coordinates should be groupId:artifactId:version.
  --exclude-packages          Comma-separated list of groupId:artifactId, to exclude while
                              resolving the dependencies provided in --packages to avoid
                              dependency conflicts.
  --repositories              Comma-separated list of additional remote repositories to
                              search for the maven coordinates given with --packages.
  --py-files PY_FILES         Comma-separated list of .zip, .egg, or .py files to place
                              on the PYTHONPATH for Python apps.
  --files FILES               Comma-separated list of files to be placed in the working
                              directory of each executor. File paths of these files
                              in executors can be accessed via SparkFiles.get(fileName).
  --archives ARCHIVES         Comma-separated list of archives to be extracted into the
                              working directory of each executor.

  --conf, -c PROP=VALUE       Arbitrary Spark configuration property.
  --properties-file FILE      Path to a file from which to load extra properties. If not
                              specified, this will look for conf/spark-defaults.conf.

  --driver-memory MEM         Memory for driver (e.g. 1000M, 2G) (Default: 1024M).
  --driver-java-options       Extra Java options to pass to the driver.
  --driver-library-path       Extra library path entries to pass to the driver.
  --driver-class-path         Extra class path entries to pass to the driver. Note that
                              jars added with --jars are automatically included in the
                              classpath.

  --executor-memory MEM       Memory per executor (e.g. 1000M, 2G) (Default: 1G).

  --proxy-user NAME           User to impersonate when submitting the application.
                              This argument does not work with --principal / --keytab.

  --help, -h                  Show this help message and exit.
  --verbose, -v               Print additional debug output.
  --version,                  Print the version of current Spark.

 Cluster deploy mode only:
  --driver-cores NUM          Number of cores used by the driver, only in cluster mode
                              (Default: 1).

 Spark standalone or Mesos with cluster deploy mode only:
  --supervise                 If given, restarts the driver on failure.

 Spark standalone, Mesos or K8s with cluster deploy mode only:
  --kill SUBMISSION_ID        If given, kills the driver specified.
  --status SUBMISSION_ID      If given, requests the status of the driver specified.

 Spark standalone, Mesos and Kubernetes only:
  --total-executor-cores NUM  Total cores for all executors.

 Spark standalone, YARN and Kubernetes only:
  --executor-cores NUM        Number of cores used by each executor. (Default: 1 in
                              YARN and K8S modes, or all available cores on the worker
                              in standalone mode).

 Spark on YARN and Kubernetes only:
  --num-executors NUM         Number of executors to launch (Default: 2).
                              If dynamic allocation is enabled, the initial number of
                              executors will be at least NUM.
  --principal PRINCIPAL       Principal to be used to login to KDC.
  --keytab KEYTAB             The full path to the file that contains the keytab for the
                              principal specified above.

 Spark on YARN only:
  --queue QUEUE_NAME          The YARN queue to submit to (Default: "default").
      
```





`--master ` :表示要连接的集群管理器 （集群的URL）

| 值                  | 描述                                                         |
| ------------------- | ------------------------------------------------------------ |
| `spark://host:port` | 本地模式下，Spark 驱动器程序和各执行器程序在同一个 Java 进程中运行。这是一个特例；执行器程序通常都运行在专用的进程中。 |
| `mesos://host:port` | 连接到指定端口的 Mesos 集群上。默认情况下 Mesos 主节点监听 5050 端口 yarn 连接到一个 YARN 集群。当在 YARN 上运行时，需要设置环境变量 HADOOP_CONF_DIR 指向 Hadoop 配置目录，以获取集群信息 |
| `local`             | 运行本地模式，适用单核                                       |
| `local[N]`          | 运行本地模式，适用N个核心                                    |
| `local[*]`          | 运行本地模式，适用尽可能多的核心                             |



`--jars`

`--files`



##### 打包和依赖

`--jars` 这个选项可以提交独立的依赖，比如mysql驱动。

常见的打包工具是 Maven和Sbt



依赖冲突

依赖冲突在spark的表现为 spark作 业执行过程中抛出 NoSuchMethodError、ClassNotFoundException，或其他与类加载相关的JVM 异常。

场景：当用户应用与 Spark 本身依赖同一个库时可能会发生依赖冲突。 解决办法

- 修改应用，使其使用的依赖库的版本和spark所使用的相同。
- shading打包。



#### 集群管理器

spark 支持两类集群管理器

- 独立部署，使用自带的独立模式
- 集成到共享集群上（hadoop集群），运行在 Hadoop YARN 和 Apache Mesos 上。

集群模式(`--delploy-mode`)

- client 模式，driver启动在 spark-submit 所在的机器上。
- cluster模式，driver 启动在集群内部。

独立部署

Hadoop YARN



Mesos



Amazon EC2



#### Spark配置机制

- spark-submit方式
	- spark-submit 默认会读取 `conf/spark-default.conf` 文件,也可以用选项` --properties-File` 指定需要读取的配置文件。
	- 使用 `--conf prop=value` 的方式指定。

- 程序代码中用 `SparkConf` 类的 set方法 指定配置信息。

优先级

最高的是显示调用set() 方法设置的选项，其次是 通过 spark-submit 传递的参数，再次是写在配置文件的值，最后是系统默认值。这和sparkConf 构造方式有关。



特别的，`SPARK_LOCAL_DIRS` 需要在 conf/spark-env.sh 指定为 逗号分开的存储位置的列表，这个选项是spark用来 shuffle 本地存储的路径。





Spark 会对逻辑执行计划作一些优化，比如将连续的映射转为流水线化执行，将多个操作合并到一个步骤中等。这样 Spark就把逻辑计划转为一系列步骤（stage）。而每个步骤又由多个任务组成。这些任务会被打包并送到集群中。任务是 Spark 中最小的工作单元，用户程序通常要启动成百上千的独立任务。



#### Spark 的性能指标

1. 并行度

	RDD执行的时候会被分为一系列的分区，spark根据这些分区创建task，由于每个task线程只能运行一个rdd partition，所以通过设定分区数来对并行度调优。
	
	spark 并行度是指一个stage下并行的task的数量。分为资源并行度（物理并行度）和数据并行度（逻辑并行度）。
	
	资源并行度：由节点数（executor）和CPU数（core）决定。
	
	数据并行度：task（partition）的数量。
	
	
	
	[CSDN-Spark调优之 -- Spark的并行度深入理解](https://blog.csdn.net/eraining/article/details/109353935)
	
	
	
	调整并行度
	
	一般设置在 总core的 2~3 倍
	
	- 使用参数在 有shuffle算子中指定
	- 重分区 repatriation() 或者 coalesce()
	
	
	
	影响RDD并行度
	
	- 用户指定（shuffle参数，repatriation）
	- 输入RDD根据底层存储系统选择并行度，
		- 从HDFS读，根据文件数和block大小
	- 从数据混洗后的 RDD 派生下来的 RDD 则会采用与其父 RDD 相同的并行度
	
	
	
	类比就是，多人一起吃蛋糕（数据量），每人一次只能吃一块。人数相当于资源，蛋糕相当于总数据量，切块相当于 partition。蛋糕分的块少，造成只有一些人在吃，而且没人吃的很久; 切的太多，每个人吃的块数太多，反而会慢。
	
	

2. 序列化格式

	序列化在Shuffle的时候发生，默认使用java内建的序列化库，也可以使用轻量的kyro第三方序列化库。

	如果代码中引用了一个没有扩展java的Serializeable接口类，会抛出NotSerializableException 异常

	可以使用一个特别的选项调试这个情况 "-Dsun.io.serialization.extended DebugInfo=true " 

	需要在spark-submit 的  --driver-java-options 和 --executor-java-options 标记来打开这个选项

3. 内存管理

	RDD数据存储（cache，persist） 

	shuffle缓存

	用户代码

	

[Tuning - Spark 3.3.1 Documentation (apache.org)](https://spark.apache.org/docs/latest/tuning.html)





#### 构建工具

使用构建工具，生成单个大 JAR 包，包含应用的所有的传递依赖。这通常被称为超级（uber）JAR 或者组合（assembly）JAR，大多数 Java 或 Scala 的构建工具都支持生成这样的工件。

Java 和 Scala 中使用最广泛的构建工具是 Maven 和 sbt。它们都可以用于这两种语言，不过 Maven 通常用于 Java 工程，而 sbt 则一般用于 Scala 工程。









#### SparkSQL

操作结构化数据的接口

结构信息：每条记录共用的已知的字段的集合。



SparkSQL 功能

1）连接各种结构化数据源（hive，json，parquet）

2）支持JDBC、ODBC协议进行SparkSQL查询

3）SQL支持和自定义函数支持，和API的混合编程。



使用特殊的RDD描述结构化数据 SchemaRDD （ DataFrame）

DataFrame（SchemaRDD）

1. 存放Row对象的RDD，每个Row代表一行记录。

2. 包含结构信息（数据字段），可以利用结构信息高效存储数据，
3. 运行SQL查询。



和Hive的关系

编译时包含Hive支持的SparkSQL ： 支持Hive表的访问，UDF，SerDE，HQL

编译时不包含Hive的支持： 应用于Hive之间发生了依赖冲突，并且无法通过依赖排除异己以来封装解决问题。



#### DataFrame & SchemaRDD

> DataFrame的前身是SchemaRDD，从Spark 1.3.0开始SchemaRDD更名为DataFrame



DataFrame 概念上类似于数据库的表，跟RDD区别时有了Schema

DataFrame : A DataFrame is equivalent to a relational table in Spark SQL

实现机制： 由Row对象组成的RDD，附带每列数据类型的结构信息







Row

Row对只是对基本数据类型的数据的封装。本质上就是一个定长的字段数组。

取值

既然时数组，就是按照下标取值。标准的取值方法get(scala Apply), 读取一个列序号然后返回Object(scala 的 Any）类型对象然后再把对象转为正确的类型。对于基本类型（Boolean Byte Double Float Int Long Short String） 都有getType 方法，可以直接把值作为相应的类型返回。



缓存

使用专门的cacheTable("tableNamee") 这是因为缓存表而不是缓存对象。

缓存数据表时候，可以在内存种列式存储数据。生命周期和Driver相同。

可以通过spark web界面的 Storage看到缓存的表。



##### 读取和存储数据

hive 

可以直接读取hive表，支持任何Hive支持的存储格式(SerDe),包括文本文件，RCFiles, ORC,Parquet Avro 以及 Protocol Buffer



DataFrame读取存储数据





创建DataFrame

1. 基于RDD



2. 读取外部数据源





JDBC/ODBC服务器

JDBC 服务器与 Hive 中的 HiveServer2 相一致,由于使用了Thrift通讯协议，他也被称为Thrift Server。



启动Thrift Server 

`${SPARK_HOME}/sbin/start-thriftserver.sh --master sparkMaste`

默认端口 10000

spark 自带了Beeline客户端程序

`./bin/beeline -u jdbc:hive2://localhost:10000`

 

#### 自定义UDF

sql 方式

```scala
spark.udf.register（"function name", (param01:Type,...) ={}）
```



DSL 方式

```scala
import org.apache.spark.sql.functions._
val xxx_udf = udf (
	(param01: Type,...) ={
        
    }
)
```



SparkSQL 性能

1. 表达聚合、分组聚合更加容易，比如对某列求值，不需要累加器的操作
2. 借助SQ引擎的本身的优化，比如谓词下推，可以把一些筛选条件推到数据存储层，从而减少IO
3. 根据类型信息对查询进行优化
4. 在内存对数据进行列式存储。（列式存储对某几个字段查询减少了IO

要充分利用SQL简洁性和编程语言擅长表达复杂逻辑的优点。



性能选项



###### SparkStreaming













































































