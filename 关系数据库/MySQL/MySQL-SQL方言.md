官方文档

[MySQL :: MySQL 8.0 Reference Manual :: 13 SQL Statements](https://dev.mysql.com/doc/refman/8.0/en/sql-statements.html)

## 1. select 

[ SELECT Statement](https://dev.mysql.com/doc/refman/8.0/en/select.html)

查询

> 这部分的文档还没看

```mysql
SELECT
    [ALL | DISTINCT | DISTINCTROW ]
    select_expr [, select_expr] ...
    [FROM table_references]
    [WHERE where_condition]
    [GROUP BY {col_name | expr | position}, ... [WITH ROLLUP]]
    [HAVING where_condition]
    [WINDOW window_name AS (window_spec)
        [, window_name AS (window_spec)] ...]
    [ORDER BY {col_name | expr | position}
      [ASC | DESC], ... [WITH ROLLUP]]
    [LIMIT {[offset,] row_count | row_count OFFSET offset}]
```

查询赋予变量

```mysql
SELECT
    [ALL | DISTINCT | DISTINCTROW ]
    select_expr [, select_expr] ...
    [into_option]
    [FROM table_references]
into_option: {INTO var_name [, var_name] ...}
```



最简单的 select

```mysql
select 1
```

借助这个语法，可以简单测试一些函数，运算。

#### 1. 去重规则

**[ALL | DISTINCT | DISTINCTROW ]**

只能写在所有列最前面，对后面的所有列组成的元组去重。

NULL参与运算

#### 2. 过滤

**where where_condition**

行级别的过滤

#### 3. 排序

- **ASC（ascend）: 升序**
- **DESC（descend）:降序**

不写排序规则，默认是升序

支持列名，别名，列的位置引用。这是因为 `order by` 在 `select` 之后执行。

如果有 group by 子句，order by 后的字段必须来自于 group by 的字段或者聚合函数。

order by 支持 select 没有的字段（ 这是MySQL的扩展

#### 4. 分页

```mysql
 LIMIT [位置偏移量,] 行数
```

位置偏移量就是前面有多少行，对于第一行就是  `limit 0,1` 或 `limit 1`

注： 位置偏移量MySQL唯一一个从0序号

新的语法

```mysql
LIMIT 行数 OFFSET 位置偏移量
```

- 分页显式公式**：（当前页数-1）*每页条数，每页条数**

```mysql
SELECT * FROM table 
LIMIT(PageNo - 1)*PageSize,PageSize;
```



#### 5. 分组



#### 6. 组过滤

having 

#### 7. 运行顺序

```
FROM -> WHERE -> GROUP BY -> HAVING -> SELECT 的字段 -> DISTINCT -> ORDER BY -> LIMIT
```

## 2. 表达式

todo ：

[MySQL :: MySQL 8.0 Reference Manual :: 9.5 Expressions](https://dev.mysql.com/doc/refman/8.0/en/expressions.html)

## 2. 运算符

运算符和函数和类型绑定在一起的，所以按照以下掌握

1. 含义
2. 属于哪类（数值运算符，逻辑运算符
3. 特殊值运算（NULL）
4. 返回值
5. 优先级



- 算数运算符： + - * / % 

	[Arithmetic Operators](https://dev.mysql.com/doc/refman/8.0/en/arithmetic-functions.html)

- 比较运算符：`= <=> <> != < <= > >=`， `is null` `is not null` ,`in` `not in` `between and`（包含边界）

	[Comparison Functions and Operators](https://dev.mysql.com/doc/refman/8.0/en/comparison-operators.html#operator_greater-than-or-equal)

	适用于值，字符串，表达式，如果为为真返回 1 ， 为假返回0

- 逻辑运算符： `not` 或 `!` , `and` 或 `&&` , `or` 或 `||` ，`XOR`
	返回值 ： 真 1 假 0， 有NULL值返回NULL。
	[Logical Operators](https://dev.mysql.com/doc/refman/8.0/en/logical-operators.html)

- string运算符 `like` `regexp` `rlike` 

	[String Functions and Operators](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html)

	like 注意转义字符的问题

## 3. 关联查询

[[join]]

要点：

- MySQL没有 FULL JOIN ，可以用 LEFT JOIN **UNION** RIGHT join代替。



**SQL99 语法新特性**

- 自然连接

	自动查询两张表所有相同的字段，进行等值连接。是等值连接的语法糖。

	```mysql
	select tba.*,tba.* from tba natural join tbb
	```

- USING 连接

	指定连接需要的同名字段

	```mysql
	select tba.*,tba.* from tba natural join tbb using(same_col)
	```

	

**UNION [ALL]**

上下拼两张表



## 4. 子查询

#### 4.1 分类

1. 内查询的结果返回一条还是多条记录，将子查询分为`单行子查询`、`多行子查询`。单行和多行仅是谓词不同。
2. 按照内查询的位置来分，有 select子查询，where子查询，having子查询

3. 我们按内查询是否被执行多次，将子查询划分为`相关(或关联)子查询`和`不相关(或非关联)子查询`。

- 在关系代数中，子查询和关联完全等价，也就是说所有用**子查询的**都可以用**join**替代。



##### 1. 单行子查询

- 使用比较的查询

	[Comparisons Using Subqueries](https://dev.mysql.com/doc/refman/8.0/en/comparisons-using-subqueries.html)

- 注意比较可以是元组的比较。

	可以显式的使用元组构造。

	[Row Subqueries](https://dev.mysql.com/doc/refman/8.0/en/row-subqueries.html)

	行构造函数 `Row()`,只能用于`多列`比较。

##### 2. 多行子查询

- 也称为集合比较子查询
- 内查询返回多行
- 使用多行比较操作符（ IN， ANY， ALL， SOME， NOT IN）

[Subqueries with ANY, IN, or SOME](https://dev.mysql.com/doc/refman/8.0/en/any-in-some-subqueries.html)

##### 3. where 子查询

可以使用 单行/多行子查询 ， 相关/不相关子查询

##### 4. having 子查询

可以使用 单行/多行子查询 ， 相关/不相关子查询

##### 5. select 子查询

可以使用 单行/多行子查询 ， 相关/不相关子查询。必须保证子查询的执行结果是一行结果。

##### 6. case 子查询

case 上也可以使用 单行/多行子查询，相关/不相关子查询

##### 7. FROM子查询

- from 子查询完全可以看成一张二维表。
- 借助这个特性可以构造虚拟表

##### 8. 相关子查询

如果子查询的执行依赖于外部查询，通常情况下都是因为子查询中的表用到了外部的表，并进行了条件关联，因此每执行一次外部查询，子查询都要重新计算一次，这样的子查询就称之为`关联子查询`。

**执行**

相关子查询可以看成两个for 循环，外层循环A表的每一行，对A表的每一行循环一次B表。



#### 4.2 **空值问题**

子查询作为过滤条件会忽略空值



#### 4.3 语法糖

- **使用 EXISTS，NOT EXISTS 替代 in ， not in**
	- exists/not exists,经常用于相关子查询
	- 如何选用取决于两者的执行逻辑。

## 5. DDL 

#### [1. CREATE DATABASE Statement](https://dev.mysql.com/doc/refman/8.0/en/create-database.html)

```mysql
CREATE {DATABASE | SCHEMA} [IF NOT EXISTS] db_name
    [create_option] ...

create_option: [DEFAULT] {
    CHARACTER SET [=] charset_name
  | COLLATE [=] collation_name
}
```



#### [ 2. ALTER DATABASE Statement](https://dev.mysql.com/doc/refman/8.0/en/alter-database.html)

- 更改数据库字符集

```mysql
ALTER DATABASE 数据库名 CHARACTER SET 字符集;  #比如：gbk、utf8等
```

#### [3. DROP DATABASE Statement](https://dev.mysql.com/doc/refman/8.0/en/drop-database.html)

- 删除数据库

```mysql
DROP {DATABASE | SCHEMA} [IF EXISTS] db_name
```


#### [4. CREATE TABLE Statement](https://dev.mysql.com/doc/refman/8.0/en/create-table.html)

```mysql
CREATE [TEMPORARY] TABLE [IF NOT EXISTS] tbl_name
    (create_definition,...)
    [table_options]
    [partition_options]

CREATE [TEMPORARY] TABLE [IF NOT EXISTS] tbl_name
    [(create_definition,...)]
    [table_options]
    [partition_options]
    [IGNORE | REPLACE]
    [AS] query_expression

CREATE [TEMPORARY] TABLE [IF NOT EXISTS] tbl_name
    { LIKE old_tbl_name | (LIKE old_tbl_name) }

create_definition: {
    col_name column_definition
  | {INDEX | KEY} [index_name] [index_type] (key_part,...)
      [index_option] ...
  | {FULLTEXT | SPATIAL} [INDEX | KEY] [index_name] (key_part,...)
      [index_option] ...
  | [CONSTRAINT [symbol]] PRIMARY KEY
      [index_type] (key_part,...)
      [index_option] ...
  | [CONSTRAINT [symbol]] UNIQUE [INDEX | KEY]
      [index_name] [index_type] (key_part,...)
      [index_option] ...
  | [CONSTRAINT [symbol]] FOREIGN KEY
      [index_name] (col_name,...)
      reference_definition
  | check_constraint_definition
}

column_definition: {
    data_type [NOT NULL | NULL] [DEFAULT {literal | (expr)} ]
      [VISIBLE | INVISIBLE]
      [AUTO_INCREMENT] [UNIQUE [KEY]] [[PRIMARY] KEY]
      [COMMENT 'string']
      [COLLATE collation_name]
      [COLUMN_FORMAT {FIXED | DYNAMIC | DEFAULT}]
      [ENGINE_ATTRIBUTE [=] 'string']
      [SECONDARY_ENGINE_ATTRIBUTE [=] 'string']
      [STORAGE {DISK | MEMORY}]
      [reference_definition]
      [check_constraint_definition]
  | data_type
      [COLLATE collation_name]
      [GENERATED ALWAYS] AS (expr)
      [VIRTUAL | STORED] [NOT NULL | NULL]
      [VISIBLE | INVISIBLE]
      [UNIQUE [KEY]] [[PRIMARY] KEY]
      [COMMENT 'string']
      [reference_definition]
      [check_constraint_definition]
}

data_type:
    (see Chapter 11, Data Types)

key_part: {col_name [(length)] | (expr)} [ASC | DESC]

index_type:
    USING {BTREE | HASH}

index_option: {
    KEY_BLOCK_SIZE [=] value
  | index_type
  | WITH PARSER parser_name
  | COMMENT 'string'
  | {VISIBLE | INVISIBLE}
  |ENGINE_ATTRIBUTE [=] 'string'
  |SECONDARY_ENGINE_ATTRIBUTE [=] 'string'
}

check_constraint_definition:
    [CONSTRAINT [symbol]] CHECK (expr) [[NOT] ENFORCED]

reference_definition:
    REFERENCES tbl_name (key_part,...)
      [MATCH FULL | MATCH PARTIAL | MATCH SIMPLE]
      [ON DELETE reference_option]
      [ON UPDATE reference_option]

reference_option:
    RESTRICT | CASCADE | SET NULL | NO ACTION | SET DEFAULT

table_options:
    table_option [[,] table_option] ...

table_option: {
    AUTOEXTEND_SIZE [=] value
  | AUTO_INCREMENT [=] value
  | AVG_ROW_LENGTH [=] value
  | [DEFAULT] CHARACTER SET [=] charset_name
  | CHECKSUM [=] {0 | 1}
  | [DEFAULT] COLLATE [=] collation_name
  | COMMENT [=] 'string'
  | COMPRESSION [=] {'ZLIB' | 'LZ4' | 'NONE'}
  | CONNECTION [=] 'connect_string'
  | {DATA | INDEX} DIRECTORY [=] 'absolute path to directory'
  | DELAY_KEY_WRITE [=] {0 | 1}
  | ENCRYPTION [=] {'Y' | 'N'}
  | ENGINE [=] engine_name
  | ENGINE_ATTRIBUTE [=] 'string'
  | INSERT_METHOD [=] { NO | FIRST | LAST }
  | KEY_BLOCK_SIZE [=] value
  | MAX_ROWS [=] value
  | MIN_ROWS [=] value
  | PACK_KEYS [=] {0 | 1 | DEFAULT}
  | PASSWORD [=] 'string'
  | ROW_FORMAT [=] {DEFAULT | DYNAMIC | FIXED | COMPRESSED | REDUNDANT | COMPACT}
  | START TRANSACTION 
  | SECONDARY_ENGINE_ATTRIBUTE [=] 'string'
  | STATS_AUTO_RECALC [=] {DEFAULT | 0 | 1}
  | STATS_PERSISTENT [=] {DEFAULT | 0 | 1}
  | STATS_SAMPLE_PAGES [=] value
  | tablespace_option
  | UNION [=] (tbl_name[,tbl_name]...)
}

partition_options:
    PARTITION BY
        { [LINEAR] HASH(expr)
        | [LINEAR] KEY [ALGORITHM={1 | 2}] (column_list)
        | RANGE{(expr) | COLUMNS(column_list)}
        | LIST{(expr) | COLUMNS(column_list)} }
    [PARTITIONS num]
    [SUBPARTITION BY
        { [LINEAR] HASH(expr)
        | [LINEAR] KEY [ALGORITHM={1 | 2}] (column_list) }
      [SUBPARTITIONS num]
    ]
    [(partition_definition [, partition_definition] ...)]

partition_definition:
    PARTITION partition_name
        [VALUES
            {LESS THAN {(expr | value_list) | MAXVALUE}
            |
            IN (value_list)}]
        [[STORAGE] ENGINE [=] engine_name]
        [COMMENT [=] 'string' ]
        [DATA DIRECTORY [=] 'data_dir']
        [INDEX DIRECTORY [=] 'index_dir']
        [MAX_ROWS [=] max_number_of_rows]
        [MIN_ROWS [=] min_number_of_rows]
        [TABLESPACE [=] tablespace_name]
        [(subpartition_definition [, subpartition_definition] ...)]

subpartition_definition:
    SUBPARTITION logical_name
        [[STORAGE] ENGINE [=] engine_name]
        [COMMENT [=] 'string' ]
        [DATA DIRECTORY [=] 'data_dir']
        [INDEX DIRECTORY [=] 'index_dir']
        [MAX_ROWS [=] max_number_of_rows]
        [MIN_ROWS [=] min_number_of_rows]
        [TABLESPACE [=] tablespace_name]

tablespace_option:
    TABLESPACE tablespace_name [STORAGE DISK]
  | [TABLESPACE tablespace_name] STORAGE MEMORY

query_expression:
    SELECT ...   (Some valid select or union statement)
```



###### 1.直接建表

```
CREATE TABLE [IF NOT EXISTS] 表名(
	字段1, 数据类型 [约束条件] [默认值],
	字段2, 数据类型 [约束条件] [默认值],
	字段3, 数据类型 [约束条件] [默认值],
	……
	[表约束条件]
);
```

###### 2.根据查询条件建表

```mysql
CREATE TABLE [IF NOT EXISTS] tbl_name
    [(字段1, 数据类型 [约束条件] [默认值],...)]  -- 这些字段表示额外的字段，创建出来都为空。和query_expression无关。
    [AS] query_expression
```

###### 3. like表

```mysql
CREATE TABLE [IF NOT EXISTS] tbl_name
    { LIKE old_tbl_name | (LIKE old_tbl_name) }
```

只创建表，不复制数据。

#### [5.ALTER TABLE Statement](https://dev.mysql.com/doc/refman/8.0/en/alter-table.html)

##### 修改列

**使用 ALTER TABLE 语句可以实现：**

- 向已有的表中添加列
- 修改现有表中的列
- 删除现有表中的列
- 重命名现有表中的列

注意，DML没有事务支持

```mysql
ALTER TABLE tbl_name  option col_name column_definition [FIRST | AFTER col_name]
column_definition: {
    data_type [NOT NULL | NULL] [DEFAULT {literal | (expr)} ]
      [AUTO_INCREMENT] [UNIQUE [KEY]] [[PRIMARY] KEY]   -- AUTO_INCREMENT 必须定义在key上
      [COMMENT 'string']
}

option: {ADD |MODIFY |CHANGE|DROP}
column_definition: {
    data_type [NOT NULL | NULL] [DEFAULT {literal | (expr)} ]
      [AUTO_INCREMENT] [UNIQUE [KEY]] [[PRIMARY] KEY]
      [COMMENT 'string']
}
```

添加列

```mysql
ALTER TABLE tbl_name ADD [COLUMN] 字段名 字段类型 约束条件 【FIRST|AFTER 字段名】;
```

修改列

```mysql
ALTER TABLE 表名 MODIFY 【COLUMN】 字段名1 字段类型 【DEFAULT 默认值】【FIRST|AFTER 字段名2】;
```

重命名列

```mysql
ALTER TABLE 表名 CHANGE 【column】 列名 新列名 新数据类型;
```

删除列

```mysql
ALTER TABLE 表名 DROP 【COLUMN】字段名
```



重命名表

```mysql
RENAME TABLE emp TO myemp;

ALTER table dept RENAME [TO] detail_dept;  -- [TO]可以省略
```



清空表

```mysql
TRUNCATE TABLE detail_dept;
```

TRUNCATE语句**不能回滚**，而使用 DELETE 语句删除数据，可以回滚



##### 修改索引



##### 修改约束





#### 6.DDL支持事务

[Atomic Data Definition Statement Support](https://dev.mysql.com/doc/refman/8.0/en/atomic-ddl.html#atomic-ddl-supported-statements)

InnoDB支持表的DDL事务



## 6. DML

#### 1. [ INSERT Statement](https://dev.mysql.com/doc/refman/8.0/en/insert.html)

值的方式添加

单条省略列，按建表顺序对应

```mysql
INSERT INTO table_name
VALUES (value1,value2,....);
```

指定顺序

```mysql
INSERT INTO table_name(column1 [, column2, …, columnn]) 
VALUES (value1,value2,....);
```

多行插入

```mysql
INSERT INTO table_name 
VALUES 
(value1 [,value2, …, valuen]),
(value1 [,value2, …, valuen]),
……
(value1 [,value2, …, valuen]);

#或者

INSERT INTO table_name(column1 [, column2, …, columnn]) 
VALUES 
(value1 [,value2, …, valuen]),
(value1 [,value2, …, valuen]),
……
(value1 [,value2, …, valuen]);
```

使用INSERT同时插入多条记录时，MySQL会返回一些在执行单行插入时没有的额外信息，这些信息的含义如下：
●　Records：表明插入的记录条数。
●　Duplicates：表明插入时被忽略的记录，原因可能是这些记录包含了重复的主键值。
●　Warnings：表明有问题的数据值，例如发生数据类型转换。

- `VALUES`也可以写成`VALUE`，但是VALUES是标准写法。


- 字符和日期型数据应包含在单引号中。（这是字面量问题）



**insert into as query**

基本语法格式如下：

```mysql
INSERT INTO 目标表名
(tar_column1 [, tar_column2, …, tar_columnn])
SELECT
(src_column1 [, src_column2, …, src_columnn])
FROM 源表名
[WHERE condition]
```

- 在 INSERT 语句中加入子查询。 
- **不必书写** **VALUES** **子句。** 
- 子查询中的值列表应与 INSERT 子句中的列名对应。



#### 2. [UPDATE Statement](https://dev.mysql.com/doc/refman/8.0/en/update.html)

```
UPDATE table_reference
    SET [col_name = value,..]
    [WHERE where_condition]
    [ORDER BY ...]
    [LIMIT row_count]

value:
    {expr | DEFAULT}
```

如果省略 WHERE 子句，则表中的所有数据都将被更新。

order by 按照顺序更新

limit 表示更新条数，只适用于单表，不适用于多表。



#### 3.[DELETE Statement](https://dev.mysql.com/doc/refman/8.0/en/delete.html)

```
DELETE FROM tbl_name [[AS] tbl_alias]
    [WHERE where_condition]
    [ORDER BY ...]
    [LIMIT row_count]
```

如果没有WHERE子句，DELETE语句将删除表中的所有记录。

order by 按照顺序删除

limit 表示更新条数，只适用于单表，不适用于多表。
