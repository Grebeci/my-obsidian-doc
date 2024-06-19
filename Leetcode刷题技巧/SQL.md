#### 1. () => null

SQL 的查询结果是一个集合，但是题目要求是空集合就要返回 null ，如：

[176. 第二高的薪水 - 力扣（LeetCode）](https://leetcode.cn/problems/second-highest-salary/)



明确要求：

>  查询并返回 `Employee` 表中第二高的薪水 。如果不存在第二高的薪水，查询应该返回 `null(Pandas 则返回 None)` 。

##### 方法1 ：

```bash
SELECT () AS xxx
```

`()` ： 是一个 expression。

```sql
select 
 (
    select salary as SecondHighestSalary
    from (
        select 
            salary,
            dense_rank() over(order by salary desc ) as rk
        from Employee
    )t
    where rk = 2
    limit 1
) as SecondHighestSalary
```

`limit 1` 必要的，`select ()`  是表达式，只能接受一行。

##### 方法2：

聚合函数会接受集合，集合为空返回 null

```sql
  select max(salary) as SecondHighestSalary
  from (
      select 
          salary,
          dense_rank() over(order by salary desc ) as rk
      from Employee
  )t
  where rk = 2
```

`max ` 即去重，且集合为空返回 `null`
