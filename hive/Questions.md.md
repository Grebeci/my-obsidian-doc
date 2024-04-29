#### Hive 执行引擎的讨论

执行引擎是不一定是MR，也可能是 Spark、Tez。由此有两种架构，

**Hive On Spark ：** 这个已经不流行了。

- 生态位竞争： Spark-SQL 出现后，Spark 已有 SQL 引擎了，随着 Spark SQL的功能不断增强，Hive on Spark的独特优势减少。

- 运维复杂：同时维护 Hive 、Spark 配置，运维成本高。

- SQL 语义不一致导致的问题，（目前还没有对应的业务案例，待补充 #/programs/hive/hiveonspark），

  可以参考：

  spark 文档对hive的支持部分的解释，[Compatibility with Apache Hive](https://spark.apache.org/docs/2.4.5/sql-migration-guide-hive-compatibility.html).

  hive 文档对hive on spark 架构解释： https://cwiki.apache.org/confluence/display/Hive/Hive+on+Spark

  hive 文档对 hive on spark 配置 ： [Hive+on+Spark:+Getting+Started#HiveonSpark:GettingStarted-VersionCompatibility](ttps://cwiki.apache.org/confluence/display/Hive/Hive+on+Spark:+Getting+Started#HiveonSpark:GettingStarted-VersionCompatibility)

**Hive on Tez:** 这个国内不太流行（还未找到案例）。

改进的点 ： https://stackoverflow.com/questions/41630987/tez-execution-engine-vs-mapreduce-execution-engine-in-hive

- 使用内存进行传输数据，减少MR内部或者MR之间的数据落盘。
- 使用DAG灵活安排数据操作。

> 现代数据处理框架（如 Tez 或 Spark）试图优化这些操作，通过智能调度和执行来减少数据传输和中间存储。这些框架能够将多个逻辑操作合并到一个物理执行计划中，尽可能地减少作业数量和中间数据的写入。

AI参考 ： [GPT-Response (github.com)](https://gist.github.com/Grebeci/f68eff6c526aaafb6001ff2dc4b89b48)