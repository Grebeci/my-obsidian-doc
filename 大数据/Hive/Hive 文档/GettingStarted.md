[TOC]

## 安装与配置

您可以通过下载tarball来安装Hive的稳定版本，或者您可以下载源代码并从中构建Hive。



> Tarball 是一种文件归档格式，通常用于在 Unix 和 Linux 系统中打包和分发多个文件。这种格式使用 `.tar` 文件扩展名，并且可以通过额外的压缩（如 gzip 或 bzip2）来减小文件大小，此时扩展名变为 `.tar.gz` 或 `.tar.bz2`。



## 运行 `HiveServer2` 和 `Beeline`

### Requirements

- Java 1.7
- Hadoop 2.x (preferred), 1.x (not supported by Hive 2.0.0 onward).Hadoop 2。
- Hive主要用于Linux和Windows生产环境。Mac是一种常用的开发环境。本文档中的说明适用于Linux和Mac。在Windows上使用它的步骤略有不同。

### Installing Hive from a Stable Release

首先从Apache下载镜像下载Hive的最新稳定版本((see [Hive Releases](https://hive.apache.org/downloads.html)).)。

接下来，您需要解压缩tarball。这将导致创建名为hive-x.y的子目录。Z(其中X.Y.Z为发行号):

```
  $ tar -xzvf hive-x.y.z.tar.gz
```

设置环境变量HIVE_HOME指向安装目录:

```
  $ cd hive-x.y.z
  $ export HIVE_HOME={{pwd}}
```

最后，将`$HIVE_HOME/bin`添加到你的`PATH`中:

```
  $ export PATH=$HIVE_HOME/bin:$PATH
```

### Building Hive from Source

具体请参见了解Hive分支。

> 没有摘抄，这段还没有实践过，具体参考官网具体内容。



### Running Hive运行蜂巢

Hive 使用 Hadoop，所以:

- 必须在 `PATH` 中有Hadoop。
- `export HADOOP_HOME=<hadoop-install-dir>`

此外，您必须使用以下HDFS命令创建`/tmp`和 `/user/hive/warehouse` (也称为`hive.metastore.warehouse.dir`)，并将它们设置为`chmod g+w`，才能在hive中创建表。

```
  $ $HADOOP_HOME/bin/hadoop fs -mkdir       /tmp
  $ $HADOOP_HOME/bin/hadoop fs -mkdir       /user/hive/warehouse
  $ $HADOOP_HOME/bin/hadoop fs -chmod g+w   /tmp
  $ $HADOOP_HOME/bin/hadoop fs -chmod g+w   /user/hive/warehouse
```

> 每次初始化 Hive 库 ，必须保证这些目录是空的或者不存在。
>
> 这个有个问题是，使用 schematool 初始化数据库是否会创建默认目录。或者说 schematool 是否涉及 HDFS的操作。

您可能会发现设置 `HIVE_HOME`很有用(尽管不是必需的):

```
  $ export HIVE_HOME=<hive-install-dir>
```

#### 运行Hive命令行

在shell中使用Hive 命令行 [command line interface](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) (CLI) 接口。

```shell
  $ $HIVE_HOME/bin/hive
```

#### 运行 `HiveServer2` 和 `Beeline`

从Hive 2.1开始，我们需要运行下面的schematool命令作为初始化步骤。例如，我们可以使用“derby”作为数据库类型。

```shell
  $ $HIVE_HOME/bin/schematool -dbType <db type> -initSchema
```

> 这个命令需要记住。



[HiveServer2](https://cwiki.apache.org/confluence/display/Hive/Setting+Up+HiveServer2) (在Hive 0.11中引入)有自己的CLI，称为 [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients). 。HiveCLI现在被Beeline取代了，因为它缺乏HiveServer2的多用户、安全性和其他功能。从shell运行HiveServer2和Beeline:

```
  $ $HIVE_HOME/bin/hiveserver2

  $ $HIVE_HOME/bin/beeline -u jdbc:hive2://$HS2_HOST:$HS2_PORT
```

> 这个命令需要记住



由HiveServer2的JDBC URL启动，它取决于启动HiveServer2的地址和端口。默认情况下，它将是(localhost:10000)，因此地址看起来像`jdbc:hive2://localhost:10000。`



或者在相同的进程中启动 `Beeline` 和 `HiveServer2` 用于测试目的，以获得与 `HiveCLI` 类似的用户体验: 

```
  $ $HIVE_HOME/bin/beeline -u jdbc:hive2://
```

> 这个的意思是不启动 hiveserver2 ，beeline 进程是否会自动启动 hiveserver2
>
> 这个存疑，还没有实验过，需要验证下。



### 配置管理简介

- Hive默认从  `<install-dir>/conf/hive-default.xml` 获取配置。
- 配置目录的位置可以通过设置 `HIVE_CONF_DIR` 环境变量来改变。
- 可以通过(重新)在 `<install-dir>/conf/hive-site.xml` 中定义配置变量来更改它们。
- Log4j配置存储在`<install-dir>/conf/hive-log4j.properties`中。

- Hive配置是Hadoop的一个覆盖层——默认情况下它继承了Hadoop的配置变量。

- Hive配置可以通过以下方式进行操作:

  - 编辑`hive-site.xml`并在其中定义任何所需的变量(包括Hadoop变量)。

  - 使用set命令(参见下一节)

  - 调用Hive(已弃用)，Beeline或HiveServer2使用语法:

    ```
    $ bin/hive --hiveconf x1=y1 --hiveconf x2=y2  //这将变量x1和x2分别设置为y1和y2
    ```

    ```
    bin/hiveserver2 --hiveconf x1=y1 --hiveconf x2=y2  // 这将服务器端变量x1和x2分别设置为y1和y2
    ```

    ```shell
    bin/beeline --hiveconf x1=y1 --hiveconf x2=y2  //这将客户端变量x1和x2分别设置为y1和y2。
    ```

### Runtime Configuration运行时配置

- Hive查询使用 `map-reduce` 查询执行，因此，此类查询的行为可以由Hadoop配置变量控制。

- HiveCLI(已弃用)和Beeline命令'SET'可用于设置任何Hadoop(或Hive)配置变量。例如:

  ```
      beeline> SET mapred.job.tracker=myhost.mycompany.com:50030;
      beeline> SET -v;
  ```

  后者显示所有当前设置。如果没有-v选项，则只显示与Hadoop基本配置不同的变量。

> mapred.job.tracker 控制如何提交到hadoop集群，这个配置不用记忆，只是作为一个例子



### Hive Logging 

Hadoop MapReduce 任务的日志存储在执行任务的节点上，HiveServer2 的日志存储在运行 HiveServer2 的服务器上，而 Beeline 的日志则存储在运行 Beeline 客户端的机器上。

缺省情况下，CLI不会将日志发送到控制台。Hive 默认的日志级别是 `INFO`。

日志存放在 :  ``/tmp/<*user.name*>`:` 目录下。

要配置不同的日志位置，设置`hive.log.dir`在 `$HIVE_HOME/conf/hive-log4j.properties`中。确保目录设置了sticky位(`chmod 1777 <*dir*>`)。

- `hive.log.dir=<other_location>`

如果用户愿意，可以通过添加如下所示的参数将日志发送到控制台:

- `bin/hive --hiveconf hive.root.logger=INFO,console  //for HiveCLI (deprecated)`
- `bin/hiveserver2 --hiveconf hive.root.logger=INFO,console`

> 关于日志级别、RollingPolicy、本地模式，参考文档。





## DDL操作

### 创建Hive表

```
  hive> CREATE TABLE pokes (foo INT, bar STRING);
```

创建一个名为pokes的表，其中有两列，第一列是整数，另一列是字符串。

```
  hive> CREATE TABLE invites (foo INT, bar STRING) PARTITIONED BY (ds STRING);
```

创建一个名为“invites”的表，其中包含两列和一个名为ds的分区列。分区列是一个虚拟列。它不是数据本身的一部分，而是来自装载特定数据集的分区。

默认情况下，假定表为文本输入格式，并且假定分隔符为`^A`(ctrl-a)。

### Browsing through Tables浏览表格

```
  hive> SHOW TABLES;
```

列出所有的表。

```
  hive> SHOW TABLES '.*s';
```

列出所有以's'结尾的表。模式匹配遵循Java正则表达式。请查看此文档链接http://java.sun.com/javase/6/docs/api/java/util/regex/Pattern.html。

```
hive> DESCRIBE invites;
```

显示列列表。

### Altering and Dropping Tables

表名可以更改，列可以添加或替换:

```
  hive> ALTER TABLE events RENAME TO 3koobecaf;
  hive> ALTER TABLE pokes ADD COLUMNS (new_col INT);
  hive> ALTER TABLE invites ADD COLUMNS (new_col2 INT COMMENT 'a comment');
  hive> ALTER TABLE invites REPLACE COLUMNS (foo INT, bar STRING, baz INT COMMENT 'baz replaces new_col2');
```

替换所有现有列，并且只更改表的模式，而不更改数据。表必须使用本机SerDe。REPLACE COLUMNS也可以用于从表的模式中删除列:

```
  hive> ALTER TABLE invites REPLACE COLUMNS (foo INT COMMENT 'only keep the first column');
```

删除表:

```
  hive> DROP TABLE pokes;
```



> 正常情况下，列最好只 add，不要 DROP、REPLACE。



### Metadata Store元数据存储

Metadata 能嵌入 Hiveserver2 ， 也能单独作为服务进程。看配置



## DML操作

将文本文件中的数据加载到Hive中:

```
  hive> LOAD DATA LOCAL INPATH './examples/files/kv1.txt' OVERWRITE INTO TABLE pokes;
```

将包含以ctrl-a分隔的两列的文件装入pokes表。'LOCAL'表示输入文件位于本地文件系统上。如果省略'LOCAL'，则在HDFS中查找该文件。

关键字'OVERWRITE'表示删除表中的现有数据。如果省略'OVERWRITE'关键字，则数据文件将追加到现有数据集。

NOTES:

- load命令不对模式执行数据验证。

- 如果文件在hdfs中，则将其移动到hive控制的文件系统命名空间中。
  
  Hive目录的根目录由`hive-default.xml`中的`hive.metastore.warehouse.dir`选项指定。我们建议用户在通过Hive创建表之前先创建这个目录。

```
  hive> LOAD DATA LOCAL INPATH './examples/files/kv2.txt' OVERWRITE INTO TABLE invites PARTITION (ds='2008-08-15');
  hive> LOAD DATA LOCAL INPATH './examples/files/kv3.txt' OVERWRITE INTO TABLE invites PARTITION (ds='2008-08-08');
```

上面的两个LOAD语句将数据加载到表邀请的两个不同分区中。表邀请必须通过键ds来创建分区，这样才能成功。

```
  hive> LOAD DATA INPATH '/user/myname/kv2.txt' OVERWRITE INTO TABLE invites PARTITION (ds='2008-08-15');
```

上面的命令将把数据从HDFS文件/目录加载到表中。
注意，从HDFS加载数据将导致移动文件/目录。因此，操作几乎是瞬间完成的。



## SQL OperationsSQL操作

Hive的查询操作请参见[Select](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Select).。



### Example Queries示例查询

下面显示了一些查询示例。它们可以在`build/dist/examples/queries`中找到。
在Hive源代码`ql/src/test/queries/positive`中可以找到更多。



#### SELECTS and FILTERS

```
  hive> SELECT a.foo FROM invites a WHERE a.ds='2008-08-15';
```

从表 `invites` 分区`ds=2008-08-15`的所有行中选择列foo。

注意，在后面的所有示例中，`INSERT`(插入到Hive表、本地目录或HDFS目录中)是可选的。

```
  hive> INSERT OVERWRITE DIRECTORY '/tmp/hdfs_out' SELECT a.* FROM invites a WHERE a.ds='2008-08-15';
```

将`invites`表分区 `ds=2008-08-15` 中的所有行选择到HDFS目录中。结果数据位于该目录下的文件中(取决于 mappers 的数量)。

NOTE:  注意:分区列(如果有)是通过 `*`  来选择的。它们也可以在投影子句中指定。

分区表必须始终在语句的 `WHERE` 子句中选择一个分区。

```
  hive> INSERT OVERWRITE LOCAL DIRECTORY '/tmp/local_out' SELECT a.* FROM pokes a;
```



从poke表中选择所有行到本地目录。

```
  hive> INSERT OVERWRITE TABLE events SELECT a.* FROM profiles a;
  hive> INSERT OVERWRITE TABLE events SELECT a.* FROM profiles a WHERE a.key < 100;
  hive> INSERT OVERWRITE LOCAL DIRECTORY '/tmp/reg_3' SELECT a.* FROM events a;
  hive> INSERT OVERWRITE DIRECTORY '/tmp/reg_4' select a.invites, a.pokes FROM profiles a;
  hive> INSERT OVERWRITE DIRECTORY '/tmp/reg_5' SELECT COUNT(*) FROM invites a WHERE a.ds='2008-08-15';
  hive> INSERT OVERWRITE DIRECTORY '/tmp/reg_5' SELECT a.foo, a.bar FROM invites a;
  hive> INSERT OVERWRITE LOCAL DIRECTORY '/tmp/sum' SELECT SUM(a.pc) FROM pc1 a;
```

选择列的和。也可以使用平均值、最小值或最大值。

#### GROUP BY

```
  hive> FROM invites a INSERT OVERWRITE TABLE events SELECT a.bar, count(*) WHERE a.foo > 0 GROUP BY a.bar;
  hive> INSERT OVERWRITE TABLE events SELECT a.bar, count(*) FROM invites a WHERE a.foo > 0 GROUP BY a.bar;
```

#### JOIN

```
  hive> FROM pokes t1 JOIN invites t2 ON (t1.bar = t2.bar) INSERT OVERWRITE TABLE events SELECT t1.bar, t1.foo, t2.foo;
```

#### MULTITABLE INSERT

```
  FROM src
  INSERT OVERWRITE TABLE dest1 SELECT src.* WHERE src.key < 100
  INSERT OVERWRITE TABLE dest2 SELECT src.key, src.value WHERE src.key >= 100 and src.key < 200
  INSERT OVERWRITE TABLE dest3 PARTITION(ds='2008-04-08', hr='12') SELECT src.key WHERE src.key >= 200 and src.key < 300
  INSERT OVERWRITE LOCAL DIRECTORY '/tmp/dest4.out' SELECT src.value WHERE src.key >= 300;
```

#### STREAMING流媒体

```
  hive> FROM invites a INSERT OVERWRITE TABLE events SELECT TRANSFORM(a.foo, a.bar) AS (oof, rab) USING '/bin/cat' WHERE a.ds > '2008-08-09';
```
