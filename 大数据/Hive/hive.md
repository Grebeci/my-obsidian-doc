## 一、概述

Hive作为一个数仓工具，把 **结构化的** 数据映射成一张表，对外使用类SQL（HiveQL）的语言查询数据。在数仓、数据处理上，会把 Hive 表 当成数据的**数据结构**。

在底层，Hive将 HiveQL 翻译成 MR 程序，再提交给 集群（Yarn） 执行。[[Questions.md#Hive 执行引擎的讨论]]

在HDFS中，数据基本是通过文件存储的，而 Hive 需要把数据文件映射成表，这就需要元数据库支持。 由于这些元数据频繁访问，一般存进数据库里（MySQL,SQLServer)。

---

## 二、Hive 结构

### 2.1 角色

```
                         ┌───────────────────────┐
                         │Clinet                 │
                         │          CLL    JDBC  │
                         ├───────────────────────┤
┌──────────────┐         │        Driver         │
│   metastore  ◄────────►│                       │
└──────────────┘         │                       │
                         │                       │
                         └────────────┬──────────┘
                                      │
                                      │
                           ┌──────────▼─────────┐
                           │      MapReduce     │
                           └──────────┬─────────┘
                                      │
                                      │
                                      │
           ┌───────────────────┐      │
           │      HDFS         ◄──────┘
           └───────────────────┘
```

**用户接口：Client**

CLI（command-line interface）、JDBC/ODBC(jdbc访问hive)、WEBUI（浏览器访问hive）

**驱动器：Driver**

解析器，编译器，优化器，执行器

**HDFS，MapReduce** 

存储，计算

**元数据：Metastore**

默认derby，推荐mysql

### 2.2 Hive 组件

Hive 是 `CS` 架构，在服务端有常驻守护进程： `HiveServer2` 和 `Metastore`。客户端有 **Hive CLI (Command Line Interface)**:，主要包括 ， `bin/hive`（已过时） 和 `Beeline`

```
metastore  ========> metadata(mysql)
    /\
    ||
    || 9083
    ||
hiveserver2
    /\
    ||
    || 10000
    ||
 Client ：beeline jdbc odbc 
```

[[hive运行时服务]]

---


## 三、HQL

### 1. 元数据查询

[[show-desc查询]]

### 2. 建库&建表

**DataBase**

[[HQL语法备忘录#数据库]]
	create
	alter
	
**Table**

[[HQL语法备忘录#表]]
	create

​    TEXTFILE 默认分隔符是 `\001` （SOH字符）   linux vim 显示为 `^A` 

​	alter

### 3. DML

[[Hive-DML]]

### 4. Data 查询

---

## 四、Data Type 

大纲

1. 类型描述
	1. 类型解释，字节数，对应java基类
	2. 继承关系（隐式转换依赖于继承关系）
	3. 字面量
	4. 隐式转换

2. 复杂类型
	1. 含义
	2. 构造
	3. 访问
	4. 注意： 不要使用union类型

3. 官方文档

	[TypeSystem](https://cwiki.apache.org/confluence/display/Hive/Tutorial#Tutorial-TypeSystem)

	[Data Types](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Types)

Hive分为基本数据类型和复杂类型，基本类型包含数值类型，字符串类型，日期/时间类型 

##### Primitive Types

| Hive数据类型 | Java数据类型  | 长度                                                 | 例子                                  |
| ------------ | ------------- | ---------------------------------------------------- | ------------------------------------- |
| TINYINT      | byte          | 1byte有符号整数                                      | 20                                    |
| SMALINT      | short         | 2byte有符号整数                                      | 20                                    |
| INT          | int           | 4byte有符号整数                                      | 20                                    |
| BIGINT       | long          | 8byte有符号整数                                      | 20                                    |
| BOOLEAN      | boolean       | 布尔类型，true或者false                              | TRUE FALSE                            |
| FLOAT        | float         | 单精度浮点数                                         | 3.14159                               |
| DOUBLE       | double        | 双精度浮点数                                         | 3.14159                               |
| STRING       | String        | 字符系列。可以指定字符集。可以使用单引号或者双引号。 | ‘now is the time’  “for all good men” |
| TIMESTAMP    | LocalDateTime | 时间类型                                             |                                       |
| DATE         | LocalDate     | 日期类型                                             |                                       |
| BINARY       |               | 字节数组                                             |                                       |


整型（int, tinyint ,smallint, int, bigint） ， 布尔型（bool） ,浮点型 （float, double，decimal） , 字符串（string,varchar,char）

时间日期类型： 

Timestamp： （在hive中）形如 `"YYYY-MM-DD HH:MM:SS.fffffffff` 字符串，没有时区概念，内部实现是 JDBC的 `java.sql.Timestamp`

Date ：形如`YYYY-MM-DD` 字符串，内部实现是 `java.sql.Date`

Intervals ：时间间隔， 日期加减运算的 time units

在开发中，
1. 建表时侯时候，时间类型都表示为格式化的 `String`， `Unix Timestamp` 表示为 `Bigint` 
2. `Format-String` 都能被 UDF 函数默认 cast
4. timestamp，date 类型是写 sql 转换时间格式用的，转换规则是 `向Timestamp 转换`


##### Complex Types

- arrays: `ARRAY<data_type>`  ： 有序的数据集合

- maps: `MAP<primitive_type, data_type>` ： 键值对的数据集合，键必须是 primitive type ， 值可以是任何类型。这里 `primitive type` 是 [Hive语境下]( https://cwiki.apache.org/confluence/display/Hive/Tutorial#Tutorial-TypeSystem)的

- structs: `STRUCT<col_name : data_type [COMMENT col_comment], ...>`  ： 类似C语言结构体的数据类型，可以存储多个不同名称和类型的字段。


这些类型不建议表示为列类型，在大多场景下，这些类型都是数据处理的中间类型。

如果字段类型必须用这些，那么要求

1. 需要在建表中指定映射方式（构造方式）。

2. 确保文件格式严格正确。

| type                                                      | constructor                          | acess  |
| --------------------------------------------------------- | ------------------------------------ | ------ |
| `ARRAY<data_type>`                                        | array(val1, val2, ...)               | A[n]   |
| `MAP<primitive_type, data_type>`                          | map(key1, value1, key2, value2, ...) | M[key] |
| `STRUCT<col_name : data_type [COMMENT col_comment], ...>` | struct(val1, val2, val3, ...)        | S.x    |

参见：

[Complex Type Constructors](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#:~:text=Hive%202.2.0.-,Complex%20Type%20Constructors,-The%20following%20functions)

[Operators on Complex Types](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-OperatorsonComplexTypes:~:text=the%20tag%20parameter.-,Operators%20on%20Complex%20Types,-The%20following%20operators))


##### 类型转化

[TypeSystem](https://cwiki.apache.org/confluence/display/Hive/Tutorial#Tutorial-TypeSystem)

hive的原子类型是可以进行隐式转换的，类似于java的类型转换。

1. 隐式转换的规则

	如上面官网链接上的对类型继承的层次描述，原则是，子类型可以转为祖先类型

2. 可以使用 `cast` 操作显式进行数据类型的转换

	`cast(expr as <type>)`

	如果转换失败，返回 `NULL`



##### NULL 相关

hive对 NULL的处理： Hive默认 `NULL` 存储为 `\N`， 在存储为 `TEXTFILE` 格式时，可以用 `row format NULL DEFINED AS` `char` 指定。

注意区分字符串的  `"NULL"`， 空字符串 `""`  字符串 `"\N"` , 关键字 `NULL`  的语义。

**处理**

- 使用 `IS NULL`  和 `IS NOT NULL` 运算符来检查值是否为 NULL，使用 coalsece 和nvl 处理NULL值。

- 处理函数

| Return Type | name                          | description                                                  |
| ----------- | ----------------------------- | ------------------------------------------------------------ |
| T           | nvl(T value, T default_value) | Returns default value if value is null else returns value (as of HIve [0.11](https://issues.apache.org/jira/browse/HIVE-2288)). |
| T           | COALESCE(T v1, T v2, ...)     | Returns the first v that is not NULL, or NULL if all v's are NULL. |

- 在聚合函数 （sum，avg，max）中，NULL值会被忽略。


## 五、元数据

#### 1. 元数据查询

见上

#### 2. 内部表外部表

默认创建表不加 `external` 关键字的是内部表。hive会控制着数据的生命周期。

- 删除内部表，下面的数据也会随之删除。如果删除一个分区， 分区文件夹和分区数据也会删除
- 对于外部表，删除表只会删除元数据，表的数据不会被删除，如果删除一个分区，只会删除分区的元数据信息。


内部表和外部表互相转换

```mysql
alter table table_namee set tblproperties('EXTERNAL'='TRUE');
```
注意大小写敏感：`'EXTERNAL'='TRUE'`


查看表的类型

使用 `desc formatted tbl_name` 查看表类型。

- `Table Type:       EXTERNAL_TABLE` 内部表
- `Table Type:       MANAGED_TABLE` 外部表


#### 3. 分区

分区表实际上就是对应一个HDFS文件系统上的独立的文件夹，该文件夹下是该分区所有的数据文件。

分区的文件夹的名称是： `partition_col=value`



##### insert data into partition 

[[Hive-DML#insert data into partition]]

- 如果是分区表，必须指定分区字段
- 动态分区
#### 4. 元数据的存储

Hive描述表结构是按照每个分区存储的，也就是说，每个分区的列可能不同。

> 使用 `alter table [add/drop] columns` 更改表结构时候，Hive也会在元数据中记录该分区与表级别不同的列结构信息。因为对外表现是每个分区的列结构信息不完全一样。

[[hive使用经验#2.alter table add column 大量的字段（原表超过1000个字段，分区超过1000个）]]


## 六、压缩 和存储

>这个地方少了一个压缩存储的总结

orc => lzo

parquet => snappy

orc  =>snappy (度小满)

具体测试

> 这里要有个实际工作经验的例子的。还有一些实践数据

## 七、优化

> 优化这个地方应该有个总结，但是 Hive的优化做的很少，没有经过实验和实践，所以这里是有个todo的
>
> - 先把所有的优化，官网上，博客上的总结的优化手段收集起来
> - 验证，包括通过做实验或者实际工作的经验。必须明确他们的优化手段的有效性和收益
> - 明确优化的手段的原理
> - 原则性的不叫优化，分开总结。

## Hive 错误认识，用法

由于元数据是分开存储的，所以数据的path可以不在表路径下。这可能会引发一些问题。

对于内外部表

add partition

drop partition 

load data



## 八、面试题

[[hive面试题]]

