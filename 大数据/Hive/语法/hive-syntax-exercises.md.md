### database

##### 1.

编写一条 SQL 语句来创建一个名为 `test_hive` 的 Hive 数据库，该数据库的注释为 'test_hive'，数据存储路径为 '/user/hive/warehouse/test_hive.db'。

```hive
CREATE DATABASE IF NOT EXISTS test_hive
COMMENT 'test_hive'
LOCATION '/user/hive/warehouse/test_hive.db';
```



编写一条 SQL 语句来创建一个以 "test_" 前缀命名的新 Hive 数据库，要求数据库名称是随机生成的，注释为 'test_database'，数据存储路径为 '/user/hive/warehouse/'。



```sql
CREATE DATABASE IF NOT EXISTS test_database
COMMENT 'test_database'
LOCATION '/user/hive/warehouse/test_database.db';
```



##### 2. 

**题目1：**

编写一条 SQL 查询语句，用于显示当前 Hive 实例中的所有数据库。

**答案1：**

```sql
SHOW DATABASES;
```

**题目2：**

编写一条 SQL 查询语句，查找所有 前缀是 ”test“ 的数据库。

**答案2：**

```sql
SHOW DATABASES LIKE 'test*';
```

**题目3：**

编写一条 SQL 查询语句，用于展示  `test_hive` 的信息(`desc`).

**答案3：**

```sql
desc database test_hive;
```

**题目4：**

编写两条 SQL 查询语句，将当前会话的默认数据库设置为名为 `test_hive` 的数据库，然后检查之。

**答案4：**

```sql
use test_hive;
SELECT current_database();

```



##### 3

**题目：**

编写一条 SQL 语句，将名为 `test_hive` 的数据库的属性 `create_time` 设置为 '2020-01-01'。

**答案：**

```sql
ALTER DATABASE test_hive SET DBPROPERTIES ('create_time'='2020-01-01');
```



##### 4.  最后

**题目：**

编写 SQL 语句，分别删除名为 `test_hive` 和 `test_database` 的两个数据库，并通过展示所有数据库的方式验证其已被成功删除。

**答案：**

```hive
-- 删除 test_hive 数据库
DROP DATABASE IF EXISTS test_hive;

-- 删除 test_database 数据库
DROP DATABASE IF EXISTS test_database;

-- 验证数据库已被删除
SHOW DATABASES;
```



### table

##### 1. 

**题目：**

根据以下要求，编写一条 SQL 语句，创建一个外部表 `test_hive.partitioned_table`：

| 字段名 | 数据类型 | 说明 |
| ------ | -------- | ---- |
| id     | int      |      |
| name   | string   |      |



表结构要求，这个表使用文本存储就可。

|                |                                                              |      |
| -------------- | ------------------------------------------------------------ | ---- |
| 表注释：       | partitioned table                                            |      |
| 分区字段：     | dt (string)                                                  |      |
| 行格式：       | 字段以制表符 '\t' 分隔，行以换行符 '\n' 结束，空值表示为 '\N' |      |
| 存储格式：     | TEXTFILE                                                     |      |
| 数据存储路径： | /user/hive/warehouse/test_hive.db/partitioned_table          |      |



注意先查看 db的路径。

**答案：**

```sql
create external table test_hive.partitioned_table
(
    id   int,
    name string
) comment "partitioned table"
partitioned by (dt string)
row format delimited fields terminated by '\t' lines terminated by '\n' null defined as '\N'
stored as textfile
location '/user/hive/warehouse/test_hive.db/partitioned_table';
```



**题目：**

根据以下要求，编写一条 SQL 语句，创建一个外部表 `test_hive.json_table`，该表使用 JSON 格式存储数据，并且需要指定相应的 SerDe。

| 字段名 | 数据类型 | 说明 |
| ------ | -------- | ---- |
| id     | int      |      |
| name   | string   |      |
| age    | int      |      |

表注释：JSON table

数据存储路径：/user/hive/warehouse/test_hive.db/json_table

存储格式：TEXTFILE

Serde： SERDE 'org.apache.hive.hcatalog.data.JsonSerDe'

注意， json 必须手动指定 serde

**答案：**

```sql
CREATE EXTERNAL TABLE test_hive.json_table (
                                               id INT,
                                               name STRING,
                                               age INT
)
    COMMENT "JSON table"
    ROW FORMAT SERDE 'org.apache.hive.hcatalog.data.JsonSerDe'
    STORED AS TEXTFILE
    LOCATION '/user/hive/warehouse/test_hive.db/json_table' ;
```



**题目：**

根据以下要求，编写一条 SQL 语句，创建一个外部表 `test_hive.student`：

| 字段名 | 数据类型 | 说明 |
| ------ | -------- | ---- |
| id     | int      |      |
| name   | string   |      |

分区字段：date (string)

存储格式：ORC

压缩格式：SNAPPY

数据存储路径：hdfs://user/hive_db/test_hive/student

**答案：**

```sql
CREATE EXTERNAL TABLE test_hive.student (
                                            id INT,
                                            name STRING
)
    comment "student table"
    PARTITIONED BY (dt STRING)
    STORED AS ORC
    LOCATION '/user/hive/warehouse/test_hive.db/student'
    TBLPROPERTIES ("orc.compress"="SNAPPY");
```



##### 2.ctas



##### 3. show/desc

##### 4. alter table

