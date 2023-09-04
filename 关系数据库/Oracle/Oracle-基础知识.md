为了扎实的掌握基础知识，基本上是对书籍的摘抄和整理。这部分需要背诵并且理解。

### 1. 产品描述

- Oracle 有哪些版本，每个版本的迭代的功能是什么 ? 有什么区别
- 现在广泛用于生产环境的是哪个版本，需要学习哪个版本？

官方文档：

[Oracle Database Database Concepts, 19c](https://docs.oracle.com/en/database/oracle/oracle-database/19/cncpt/index.html#Oracle%C2%AE-Database)


### 2. 安装

目前，主流的教程都是在windows安装 Oracle11g的版本，安装复杂。

在 Linux安装需要一系列的配置，可以脚本化。

Getting Startted : 云市场镜像部署Oracle数据库。[如何使用云市场镜像部署Oracle数据库](https://help.aliyun.com/zh/ecs/use-cases/the-cloud-market-image-deployment-of-oracle-database#section-f7o-i2d-kta)


##### 客户端工具

**Oracle Enterprise Manager**

Oracle数据库的企业管理器 (Oracle Enterprise Manager ), OE Manager 还提供的以web界面形式管理Oracle数据库工具

**SQLplus ：**

命令行工具，可以连接本地或者远程的数据库。


### 3. OverView

DBMS 和 RDBMS 的各自的定义和区别

[About Relational Databases](https://docs.oracle.com/en/database/oracle/oracle-database/19/cncpt/introduction-to-oracle-database.html#CNCPT-GUID-166C1E31-CDBC-47D9-867A-3D4C9AAC837D)


##### 3.1 Schema Objects

RDBMS 的一个特点是物理数据存储独立于逻辑数据结构。

在 Oracle 数据库中，数据库模式是逻辑数据结构或模式对象的集合。数据库用户拥有一个数据库模式，其名称与用户名相同。

模式对象是用户创建的结构，直接引用数据库中的数据。数据库支持多种类型的模式对象，其中最重要的是表和索引。



#### 3.2 Data Access

SQL

PL/SQL

Oracle 的 **SQL** 扩展，主要用于编写存储过程，(自定义）函数。提供服务器编程的功能。


#### 3.3 Transaction Management

##### Transactions

事务是包含一条或多条 SQL 语句的 **逻辑原子单元**。

RDBMS 必须能够对 SQL 语句进行分组，使它们要么全部提交（即应用到数据库），要么全部回滚（即撤销）。


##### Data Concurrency 数据并发性

多用户 RDBMS 的一个要求是控制数据并发，即多个用户同时访问同一数据。

如果多个用户访问相同的数据，那么管理并发性的一种方法就是让用户等待。

Oracle 数据库使用锁来控制对数据的并发访问。锁是一种机制，可防止访问共享资源的事务之间发生破坏性交互。锁有助于确保数据完整性，同时允许最大限度地并发访问数据。

##### Data Consistency 数据一致性

在 Oracle 数据库中，每个用户都必须看到一致的数据视图，包括用户自己的事务和其他用户已提交事务的可见更改。

例如，数据库必须防止脏读（dirty read）问题，即一个事务看到另一个并发事务做出的未提交更改。

Oracle 数据库始终执行语句级读取一致性，这保证了单个查询返回的数据在单个时间点上的提交和一致性。根据事务隔离级别的不同，这个时间点是语句打开的时间或事务开始的时间。Oracle 闪回查询功能可让您明确指定这个时间点。

数据库还可以为事务中的所有查询提供读一致性，即事务级读一致性。在这种情况下，事务中的每条语句都会从相同的时间点（即事务开始的时间）查看数据。


### 4. 文档结构

[Oracle DOC](https://docs.oracle.com/en/database/oracle/oracle-database/19/cncpt/introduction-to-oracle-database.html#CNCPT-GUID-C756D70F-75D4-4234-BEB5-CB05A4742128)

必读：

[Oracle Database 2 Day Developer's Guide](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/19/cncpt&id=GUID-AE8B7A74-AD1F-4474-B0CF-B3E95D075DDA)

[Database Concepts](https://docs.oracle.com/en/database/oracle/oracle-database/19/cncpt/preface.html#GUID-B1BA6EA9-3CB2-4BFB-B21A-C157887F68B7)




### Oracle 架构

多租户架构，一个oracle实例可以存放多个数据库。

分片架构：  Oracle Sharding 是一种数据库扩展技术，它基于在多个 Oracle 数据库之间对数据进行水平分区。应用程序将数据库池视为单个逻辑数据库。
	
  每个数据库托管到一个专用节点上，这种配置下，每个数据库是一个分片，所有分片共同组成一个逻辑数据库。称为分片数据库。

   水平分区是指将数据库表分割成多个分区，每个分区包含的表列数相同，但行的子集不同。以这种方式分割的表也称为分片表。
