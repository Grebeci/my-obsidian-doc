### 1. TopN 

所谓的分组取topN，也就是 对key 进行分组，取每个key对应的第几个。例如 score(student_id, course, score) , 求每科的第N名学生。

- 没有分组的要求，也就是取整个集合的第N条。直接 使用`limit N` 就行。但是对于并列的，无法直接使用 limit方法。

- 对于有分组要求的，就需要用`窗口函数` 或者`笛卡尔积`的思路了。

##### 使用窗口函数的思路

例子： score(student_id, course, score) , 求每科分数最高的第N名学生 

```mysql
select 
	 student_id,
     course,
     score
from (
    select 
        student_id,
        course,
        score,
        rank() over(partition by course order by score desc)  as rk  -- rank/ dense_rank 看要求
    from score
)t
where rk = N
```



主要的排名窗口函数

row_number() , rank() , dense_rank()

##### 不使用窗口函数

例子：Employee(id,salary) ,求第N高的薪水的人。（注：可能有并列的情况，一并列出）

这个使用笛卡尔积的思路就是，对于每人，查找比这个人大的有几个，他就是第几名。这个方法对于重复的也可以解决。

```mysql
select 
	id,
	salary
from(
    select
        e1.id as id,
    	max(e1.salary) as salary,  -- 看情况是否需要对应的薪资
        count(e2.salary ) as rn    -- count(e2.salary) 对应 rank，count(distinct) 对应 dense_rank
    from Employee e1 join Employee e2 on e1.salary <= e2.salary
    group by e1.id
) t
where rn = N
```



例子： score(student_id, course, score) , 求每科分数最高的第N名学生 

思路相同，对于每科，查找比当前行大的分数有几个，只不过和之前例子相比增加一个 course 维度。

```mysql
select 
	course,
	student_id,
	score
from (
    select 
        s1.course,
        count(s2.score) as rn ,
        -- 如果需要列出其他维度
        max(s1.student_id) as student_id,
        max(s1.score) as score,
    from score s1 join s2 on s1.course = s2.course and s1.score <= s2.score
    group by s1.course
)t
where t.rn = N

```

### 2. 派生表

#### 生成连续的子序列

- 借助已经存在表，得到每行的编号

	```mysql
	select row_number()  as id from t 
	```

- cross join （笛卡尔积）

	先生成 0-9 数字，然后使用公式 n = 个位 * 1 + 十位 * 10 + 百位 * 100 + ... 来表示任意一个数

	```mysql
	--生成 0-9999数字
	with base as (
	    select n from (
	        VALUES ROW(0), ROW(1),ROW(2),ROW(3),ROW(4),ROW(5),ROW(6),ROW(7),ROW(8),ROW(9)
	    ) p(n)
	  )
	select
	    a.n+b.n * 10+c.n * 100+d.n * 1000 AS n
	from base a
	cross join base b
	cross join base c
	cross join base d
	order by n
	
	--- 对于不支持 values子句的
	with base as (
	    select n from (
	       select 0 as n  union select 1 union select 2 union select 3 union select 4 union
	        select 5 union select 6 union select 7 union select 8 union select 9
	    ) p(n)
	  )
	select
	    a.n+b.n * 10+c.n * 100+d.n * 1000 AS n
	from base a
	cross join base b
	cross join base c
	cross join base d
	order by 1
	
	```

	也可以使用cross join 结果集中数据行的数量是左右量表行数的乘积的效果，快速膨胀数量，然后使用 row_number 得到行序号

	```mysql
	-- 生成 1-10000数字 ( row_number 从1开始)
	with base as (
	    select n from (
	        VALUES ROW(0), ROW(1),ROW(2),ROW(3),ROW(4),ROW(5),ROW(6),ROW(7),ROW(8),ROW(9)
	    ) p(n)
	  )
	select
	    row_number()  AS n
	from base a
	cross join base b
	cross join base c
	cross join base d
	```



- 递归

	```mysql
	with recursive t(n) as (
	    select 0 as n
	    union all
	    select n+1 from t where n <= 99
	)
	select * from t
	```

	借助这个特性，可以生成日期序列

	参考 

	[ MySQL8.0 doc 递归示例](https://dev.mysql.com/doc/refman/8.0/en/with.html#common-table-expressions-recursive-examples)

### 3. 分组排除

```
key   value
1      10
1      20
1      30
2      10
2      20
3      10
```



1. 仅包含30 的id

2. 包含 10 且不包含 20

3. 在 10和20之间的（包含边界）

	[1084. 销售分析III - 力扣（LeetCode）](https://leetcode.cn/problems/sales-analysis-iii/comments/)






三个列的透视表（行转列）

[1179. 重新格式化部门表 - 力扣（LeetCode）](https://leetcode.cn/problems/reformat-department-table/)

排除最大值，最小值
[几乎双百的解答 - 活动参与者 - 力扣（LeetCode）](https://leetcode.cn/problems/activity-participants/solution/ji-hu-shuang-bai-de-jie-da-by-ffreturn-4931/)

```mysql
select activity as ACTIVITY
from friends
group by activity
having count(*)>any(
    select count(*) from friends group by activity
) and count(*)<any(
    select count(*) from friends group by activity
)
```



### 4. 去重

[196. 删除重复的电子邮箱 - 力扣（LeetCode）](https://leetcode.cn/problems/delete-duplicate-emails/description/)



### 5. 组内行间关系

向前偏移 N 行、向后偏移N行 和当前行的关系

[197. 上升的温度 - 力扣（LeetCode）](https://leetcode.cn/problems/rising-temperature/)





















