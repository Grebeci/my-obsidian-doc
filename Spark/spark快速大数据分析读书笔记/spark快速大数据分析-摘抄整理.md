### 核心概念

##### 1. 运行时架构

每个spark应用 使用驱动器程序（driver program） 来发起集群上的各种并行操作。driver 包含应用的main函数，并且定义集群的分布式数据集，还对这些分布式数据集做了相关操作。

driver节点 管理多个 executor节点， 并且算子会在 Executor 上并行执行。





描述一个spark 应用（application） 专用的术语。

- 根据Spark官方文档，当你在Spark中调用一个行动操作（如save()、collect()等）时，Spark会生成一个作业（Job）。作业是由许多任务（Task）组成的并行计算。每个作业会被分为更小的任务集合，即执行阶段（Stage）。执行阶段之间会存在依赖关系。
- job(作业)：spark application 会被分解为job，然后在Driver端将每个job转为一个DAG。DAG本质上就是spark的执行计划。
- stage(执行阶段)： DAG的节点，根据操作能否并行或必须串行创建。

- task是发送到 executor最小的执行单元。一个task对应一个core和partition。



###### SparkContext

sparkContext ：这个对象代表对计算集群的一个连接，可以通过这个对象创建RDD。



###### SparkSession 

spark环境对象，spark-shell 中已经初始化 SparkSession 对象 spark

###### SparkAPI

spark API 特点，利用函数式编程特性传递函数。

例如：

` lines.filter(line=> line.contains("Python"))`

filter 基于函数得操作也会在集群上并行执行，也就是说，spark会将 `line=> line.contains("Python")` 这个函数分发到各个 Executor 上，这样我们可以在 单一得 driver端编程，并让代码自动运行在多个节点上。

> 哪些代码在 driver上执行，哪些在Executor执行？
>
> 以前认为driver 执行算子外面的程序，算子和算子内部得代码分发到每个 Executor ，和这里理解得基本一致。



转化操作：DataFrame 转为新的DataFrame。所有的转化操作都是惰性求值的。spark会记录下来血缘（lineage），便于后续优化DAG（比如合并转化操作）。

行动操作：触发记录下来的所有转化操作的实际求值。



宽转化和窄转化：

- 窄转化，输出中的单个数据分区由单个输入分区计算得来的，不需要跨分区操作数据
- 宽转化，宽转化要从其他分区读取数据并进行整合，需要shuffle操作。



SparkUI

默认4040端口





spark 容错性：

- 血缘和数据不可变的 性质则让 Spark 具有容错性。因为 Spark 在血缘中记录了所有的转化操作，而 DataFrame 在转化操作间是不可变的，所以 Spark 可以通过重放记录下来的血缘来重新构建出 DataFrame，这样 Spark 便能够在失败时依然有机会恢复。









