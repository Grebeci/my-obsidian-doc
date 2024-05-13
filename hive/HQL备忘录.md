前提：

1. desc 是 describe 的简写。
2. DATABASE 和 SCHEMA 等价。
3. identifier_with_wildcards ： 只能选择 * 或 |  。这个也受版本影响。
###  数据库

#### 1. 创建数据库：

```sql
create database [if not exists] database_name
comment database_comment
location hdfs_path
with dbproperties( property_name=property_value)  --  dbproperties 一般是不写
```

#### 2. 数据库级管理的元数据

```sql
# 显示数据库
show databases;

SHOW DATABASES [LIKE 'identifier_with_wildcards'];

# 展示数据库详细信息
DESC DATABASE [EXTENDED] db_name;

# 检查当前是哪个数据库
SELECT current_database()
```

#### 3. 修改

```sql
# 低版本hive只能修改DBPROPERTIES
ALTER DATABASE database_name SET DBPROPERTIES (property_name=property_value, ...); 
```

#### 4. 删除

```sql
DROP DATABASE [IF EXISTS] database_name [RESTRICT|CASCADE];
```

### 表

#### 1. 创建

```sql
CREATE  [EXTERNAL] TABLE [IF NOT EXISTS] [db_name.]table_name    
  [(col_name data_type  [COMMENT col_comment], ... [constraint_specification])]  -- constraint_specification 无效
  [COMMENT table_comment]
  [PARTITIONED BY (col_name data_type [COMMENT col_comment], ...)]
  [CLUSTERED BY (col_name, col_name, ...) [SORTED BY (col_name [ASC|DESC], ...)] INTO num_buckets BUCKETS]
  [
   [ROW FORMAT row_format] 
   [STORED AS file_format]
     | STORED BY 'storage.handler.class.name' [WITH SERDEPROPERTIES (...)]  
  ]
  [LOCATION hdfs_path]
  [TBLPROPERTIES (property_name=property_value, ...)]   
  [AS select_statement];  
  
  
  # 复制已经存在的表结构
CREATE [TEMPORARY] [EXTERNAL] TABLE [IF NOT EXISTS] [db_name.]table_name
  LIKE existing_table_or_view_name
  [LOCATION hdfs_path];
  
# 解释
#1. 数据类型

  data_type
      : primitive_type
      | array_type
      | map_type
      | struct_type
      | union_type
  
  primitive_type
      : TINYINT
      | SMALLINT
      | INT
      | BIGINT
      | BOOLEAN
      | FLOAT
      | DOUBLE
      | DOUBLE PRECISION 
      | STRING
      | BINARY      
      | TIMESTAMP   
      | DECIMAL     
      | DECIMAL(precision, scale)  
      | DATE        
      | VARCHAR     
      | CHAR       
   
array_type
  : ARRAY < data_type >
 
map_type
  : MAP < primitive_type, data_type >
 
struct_type
  : STRUCT < col_name : data_type [COMMENT col_comment], ...>
 
union_type
   : UNIONTYPE < data_type, data_type, ... >

row_format
  : DELIMITED [FIELDS TERMINATED BY char [ESCAPED BY char]] [COLLECTION ITEMS TERMINATED BY char]
        [MAP KEYS TERMINATED BY char] [LINES TERMINATED BY char]
        [NULL DEFINED AS char]  
  | SERDE serde_name [WITH SERDEPROPERTIES (property_name=property_value, property_name=property_value, ...)]
  
 # :DELIMITED .... : 配合 stored as textfile 使用
 # serde 配合 stored as orc,sequencefile,parquet,avro 
  
 file_format:
  : SEQUENCEFILE
  | TEXTFILE   
  | RCFILE      
  | ORC        
  | PARQUET  
  | AVRO       
  | JSONFILE    -- (Note: Available in Hive 4.0.0 and later)
  | INPUTFORMAT input_format_classname OUTPUTFORMAT output_format_classname

default_value:
  : [ LITERAL|CURRENT_USER()|CURRENT_DATE()|CURRENT_TIMESTAMP()|NULL ] 
```

##### Row Format
`row format` 配合 stored as 使用的，每种格式都有固定的建表规范，详见

