### Document


Hudi 是什么？要解决什么问题 ？

Apache Hudi 是一个支持插入、更新、删除的增量数据湖处理框架。



Hudi 对数仓增加了一个功能 `快速upsert`。 对于当前的数仓，都将hive当成数据结构，对于只是更改一部分数据(upsert, delete )这个操作，hive表只能提供 overwrite 操作。至少要扫全表或者扫特定的分区，这个代价极大。而在关系数据库，在主键，事务的保证下，upsert是简单快速的操作。

对于用 hive表（基于hadoop）为数据结构的数仓，快速upsert关键在于怎么找到记录所在的文件，因此，Hudi采用索引+主键的办法，每个记录都要定义主键，然后对主键建立索引，索引映射规则是 记录键 => 文件id。如果想upsert记录，先找主键，然后算出索引对应的fileId，只对特定文件更改就行了。

upsert 每次更改特定的文件还是代价很大，于是有了两种表，是写log还是写文件。如果写文件，就是COW，写会很慢。如果写日志，异步或者读的时候merge文件，那就是MOR。因此，COW，MOR两种类型的表是不同的实现，他们会选择不同的文件布局。



### 1. 核心概念

> 官方文档不要看中文版的，和英文版的有很多不一样的地方。。
>
> - 查询类型（中英不一样？）

##### TimeLine

[概念-时间轴 | Apache Hudi](https://hudi.apache.org/cn/docs/next/overview#时间轴)

##### 文件布局

[概念-文件组织 | Apache Hudi](https://hudi.apache.org/cn/docs/next/overview#文件组织)】

[File Layouts | Apache Hudi](https://hudi.apache.org/docs/next/file_layouts/)

上面面是官方的标准解释，太糊了，重点是看 `亚马逊AWS官方博客` 对 File Layouts 解释，不懂的借助AI。

[File Layouts | 亚马逊AWS官方博客](https://aws.amazon.com/cn/blogs/china/exploring-apache-hudi-core-concepts-with-amazon-emr-studio-1-file-layouts/)： 这篇文章先看 `3. 核心概念` , 然后表概念的时候在回过来看 `4. COW 表的 File Layouts` `5. MOR 表的 File Layouts`

##### 表类型和查询类型

也可以叫做存储类型，主要是读写状态下，数据怎么组织。

注意，不同的表类型选择不同的布局方式。也就是说，布局方式是读写的存储的特定实现。

[Table & Query Types | Apache Hudi](https://hudi.apache.org/docs/next/table_types#query-types)

上面官网的没有例子。注意不要看中文版的。

例子- 亚马逊 `4. COW 表的 File Layouts` `5. MOR 表的 File Layouts`

##### Key Generation

[Key Generation | Apache Hudi](https://hudi.apache.org/docs/key_generation)

看官网就行，hudi实现 快速upsert，完整性约束都是靠 主键特性实现的。



##### Index

[Indexing | Apache Hudi](https://hudi.apache.org/docs/indexing)

官网的解释，对记录键进行索引，进行高效的upsert。



### 2.操作

hive 

spark

flink





