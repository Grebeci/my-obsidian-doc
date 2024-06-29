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


#### hive 和spark的区别

### Apache Hive

- **定位**：Hive 是一个建立在 Hadoop 上的数据仓库软件，用于提供数据汇总、查询和分析。虽然 Hive 最初是设计用来在 Hadoop 的 MapReduce 上运行的，但它现在也支持其他执行引擎，如 Tez 和 Spark。
- **守护进程**：Hive 提供了如 HiveServer2 和 Metastore 这样的守护进程服务。这些服务支持多用户环境中的并发查询和元数据管理。
- **SQL 接口**：Hive 使用类 SQL 语言（HiveQL），允许用户编写查询语句来执行数据操作，这些查询语句会被转换成底层数据处理平台（如 MapReduce、Tez、Spark）可执行的任务。

### Apache Spark

- **定位**：Spark 是一个通用的分布式数据处理系统，不仅支持批处理，也支持流处理、机器学习和图计算等。Spark 设计目标是提高大规模数据处理的速度和简化开发过程。
- **运行时**：虽然 Spark 本身不像 Hive 那样常驻守护进程，但它在运行时会启动各种服务，例如 Spark Context、Driver 和 Executors。这些组件一起在集群中协调和执行数据处理任务。
- **计算模型**：Spark 的核心是一个高速的内存计算能力和一个优化的执行计划。用户编写的程序通常包装在一个或多个 Spark 作业中，这些作业由 Spark 的调度器进行管理和执行。

### 核心区别

- **架构**：Hive 更像是构建在 Hadoop 生态之上的高层数据仓库工具，提供 SQL 层面的抽象；而 Spark 是一个更底层的数据处理框架，提供广泛的数据处理能力和算法库。
- **守护进程与独立运行**：Hive 的操作依赖于它的守护进程，如 HiveServer2 和 Metastore，这些进程为查询执行提供持久的服务；Spark 则在应用程序提交时动态创建执行环境，执行完毕后释放资源。
- **用途和适用性**：Hive 更适用于那些需要建立在大数据平台上的、标准 SQL 查询驱动的数据仓库应用；而 Spark 适用于需要复杂数据处理、实时分析和机器学习的场景。

总的来说，虽然 Hive 和 Spark 在一些环境中可以进行相互操作（如使用 Spark 作为 Hive 的执行引擎），但它们设计的出发点、主要功能和优化方向存在明显的不同。