> 官方文档
> [StorageFormats]([https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-StorageFormatsStorageFormatsRowFormat,StorageFormat,andSerDe]())
>
> [RowFormats&SerDe](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-RowFormats&SerDe)
> 总结的
> [归纳的](https://blog.csdn.net/mhtian2015/article/details/78873815)

**STORED AS**

`STORED AS` 语法将{SerDe, InputFormat, and OutputFormat}三者绑定到STORED AS file_format语法从而更加便捷，也可以不包含SerDe，这样对于某些file_format，用户可以指定使用那个serde，从而更加多样化。

用户在建表的时候可以自定义SerDe或者使用自带的SerDe。如果没有指定ROW FORMAT 或者ROW FORMAT DELIMITED，将会使用自带的

SerDe。在建表的时候，用户还需要为表指定列，用户在指定表的列的同时也会指定自定义的SerDe，Hive通过SerDe确定表的具体的列的数据。

SerDe是Serialize/Deserilize的简称， hive使用Serde进行行对象的序列与反序列化


InputFormat、OutputFormat与SerDe三者间的关系 :

>SerDe is a short name for “Serializer and Deserializer.”</br>
> Hive uses SerDe (and !FileFormat) to read and write table rows.</br>
> HDFS files –> InputFileFormat –> <key, value> –> Deserializer –> Row object</br>
> Row object –> Serializer –> <key, value> –> OutputFileFormat –> HDFS files</br>
> Note that the "key" part is ignored when reading, and is always a constant when writing. Basically row object is stored into the "value".</br>



常见的建表例子：

- 创建一张存储格式是 `TEXTFILE` 

  > textfile 一般用于在已有数据文件上建表

```sql
  create external table external test_db.student
  (
  	  id int comment '学号',
      name string comment '名字'
  )
  partitioned by (date string comment '分区字段')
  row format delimited
      FIELDS TERMINATED BY '\t'
      LINES TERMINATED BY '\n'
      NULL TERMINATED BY '\N'
  stored as TEXTFILE
  LOCATION 'hdfs://user/hive_db/test_db/student'
```

- 创建一张存储格式为 `ORC`，压缩格式为 `snappy` 的表

```sql
create external table external test_db.student
(
	id int comment '学号',
    name string comment '名字'
)
partitioned by (date string comment '分区字段')
stored as orc
LOCATION 'hdfs://user/hive_db/test_db/student'
tblproperties ("orc.compress"="SNAPPY")

```

```sql
create external table external test_db.student
(
	id int comment '学号',
    name string comment '名字'
)
partitioned by (date string comment '分区字段')
ROW FORMAT SERDE 'org.apache.hadoop.hive.ql.io.orc.OrcSerde'-- 配合stored as inputformat.. 格式,不能少
WITH SERDEPROPERTIES (
  'serialization.format' = '1'
) -- with 看需求
STORED AS
  INPUTFORMAT 'org.apache.hadoop.hive.ql.io.orc.OrcInputFormat'
  OUTPUTFORMAT 'org.apache.hadoop.hive.ql.io.orc.OrcOutputFormat'
TBLPROPERTIES (
  'orc.compress' = 'SNAPPY' -- 指定压缩格式
)
```

-  CTAS 
	Create Table As Select ，根据查询结果创建表，不支持分区表，外部表，分桶表。创建这个表是原子的

```sql

CREATE TABLE test_db.student
	row format delimited
	      FIELDS TERMINATED BY '\t'
	      LINES TERMINATED BY '\n'
	      NULL TERMINATED BY '\N'
	stored as TEXTFILE
as
select id ,name from student1
```

-  Create Table Like

	指定是否为外部表和 `TBLPROPERTIES`（一般是压缩格式）
```sql
CREATE [external] TABLE empty_key_value_store

LIKE key_value_store [TBLPROPERTIES (property_name=property_value, ...)];
```



- [external]
  - 内部表：默认存储在  `hive.metastore.warehouse.dir ` 中，删除元数据等同于删除表数据
  - 外部表：drop 表，不会删除数据.

#### 2. 表元数据查询

###### 查询表名称

```sql
# 查询数据库下表名称
use dbname;
show tables;

SHOW TABLES [IN database_name] ['identifier_with_wildcards'];
```

###### 表的统计信息

```sql
# 显示表，分区扩展：输出包括基本表信息和文件系统信息

SHOW TABLE EXTENDED [IN|FROM database_name] LIKE 'identifier_with_wildcards' [PARTITION(partition_spec)];  -- 这个是官网上的，实际使用如下

# 带通配符，查看多个表的
SHOW TABLE EXTENDED [IN|FROM database_name] LIKE 'identifier_with_wildcards' 

# 对特定表，可以带上分区 （错误
-- SHOW TABLE EXTENDED [IN|FROM database_name] LIKE 'table_name' [PARTITION(partition_spec)]; 
```

###### 查询建表语句

```sql
# 查询 create table 
SHOW CREATE TABLE ([db_name.]table_name|view_name);
```

###### 查询表结构

```sql
# 查询表结构 describe 可以简写为 desc 
DESCRIBE [EXTENDED|FORMATTED] 
  [db_name.]table_name[ col_name ( [.field_name] | [.'$elem$'] | [.'$key$'] | [.'$value$'] )* ];

# 注:  该语句显示给定表包含分区列在内的全部列，假设使用了extendedkeyword，则以Thrift序列化形式显示表的元数据，假设使用formattedkeyword，则以表格形式显示元数据。

# 注：假设表拥有复合类型的列，能够通过使用表名.复合列名（'$elem$'用于数组，'$key$'用于map的键，'$value$'用于map的键值）查看该列的属性。

# 查询所有字段
DESCRIBE [EXTENDED|FORMATTED] [db_name.]table_name

# 查询单个字段
DESCRIBE [EXTENDED|FORMATTED] [db_name.]table_name colname

# 查询单个复杂类型的单个字段
DESCRIBE [EXTENDED|FORMATTED] [db_name.]table_name colname ( [.field_name] | [.'$elem$'] | [.'$key$'] | [.'$value$'] )* 
```

##### 列

```sql
# 显示列
SHOW COLUMNS (FROM|IN) table_name [(FROM|IN) db_name];

# 查询列
DESCRIBE FORMATTED [db_name.]table_name
DESCRIBE FORMATTED [db_name.]table_name column_name; -- extended 展示更少的
DESCRIBE FORMATTED [db_name.]table_name column_name PARTITION (partition_spec); -- extended 展示更少的
```

###### 分区

```sql
# 查询分区
SHOW PARTITIONS table_name;
SHOW PARTITIONS [db_name.]table_name [PARTITION(partition_spec)]; -- 可以分级展示
SHOW PARTITIONS [db_name.]table_name [PARTITION(partition_spec)] [WHERE where_condition] [ORDER BY col_list] [LIMIT rows]; -- hive 4.0
```

###### 表属性

```sql
# 显示表属性
SHOW TBLPROPERTIES [db_name.]tblname; -- 不能用[db_name].table_name
SHOW TBLPROPERTIES [db_name.]tblname("foo");
```

###### 函数

```sql
# 系统自带的函数
SHOW FUNCTIONS [LIKE "<pattern>"];

# 用法
desc function [extended] function_name;
```



###### 配置

```sql

# 显示配置的描述信息
SHOW CONF <configuration_name>;

```



#### 3. 修改

```sql
# 改名：注意版本，可能移动hdfs目录
ALTER TABLE table_name RENAME TO new_table_name;

# 属性
ALTER TABLE table_name SET TBLPROPERTIES table_properties;

# Add SerDe Properties
ALTER TABLE table_name [PARTITION partition_spec] SET SERDE serde_class_name [WITH SERDEPROPERTIES serde_properties];

ALTER TABLE table_name [PARTITION partition_spec] SET SERDEPROPERTIES serde_properties;

ALTER TABLE table_name SET SERDEPROPERTIES ('field.delim' = ',');

# Remove SerDe Properties
ALTER TABLE table_name [PARTITION partition_spec] UNSET SERDEPROPERTIES (property_name, ... );

# Alter Partition
ALTER TABLE table_name ADD [IF NOT EXISTS] PARTITION partition_spec [LOCATION 'location'][, PARTITION partition_spec [LOCATION 'location'], ...];

# rename to
ALTER TABLE table_name PARTITION partition_spec RENAME TO PARTITION partition_spec;

#修复分区 ： 分区目录必须满足 partition_spec
MSCK [REPAIR] TABLE table_name [ADD/DROP/SYNC PARTITIONS];

# 删除分区
ALTER TABLE table_name DROP [IF EXISTS] PARTITION partition_spec[, PARTITION partition_spec, ...]


# 更改表或分区的文件格式
ALTER TABLE table_name [PARTITION partition_spec] SET FILEFORMAT file_format;

# 更改表/分区位置
ALTER TABLE table_name [PARTITION partition_spec] SET LOCATION "new location";

# 更改列名称/类型/位置/注释
ALTER TABLE table_name [PARTITION partition_spec] CHANGE [COLUMN] col_old_name col_new_name column_type
  [COMMENT col_comment] [FIRST|AFTER column_name] [CASCADE|RESTRICT];
# 支持动态分区的更改
```


内部表转外部表
```sql
# TRUE 和 FALSE 区分大小写
alter table tableName set tblproperties('EXTERNAL'='TRUE');

alter table tableName set tblproperties('EXTERNAL'='TRUE');
```

### DML  ： `load ` `insert` `update` `delete`

##### load

```sql
LOAD DATA [LOCAL] INPATH 'filepath' [OVERWRITE] INTO TABLE tablename [PARTITION (partcol1=val1, partcol2=val2 ...)]
```



##### insert as select 

```sql
Standard syntax:

INSERT OVERWRITE TABLE tablename1 [PARTITION (partcol1=val1, partcol2=val2 ...) [IF NOT EXISTS]] select_statement1 FROM from_statement;

INSERT INTO TABLE tablename1 [PARTITION (partcol1=val1, partcol2=val2 ...)] select_statement1 FROM from_statement;

```

###### 对于分区表

```sql
# 指定分区
INSERT OVERWRITE TABLE tablename1 PARTITION (partcol1=val1, partcol2=val2 ...) [IF NOT EXISTS] select_statement1 FROM from_statement;

INSERT into TABLE tablename1 PARTITION (partcol1,partcol2) [IF NOT EXISTS] 
select_statement1 FROM from_statement;

# 动态分区
INSERT OVERWRITE TABLE tablename1 PARTITION (partcol1,partcol2) [IF NOT EXISTS] 
select_statement1 FROM from_statement;

INSERT into TABLE tablename1 PARTITION (partcol1,partcol2) [IF NOT EXISTS] 
select_statement1 FROM from_statement;

# 动态分区配置是否开， 产生文件限制（小文件） ，动态分区的最大数量
```



##### 通过查询将数据写入文件系统

```sql
INSERT OVERWRITE [LOCAL] DIRECTORY directory1
  [ROW FORMAT row_format] [STORED AS file_format]
  SELECT ... FROM ...
  
  
row_format
  : DELIMITED [FIELDS TERMINATED BY char [ESCAPED BY char]] [COLLECTION ITEMS TERMINATED BY char]
        [MAP KEYS TERMINATED BY char] [LINES TERMINATED BY char]
        [NULL DEFINED AS char] 
```



##### insert .... values

```sql
Standard Syntax:
INSERT INTO TABLE tablename [PARTITION (partcol1[=val1], partcol2[=val2] ...)] VALUES values_row [, values_row ...]
  
Where values_row is:
( value [, value ...] )
where a value is either null or any valid SQL literal
```

> Hive 不支持复杂类型（数组、映射、结构、联合）的文字，因此无法在 INSERT INTO...VALUES 子句中使用它们。这意味着用户不能使用 INSERT INTO...VALUES 子句将数据插入到复杂数据类型的列中。

































































```
create table ods_omg_2_ris_db_dxm.omg_2_ris_event_dd_test_test  like ods_omg_2_ris_db_dxm.omg_2_ris_event_dd_test

alter table wangxinxin.test20220426 replace columns(name string);
```

























t_deliver_qw_task_info :最近一次下发企微添加任务信息(30分钟级

t_current_qw_relation :企业微信用户目前的好友关系





















