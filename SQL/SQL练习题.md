#### 第一题

表 T(id,course,score)，

- 每个学生成绩最高的科目
	- 有重复的都列出来
	- 有重复的随机列出一条

**分析**

这个是典型的TOPN，也是SQL的局限性：分组后，对每组只能使用聚合函数。



有重复的都列出来

```sql
select *
from (
	select id,course,score,
			rank() over(partition by id order by score desc ) as rk
    from t
)t
where rk = 1
```



有重复的随机选一条

```sql
select id, course, score
from (
	select id,course,score,
			row_number() over(partition by id order by score desc ) as rn
    from t
)t
where rn = 1

-- 或者 : 有重复去重

select 
	id, min(course) as course, min(score) as score
from (
	select id,course,score,
			rank() over(partition by id order by score desc ) as rk
    from t
)t
where rk = 1
group by id 

```



但是这样是绝对错误的

```sql
select 
	id, course, min(score) as score
from score
group by id 
```

在 MySQL 上，虽然可以 select 取其他字段时候，默认取每组的第一条，但是 course 和 min(score) 是没有对应关系的。 