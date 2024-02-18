#### SQL 的执行顺序

在 SQL 查询中，逻辑执行顺序（也就是 SQL 的解析顺序）大致如下：

1. `FROM`子句：确定要从哪个表中检索数据。
2. `WHERE`子句：对数据进行过滤。
3. `GROUP BY`子句：对数据进行分组。
4. `HAVING`子句：对分组后的数据进行过滤。
5. `SELECT`子句：选择要输出的列，如果有`DISTINCT`，则在这一步去除重复的行。
6. `ORDER BY`子句：对结果集进行排序。
7. `LIMIT`子句：限制结果集中的行数。



**1. Distinct 是先于 Limit 执行的**

 [176. 第二高的薪水](https://leetcode.cn/problems/second-highest-salary/)

考虑以下SQL 

```sql
SELECT DISTINCT salary
FROM Employee
ORDER BY salary DESC
LIMIT 1 OFFSET 1
```

`SELECT`（包括`DISTINCT`的处理）实际上是在`LIMIT`之前发生的。因此，首先执行`SELECT DISTINCT salary`去除重复的薪水值，然后对这个去重后的结果集应用`ORDER BY`和`LIMIT`。

我们可以逻辑上理解为：首先从`Employee`表中选择不重复的`salary`值，然后按照`salary`降序排列这些唯一的值，最后选择第二条记录。这就确保了我们能得到第二高的薪水值，而不是简单地选择所有结果中的第二条记录。



**2. 别名不能在同一级别的where 引用**

因为`WHERE`子句在逻辑处理顺序中先于`SELECT`子句执行。逻辑上别名还没有被定义。