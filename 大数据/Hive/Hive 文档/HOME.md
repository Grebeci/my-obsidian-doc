下面都来自于官网的文档，这是要求必须记住，并且背诵的部分。

hive的要求记住的所有知识都会平移到这里，主要来自于官网的手册。

这个文档主要是由于面试过程中，发现基础一般，所以用这种手段巩固基础。

-----------

# Apache Hive

Apache Hive™数据仓库软件便于读取、写入和管理驻留在分布式存储中的大型数据集，并使用SQL语法进行查询。

Hive建立在Apache Hadoop™之上，提供以下特性:

- 能够通过SQL轻松访问数据的工具，从而支持数据仓库任务，如提取/转换/加载(ETL)、报告和数据分析。
- 一种在各种数据格式上施加结构的机制
- 访问直接存储在Apache HDFS™或其他数据存储系统(如Apache HBase™)中的文件 
- 通过Apache Tez™、Apache Spark™或 MapReduce 执行查询
- HPL-SQL过程语言与HPL-SQL
- 亚秒查询检索通过Hive LLAP, Apache YARN和Apache Slider。

Hive提供了标准的SQL功能，包括许多SQL:2003、SQL:2011和SQL:2016的新特性。

Hive的SQL也可以通过用户定义函数(UDFs)、用户定义聚合(UDAFs)和用户定义表函数(UDTFs)来扩展用户代码。



没有单一的“Hive格式”来存储数据。Hive带有内置连接器，用于逗号和制表符分隔值(CSV/TSV)文本文件，Apache Parquet™，Apache ORC™和 其他格式。用户可以使用其他格式的连接器扩展Hive。详细信息请参见开发人员指南中的文件格式和Hive SerDe。

不是为联机事务处理(OLTP)工作负载设计的。它最适合用于传统的数据仓库任务。

Hive的设计是为了最大化可伸缩性(随着更多的机器动态添加到Hadoop集群中)、性能、可扩展性、容错性和输入格式的松耦合。

Hive的组件包括HCatalog和WebHCat。

- HCatalog是Hadoop的一个表和存储管理层，它允许用户使用不同的数据处理工具(包括Pig和MapReduce)更容易地在网格上读写数据。

  > HCatalog 是一个元数据和表管理服务，并非 Hive 的必须组件，目前生产集群不用。

- WebHCat提供了一个服务，你可以使用它来运行Hadoop MapReduce(或YARN)、Pig、Hive作业。也可以通过HTTP (REST风格)接口对Hive元数据进行操作。

  > 目前还未有实践案例。

# Hive Documentation

下面的链接提供了对Apache Hive wiki文档的访问。此列表并不完整，但您可以浏览这些wiki页面以查找其他文档。更多信息请参见Hive官方网站。

## Hive基本信息

