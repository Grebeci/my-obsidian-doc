## Load Data

Loading files into tables

[LanguageManual DML - Apache Hive - Apache Software Foundation](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DML#LanguageManualDML-Loadingfilesintotables)



Hive在将数据加载到表中时不进行任何转换。加载操作目前是纯粹的`copy/insert`操作，将数据文件移动到与Hive表对应的位置

##### 语法

```sql
LOAD DATA [LOCAL] INPATH 'filepath' [OVERWRITE] INTO TABLE tablename [PARTITION (partcol1=val1, partcol2=val2 ...)]

LOAD DATA [LOCAL] INPATH 'filepath' [OVERWRITE] INTO TABLE tablename [PARTITION (partcol1=val1, partcol2=val2 ...)] [INPUTFORMAT 'inputformat' SERDE 'serde']
```

  将本地文件/hdfs文件 copy/mv 到指定表中



##### 分区表

##### 加载每个分区的数据

表的元数据是按分区为单位存储的。让分区和数据关联的方式有三种

- dfs mkdir 数据路径，dfs put 数据后 msck repair 修复。
- dfs mkdir 数据路径，dfs put 数据后 add partition 映射数据。推荐使用这种
- dfs mkdir 数据路径，load data数据。



## Inserting 

#### Inserting data into Hive Tables from queries

语法

```sql
INSERT OVERWRITE/INTO TABLE tablename1 [PARTITION (partcol1=val1, partcol2=val2 ...) [IF NOT EXISTS]] select_statement1 FROM from_statement;

INSERT INTO TABLE tablename1 [PARTITION (partcol1=val1, partcol2=val2 ...)] 
select_statement1 FROM from_statement;
```

##### insert data into partition

- 如果是分区表，必须指定分区字段

- 动态分区

	[DynamicPartitions - Apache Hive - Apache Software Foundation](https://cwiki.apache.org/confluence/display/Hive/DynamicPartitions)

	```mysql
	1. 开配置
	hive.exec.dynamic.partition=true
	hive.exec.dynamic.partition.mode=nonstrict
	2. 语法
	Hive extension (dynamic partition inserts):
	INSERT OVERWRITE/INTO  TABLE tablename PARTITION (partcol1[=val1], partcol2[=val2] ...) [IF NOT EXISTS]] select_statement FROM from_statement;
	
	INSERT INTO TABLE tablename1 PARTITION (partcol1,partcol2 ..)
	select_statement1 FROM from_statement;
	```



#### Writing data into the filesystem from queries

```sql
Standard syntax:
INSERT OVERWRITE [LOCAL] DIRECTORY directory1
  [ROW FORMAT row_format] [STORED AS file_format] (Note: Only available starting with Hive 0.11.0)
  SELECT ... FROM ...
 
  
row_format
  : DELIMITED [FIELDS TERMINATED BY char [ESCAPED BY char]] [COLLECTION ITEMS TERMINATED BY char]
        [MAP KEYS TERMINATED BY char] [LINES TERMINATED BY char]
        [NULL DEFINED AS char] (Note: Only available starting with Hive 0.13)
```

