# feature😅

#### 1. `LIKE 'identifier_with_wildcards' ` 是 正则模式，不是 `SQL模式`

正则表达式中的通配符只能用 "*"表示任何字符，或用"|"表示选择

`SQL 模式`：类似 `SQL` 类型的表达式，其中"%"表示任意字符，"_"表示单个字符。

#### 2. show 不要使用 `db.tb` 方式引用表

- `identifier_with_wildcards` ：使用带有正则字符形式的 `db.tb` 引表名不能匹配到任何东西，只能用 `[IN|FROM database_name]` 语句限制数据库。


>  Hive Doc  上以 `show`、`desc` 为目录介绍语法，实际使用侧重于查询某些数据库元素，故重新组织下，可作为文档参考。
> 
>  只列举了 **常用** 、**必记** 的语句，对于冷门和偏门的不做记录。

## Database

[LanguageManual DDL - Apache Hive - Apache Software Foundation ~ LanguageManual DDL - Apache Hive - Apache 软件基金会](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-Create/Drop/Alter/UseDatabase)

#### 1. 显示数据库


1. 显示全部数据库 
   
	```sql
	SHOW DATABASES;
	```
	
2. 模糊查询某些数据库 

	```sql
	SHOW (DATABASES|SCHEMAS) [LIKE 'identifier_with_wildcards'];
	identifier_with_wildcards ： 支持的模式受版本影响，具体看文档。 
	```

3.  检查当前是哪个数据库

	```sql
	SELECT current_database()
	```

## Table

#### 表元数据

###### 1.表存在

1. 查询数据库下表所有的表

	```sql
	use dbname;
	show tables;
	```

2. 模糊查询

	```sql
	SHOW TABLES [IN database_name] [like 'identifier_with_wildcards'];
	```

###### 2. 表的统计信息

分区，占用空间

```sql
# 显示表，分区扩展：输出包括基本表信息和文件系统信息

SHOW TABLE EXTENDED [IN|FROM database_name] LIKE 'identifier_with_wildcards' [PARTITION(partition_spec)]; 

-- 分区表
show table extended like part_table; # part_table 必须是精确表名

-- 非分区表
show table extended like "tb_name*"
```

- 注： 如果是 **非分区表**，`identifier_with_wildcards` 才可以用用正则模式，否则只能写表名称（字符串精确查找）。
- 注2：`identifier_with_wildcards` 只能写表名，不可以用 `db.tb_name` 指定 db 用 `[IN|FROM database_name]` 语句。


###### 3. 查询建表语句

```sql
# 查询 create table 
SHOW CREATE TABLE ([db_name.]table_name|view_name);
```

借助 show create table 格式化输出所有建表语句。特定库，特定表。

###### 4. 查询表结构

```sql
# 查询表结构 describe 可以简写为 desc 
DESCRIBE [EXTENDED|FORMATTED] 
  [db_name.]table_name[ col_name ( [.field_name] | [.'$elem$'] | [.'$key$'] | [.'$value$'] )* ];

# 注:  该语句显示给定表包含分区列在内的全部列，假设使用了extended keyword，则以Thrift序列化形式显示表的元数据，假设使用formatted keyword，则以表格形式显示元数据。 查询详细信息，一律加上 `formatted`。

# 注：假设表拥有复合类型的列，能够通过使用表名.复合列名（'$elem$'用于数组，'$key$'用于map的键，'$value$'用于map的键值）查看该列的属性。

# 查询所有字段
DESCRIBE [EXTENDED|FORMATTED] [db_name.]table_name

# 查询单个字段
DESCRIBE [EXTENDED|FORMATTED] [db_name.]table_name colname

# 查询单个复杂类型的单个字段（不用记忆）
DESCRIBE [EXTENDED|FORMATTED] [db_name.]table_name colname ( [.field_name] | [.'$elem$'] | [.'$key$'] | [.'$value$'] )* 
```

```
# 数据库 test_hive; 表 table1（id，string）

# 展示某个表的元数据信息。
desc formatted test_hive.table1;

# 具体到某个(只支持单个)字段。 formatted 大部分信息没用，只用简单就可。
desc  test_hive.table1 id

# 对于 complex column 类，慎用。

```

###### 5. 表属性

```sql
# 显示表属性
SHOW TBLPROPERTIES [db_name.]tblname; 
```

```sql
-- 展示 oorc.compress， EXTERNAL  表属性
show tblproperties test_hive.table1;
```

##### 列元数据

###### 列

 **show 语法**

```sql

# 显示列
SHOW COLUMNS (FROM|IN) table_name [(FROM|IN) db_name];

```


**desc 语法**

```
# 查询所有列
DESCRIBE FORMATTED [db_name.]table_name

# 查询特定列
DESCRIBE FORMATTED [db_name.]table_name column_name; 

# 有分区的，某个字段
DESCRIBE FORMATTED [db_name.]table_name  PARTITION (partition_spec) column_name; 
```


```sql
-- show 语法比较奇怪，只需记住下面一种,in 不能省略。
show columns in db_name.tbl_name;

-- desc 全部列，formatted 大部分
desc 
```

###### 分区

```sql
# 查询分区
SHOW PARTITIONS table_name;
SHOW PARTITIONS [db_name.]table_name [PARTITION(partition_spec)]; -- 可以分级展示
SHOW PARTITIONS [db_name.]table_name [PARTITION(partition_spec)] [WHERE where_condition] [ORDER BY col_list] [LIMIT rows]; -- hive 4.0
```



## 函数

```sql
# 展示函数
SHOW FUNCTIONS [LIKE "<pattern>"];

desc function [extended] function_name;
```



## 配置

```sql
# 显示配置的描述信息
SHOW CONF <configuration_name>;

```









### question

TSV 文件格式

- 数据本身有 \t 怎么整
- 写一个解析tsv 格式的解析驱动程序，java 、 scala

CSV
