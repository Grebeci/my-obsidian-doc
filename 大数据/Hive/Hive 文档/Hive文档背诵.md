

-----

# GettingStarted](https://cwiki.apache.org/confluence/display/Hive/GettingStarted)







## DML（Hive）

### Inserting data into Hive Tables from queries

参考 ：           

[Inserting data into Hive Tables from queries](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DML#LanguageManualDML-InsertingdataintoHiveTablesfromqueries)



```sql
Standard syntax

INSERT OVERWRITE TABLE tablename1 [PARTITION (partcol1=val1, partcol2=val2 ...) [IF NOT EXISTS]] select_statement1 FROM from_statement;

INSERT INTO TABLE tablename1 [PARTITION (partcol1=val1, partcol2=val2 ...)] select_statement1 FROM from_statement;
 
Hive extension (dynamic partition inserts):
Hive 扩展（动态分区插入）：
INSERT OVERWRITE TABLE tablename PARTITION (partcol1[=val1], partcol2[=val2] ...) select_statement FROM from_statement;
INSERT OVERWRITE TABLE tablename PARTITION (partcol1[=val1], partcol2[=val2] ...) select_statement FROM from_statement；
INSERT INTO TABLE tablename PARTITION (partcol1[=val1], partcol2[=val2] ...) select_statement FROM from_statement;
INSERT INTO TABLE tablename PARTITION (partcol1[=val1], partcol2[=val2] ...) select_statement FROM from_statement；
```


- INSERT OVERWRITE 将**覆盖**表或分区中的任何现有数据。
- INSERT INTO 将**追加**到表或分区，保持现有数据不变。

- 可以对表或分区进行插入。如果表是分区的，则必须通过指定所有分区列的值来指定表的特定分区。


动态分区：

在动态分区插入中，用户可以给出部分分区规范，这意味着只需在 PARTITION 子句中指定分区列名列表。列值是可选的。如果给出了分区列值，我们称之为静态分区，否则就是动态分区。

每个动态分区列都有一个来自 select 语句的对应输入列。这意味着动态分区的创建由输入列的值决定。动态分区列必须在 SELECT 语句的列中最后指定，并且与它们在 PARTITION() 子句中出现的顺序相同。



Q: 可以指定字段名称吗？

N: 不支持，必须按照DDL字段顺序插入。hive 最常见的常见就是数仓中每天一个分区的 overwrite ， 没有 upsert 的情景。



hive 动态分区的配置：

`hive.exec.dynamic.partition = true`

`hive.exec.dynamic.partition.mode = nonstrict/strict`



### Loading files into tables

在将数据加载到表中时，Hive 不会进行任何转换。加载操作目前是纯粹的 copy/move 操作，将数据文件移动到与 Hive 表相对应的位置。

```sql
LOAD DATA [LOCAL] INPATH 'filepath' [OVERWRITE] INTO TABLE tablename [PARTITION (partcol1=val1, partcol2=val2 ...)]

```

- filepath ： 可以是相对路径，也可以是绝对路径，或者是URI hdfs://xxx

- 加载的目标可以是表或分区。如果表是分区的，则必须通过指定所有分区列的值来指定表的特定分区。
- filepath 可以指向一个文件（在这种情况下，Hive 会将该文件移入表中），也可以指向一个目录（在这种情况下，Hive 会将该目录中的所有文件移入表中）。无论哪种情况，filepath 都会指向一组文件。



### Writing data into the filesystem from queries

```
Standard syntax:

INSERT OVERWRITE [LOCAL] DIRECTORY directory1
  [ROW FORMAT row_format] [STORED AS file_format] (Note: Only available starting with Hive 0.11.0)
  SELECT ... FROM ...

```

- 写入文件系统的数据以文本形式序列化，列之间用 `^A` 分隔，行之间用换行符分隔。如果有任何列不是原始类型，那么这些列将被序列化为 JSON 格式。

Q: 有哪些方式向hive表（分区）写数据？

N：
- hive => hive : insert as select 
- add partition + load data
- add partition + insert directory
- import/export : 这个属于hive特有的语法。


Q : 有哪些添加分区的方式？
N: 
- add


### Inserting values into tables from SQL

将 VALUES 子句中列出的每一行插入到表 tablename 中 ： 

```
Standard Syntax:

INSERT INTO TABLE tablename [PARTITION (partcol1[=val1], partcol2[=val2] ...)] VALUES (col1,col2,...),(col1,col2,...) ...
```

这个语法是模拟标准SDL的 insert values。

- Hive 不支持复杂类型（array、map、struct、union）的字面意义，因此无法在 INSERT INTO...VALUES 子句中使用它们

### update/Delete/Meger

这些操作虽然提供，但是不会使用，大多hive表更新都是 insert into/overWrite

## Commands and CLI

commands

| command               | description                                                  |
| --------------------- | ------------------------------------------------------------ |
| quit <br />exit       | Use quit or exit to leave the interactive shell.             |
| `set <key> = <value>` | Sets the value of a particular configuration variable (key). Note: If you misspell the variable name, the CLI will not show an error. |
| set                   | Prints a list of configuration variables that are overridden by the user or Hive. |
| set -v                | Prints all Hadoop and Hive configuration variables.          |
| `! <command>`         | Executes a shell command from the Hive shell.                |



Beeline Commands

| Command              |                                                              |
| -------------------- | ------------------------------------------------------------ |
| `!<SQLLine command>` | 基于： http://sqlline.sourceforge.net/.<br /> `!quit` `!help` |



Beeline Command Options

| Command Options                                            | 描述                     |
| ---------------------------------------------------------- | ------------------------ |
| `-n <username> -p <password> -d <driver class>`            | 用户名，密码，驱动       |
| `-e <query>`  `-e <query>`                                 | 不交互方式查询           |
| `-f <file>`                                                | 应执行的SQL脚本文件      |
| `--hiveconf property=value`                                | hive配置属性             |
| `--showHeader`                                             | 查询结果是否有头         |
| --maxWidth=MAXWIDTH                                        | 列的最大宽度，单位是字符 |
| **--maxColumnWidth=**                                      | 每行的宽度，单位是字符   |
| **--outputformat=** [table/vertical/csv/tsv/dsv/csv2/tsv2] |                          |




- 注意，没有参数在交互式的方式限制每个查询结果行数，所以每个查询必须加上limit

- outputFormat 可以设置每一行都是 键值格式显示。有时候要用用beeline结果重定向到文件，也可以选择输出格式。



操作

hive2 之后 CRTL + C 会取消查询



## DataTypes



## Data Retrieval: Queries

#### Later view



#### Union 



#### Join



##### Join Optimization



#### Group by



### SubQueries



### virtual Columns



### windows function















