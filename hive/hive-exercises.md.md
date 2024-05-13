##### 1.

编写一条 SQL 语句来创建一个名为 `test_hive` 的 Hive 数据库，该数据库的注释为 'test_hive'，数据存储路径为 '/user/hive/warehouse/test_hive.db'。

```
CREATE DATABASE IF NOT EXISTS test_hive1
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



