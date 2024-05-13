##### 1.

编写一条 SQL 语句来创建一个名为 `test_hive` 的 Hive 数据库，该数据库的注释为 'test_hive'，数据存储路径为 '/user/hive/warehouse/test_hive.db'。

```
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
DESC DATABASE db_name;
```

**题目4：**

编写一条 SQL 查询语句，用于检查当前所在的数据库。

**答案4：**

```sql
SELECT current_database();
```

这样的描述方式确保了每个题目的唯一性，使得答案能够准确反映题目要求。

