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

#### 2. 修改

```sql
# 低版本hive只能修改DBPROPERTIES
ALTER (DATABASE|SCHEMA) database_name SET DBPROPERTIES (property_name=property_value, ...); 
```

注： hive高版本有修改 location 的选项，但是这只会影响新数据存放位置，不会影响历史数据数据位置。

#### 3. 删除

```sql
DROP (DATABASE|SCHEMA) [IF EXISTS] database_name [RESTRICT|CASCADE];
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

SHOW TABLE EXTENDED [IN|FROM database_name] LIKE 'identifier_with_wildcards' [PARTITION(partition_spec)];  -- 这个是官网上的，实际使用如下

# 带通配符，查看多个表的
SHOW TABLE EXTENDED [IN|FROM database_name] LIKE 'identifier_with_wildcards' 

# 对特定表，可以带上分区 （错误
-- SHOW TABLE EXTENDED [IN|FROM database_name] LIKE 'table_name' [PARTITION(partition_spec)]; 
```



###### 3. 查询建表语句

```sql
# 查询 create table 
SHOW CREATE TABLE ([db_name.]table_name|view_name);
```



###### 4. 查询表结构

```sql
# 查询表结构 describe 可以简写为 desc 
DESCRIBE [EXTENDED|FORMATTED] 
  [db_name.]table_name[ col_name ( [.field_name] | [.'$elem$'] | [.'$key$'] | [.'$value$'] )* ];

# 注:  该语句显示给定表包含分区列在内的全部列，假设使用了extended keyword，则以Thrift序列化形式显示表的元数据，假设使用formatted keyword，则以表格形式显示元数据。

# 注：假设表拥有复合类型的列，能够通过使用表名.复合列名（'$elem$'用于数组，'$key$'用于map的键，'$value$'用于map的键值）查看该列的属性。

# 查询所有字段
DESCRIBE [EXTENDED|FORMATTED] [db_name.]table_name

# 查询单个字段
DESCRIBE [EXTENDED|FORMATTED] [db_name.]table_name colname

# 查询单个复杂类型的单个字段
DESCRIBE [EXTENDED|FORMATTED] [db_name.]table_name colname ( [.field_name] | [.'$elem$'] | [.'$key$'] | [.'$value$'] )* 
```

###### 5. 表属性

```sql
# 显示表属性
SHOW TBLPROPERTIES [db_name.]tblname; -- 不能用[db_name].table_name
SHOW TBLPROPERTIES [db_name.]tblname("foo");
```



##### 列元数据

###### 列

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