- [Getting Started开始](https://cwiki.apache.org/confluence/display/Hive/GettingStarted)
- [Books about Hive关于Hive的书籍](https://cwiki.apache.org/confluence/display/Hive/Books+about+Hive)
- [Presentations and Papers about Hive关于Hive的演讲和论文](https://cwiki.apache.org/confluence/display/Hive/Presentations)
- [Sites and Applications Powered by HiveHive支持的站点和应用程序](https://cwiki.apache.org/confluence/display/Hive/PoweredBy)
- [Related Projects相关项目](https://cwiki.apache.org/confluence/display/Hive/RelatedProjects)
- [FAQ常见问题解答](https://cwiki.apache.org/confluence/display/Hive/User+FAQ)
- [Hive Users Mailing ListHive用户邮件列表](http://hive.apache.org/mailing_lists.html#Users)
- [About This Wiki关于本Wiki](https://cwiki.apache.org/confluence/display/Hive/AboutThisWiki)

## User Documentation用户文档

- [Hive Tutorial](https://cwiki.apache.org/confluence/display/Hive/Tutorial)
- [Hive SQL Language Manual](https://cwiki.apache.org/confluence/display/Hive/LanguageManual):  [Commands](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Commands), [CLIs](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli), [Data Types](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Types)，
  DDL ([create/drop/alter/truncate/show/describe](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL)), [Statistics (analyze)](https://cwiki.apache.org/confluence/display/Hive/StatsDev), [Indexes](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Indexing), [Archiving](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Archiving),
  DML ([load/insert/update/delete/merge](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DML), [import/export](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ImportExport), [explain plan](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Explain)),
  [Queries (select)查询(选择)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Select), [Operators and UDFs](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF), [Locks](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Locks), [Authorization](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Authorization)
- [File Formats and Compression](https://cwiki.apache.org/confluence/display/Hive/FileFormats):  [RCFile](https://cwiki.apache.org/confluence/display/Hive/RCFile), [Avro](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe), [ORC](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC), [Parquet](https://cwiki.apache.org/confluence/display/Hive/Parquet); [Compression](https://cwiki.apache.org/confluence/display/Hive/CompressedStorage), [LZO
- Procedural Language: [Hive HPL/SQL](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=59690156)
- [Hive Configuration PropertiesHive Configuration](https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties)
- Hive Clients
  - [Hive Client](https://cwiki.apache.org/confluence/display/Hive/HiveClient) ([JDBC](https://cwiki.apache.org/confluence/display/Hive/HiveClient#HiveClient-JDBC), [ODBC](https://cwiki.apache.org/confluence/display/Hive/HiveClient#HiveClient-ODBC), [Thrift](https://cwiki.apache.org/confluence/display/Hive/HiveClient#HiveClient-ThriftJavaClient))Hive Client (JDBC, ODBC, Thrift)
  - HiveServer2:  [Overview](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Overview), [HiveServer2 Client and Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients), [Hive Metrics](https://cwiki.apache.org/confluence/display/Hive/Hive+Metrics)
- [Hive Web InterfaceHive](https://cwiki.apache.org/confluence/display/Hive/HiveWebInterface)
- [Hive SerDes](https://cwiki.apache.org/confluence/display/Hive/SerDe):  [Avro SerDe](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe), [Parquet SerDe](https://cwiki.apache.org/confluence/display/Hive/Parquet#Parquet-HiveQLSyntax), [CSV SerDe](https://cwiki.apache.org/confluence/display/Hive/CSV+Serde), [JSON SerDe](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-JSON)  
- [Hive Accumulo IntegrationHive Accumulo集成](https://cwiki.apache.org/confluence/display/Hive/AccumuloIntegration)
- [Hive HBase IntegrationHive HBase集成](https://cwiki.apache.org/confluence/display/Hive/HBaseIntegration)
- [Druid Integration德鲁伊的集成](https://cwiki.apache.org/confluence/display/Hive/Druid+Integration)
- [Kudu Integration捻角羚集成](https://cwiki.apache.org/confluence/display/Hive/Kudu+Integration)
- [Hive Transactions](https://cwiki.apache.org/confluence/display/Hive/Hive+Transactions), [Streaming Data Ingest](https://cwiki.apache.org/confluence/display/Hive/Streaming+Data+Ingest), and [Streaming Mutation API](https://cwiki.apache.org/confluence/display/Hive/HCatalog+Streaming+Mutation+API) Hive Transactions, Streaming Data Ingest, Streaming Mutation API
- [Hive Counters 蜂巢计数器](https://cwiki.apache.org/confluence/display/Hive/HiveCounters)
- [Using TiDB as the Hive Metastore database 使用TiDB作为Hive Metastore数据库](https://cwiki.apache.org/confluence/display/Hive/Using+TiDB+as+the+Hive+Metastore+database)
- [StarRocks IntegrationStarRocks集成](https://cwiki.apache.org/confluence/display/Hive/StarRocks+Integration)

# Hive版本和分支

Hive 的版本通常和 Hadoop版本一起更新，选择 Hive版本，注意兼容性。