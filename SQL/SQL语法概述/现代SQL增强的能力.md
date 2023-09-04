1. with as 语法
2. values 语法
3. laterval 
4. 集合操作
5. 窗口函数

#### MySQL SQL语法

with as

[MySQL 8.0 WITH (Common Table Expressions)](https://dev.mysql.com/doc/refman/8.0/en/with.html#common-table-expressions-recursive)

[1270. 向公司CEO汇报工作的所有人 - 力扣（LeetCode）](https://leetcode.cn/problems/all-people-report-to-the-given-manager/comments/)



Values 

[MySQL 8.0  VALUES Statement](https://dev.mysql.com/doc/refman/8.0/en/values.html)



### 1. 集合操作

标准SQL定义了三种集合操作

并集： Union 

交集： intersect

差集： except (或 minux)

MySQL 的实现 ： [Set Operations with UNION, INTERSECT, and EXCEPT](https://dev.mysql.com/doc/refman/8.0/en/set-operations.html)



##### 并集

上下拼表

[MySQL  UNION Clause](https://dev.mysql.com/doc/refman/8.0/en/union.html)

##### 交集


标准SQL写法 `intersect` , MySQL8.0.31 之前没有这个操作，可以使用 内连接模拟

```
select A.* | B.*  from A join B on A.key = B.key 
```

##### 差集 

标准SQL写法 `except` , MySQL8.0.31 之前没有这个操作，可以使用 `外连接` 模拟

A-B

```
select A.* | B.*  from A left join B on A.key = B.key where B.col is null;
```



### 2. 递归

```sql
 WITH [RECURSIVE]
   cte_name [(col_name [, col_name] ...)] AS (
    -- Anchor member
    UNION ALL
    -- Recursive member
)
```

##### 运行原理：

首次构造出初始集，然后使用初始集迭代一次，后续每次迭代的集合使用上次迭代出的结果，直到最终迭代结束，将每次迭代的结果取并集。

##### 例子：

- 迭代出 1-20 的序列

	```mysql
	WITH RECURSIVE sequence (n) AS (
	    SELECT 1
	    UNION ALL
	    SELECT n + 1 FROM sequence WHERE n < 20
	)
	SELECT * FROM sequence;
	
	```

- 表 section  start end 代表一个整数集合的端点

	```
	id    start    end 
	1       1        10
	2       3        20
	3       2        5
	```

	将每个id 对应的区间每个整数集展开

	```
	id      num
	1        1
	1        2
	1        3
	...
	1        10
	2        3
	...
	```

	```mysql
	WITH RECURSIVE sequence (id, num) AS (
	    SELECT id, start FROM section
	    UNION ALL
	    SELECT id, num + 1 FROM sequence WHERE num < (SELECT end FROM section WHERE section.id = sequence.id)
	)
	SELECT * FROM sequence ORDER BY id, num;
	```

注意思考这些例子是怎样运行的。







