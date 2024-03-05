#### SQL 的执行顺序

在 SQL 查询中，逻辑执行顺序（也就是 SQL 的解析顺序）大致如下：

`FROM` => `WHERE`  => `GROUP BY`  => `HAVING` => `SELECT `  => `ORDER BY` => `LIMIT` 



**1. Distinct 是先于 Limit 执行的**

 [176. 第二高的薪水](https://leetcode.cn/problems/second-highest-salary/)

要考虑多人薪资水平相同，而题目要求结果只返回一个值。

考虑以下SQL ：

```sql
SELECT DISTINCT salary
FROM Employee
ORDER BY salary DESC
LIMIT 1 OFFSET 1
```

`SELECT`（包括`DISTINCT`的处理）实际上是在`LIMIT`之前发生的。因此，首先执行`SELECT DISTINCT salary`去除重复的薪水值，然后对这个去重后的结果集应用`ORDER BY`和`LIMIT`。

我们可以逻辑上理解为：首先从`Employee`表中选择不重复的`salary`值，然后按照`salary`降序排列这些唯一的值，最后选择第二条记录。这就确保了我们能得到第二高的薪水值，而不是简单地选择所有结果中的第二条记录。



**2. 别名不能在同一级别的where 引用**

因为`WHERE`子句在逻辑上先于`SELECT`子句执行。所以此时别名还没有被定义。



**3. 分组查询中对非聚合列的引用限制** 

[SQL 仅选择列上具有最大值的行](https://stackoverflow.com/questions/7745609/sql-select-only-rows-with-max-value-on-a-column/7745635)

在使用`GROUP BY`进行数据分组时，为了保证查询结果的明确性和一致性，`SELECT`列表中只能包含经过聚合函数处理的列或直接出现在`GROUP BY`子句中的列。

这是因为每个分组可能包含多行数据，而未在`GROUP BY`中指定的非聚合列在分组内可能有多个不同的值，导致数据库无法确定应该展示哪个具体值。因此，尝试选择与`GROUP BY`无关的非聚合列会引入不确定性，这在SQL中是不被允许的。

这个取决于各个数据库的实现,

-  `MySQL` 的 `select` 选择 非聚合列(`group by`) , 默认返回该组的任意一行。这种行为依赖于`sql_mode`设置中的`ONLY_FULL_GROUP_BY`。
- `PostgreSQL` 、`Oracle`、`SQL Server` 抛出错误。
- 其他 SQL 引擎，Presto、Hive、Spark 严格遵循 `ANSI SQL`标准，抛出错误。
- ClickHouse，对于未在`GROUP BY`子句中声明的非聚合列，它会隐式地使用`any()`聚合函数，这意味着它会从分组中任意选择一个值。

[`GREATEST(*`value1`*,*`value2`*,...)`



#### LeetCodeSQL 函数

**1. coalesce** 

`COALESCE(*`value`*,...) `： Returns the first non-`NULL` value in the list, or `NULL` if there are no non-`NULL` values.

The return type of [`COALESCE()`](https://dev.mysql.com/doc/refman/8.0/en/comparison-operators.html#function_coalesce) is the aggregated type of the argument types.



通常在 `outer join` 选择非空的列的场景中使用，例如：

The single result column that replaces two common columns is defined using the coalesce operation. That is, for two `t1.a` and `t2.a` the resulting single join column `a` is defined as `a = COALESCE(t1.a, t2.a)`, where:

```sql
COALESCE(x, y) = (CASE WHEN x IS NOT NULL THEN x ELSE y END)
```

[577. 员工奖金 - 力扣（LeetCode）](https://leetcode.cn/problems/employee-bonus/)

考虑以下SQL：

```sql
select Employee.name as name,
    bonus 
from Employee left join Bonus on Employee.empId = Bonus.empId
where coalesce(Bonus.bonus, 0) < 1000
```



**2. count**

`count (1)`     <=>  `count(*)`

`count(col)`   <=> `col`列中非空（非NULL）值





#### 分支语法

模式匹配：case operation

if/else 分支：`if()`、`IFNULL()` 



#### join

笛卡尔积考虑减少连接次数



