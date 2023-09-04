## 概述

什么是join：两表或多表的数据进行连接操作。

关联查询的基础是笛卡尔积。

**join 两种理解：**

1. 先做笛卡尔积，然后筛选；这是关系代数的角度
2. 对于等值连接，就是for循环A表，查找B表符合的条件。这是查询的角度，查询A表中某个key对应的B表value的数据。



## 分类

标准规定了常见join类型

- inner join
- left join 、 right join
- full out join

还有一些根据语义的分类

- 等值连接，非等值连接



#### 1. 外连接

存在 LEFT、RIGHT 和 FULL OUTER 联接是为了更好地控制不匹配的 ON 子句。

- left join  ： 保留左边的全量，右表匹配不到的为null。

	on 子句对B表的限制语义是：满足on条件的右表的行和左表连接，右表不满足的返回null，结果返回左表的全部行。

	总之，左连接会返回满足on子句所有的左表的数据。

- right join ： 和left join相反

- full join  ： 保留左右两边的，不匹配的为null



对于外连接还有一些要点

##### on，where 条件的限制（以left join为例

- 连接发生在join之前。因此，要限制连接的输出，要求位于where子句中，否则位于join子句中。这个对外连接区别很大：

	```sql
	SELECT a.val, b.val FROM a LEFT OUTER JOIN b ON (a.key=b.key)
	WHERE a.ds='2009-07-07' AND b.ds='2009-07-07'
	```

	由于where 子句引用了 b 表的列，b.key 所有没有连接上的输出行都会被过滤，这不符合left join 的语义。换句话说，如果在where子句中引用了b的列，那么left join 则无关紧要。

	

- on 子句的条件只决定是否可以连接上，无论是否满足on条件，左侧的所有记录都会返回。

	```
	create table al(`key` int,val int);
	create table br(`key` int,val int);
	
	insert into al values(1,'1'), (2,'2'),(3,'3'),(4,'4');
	insert into br values(3,'3'),(4,'4'),(5,'5'),(6,'6');
	
	
	mysql> select * from al left join br on al.key = br.key and al.key > 2;
	+------+------+------+------+
	| key  | val  | key  | val  |
	+------+------+------+------+
	|    1 |    1 | NULL | NULL |
	|    2 |    2 | NULL | NULL |
	|    3 |    3 |    3 |    3 |
	|    4 |    4 |    4 |    4 |
	+------+------+------+------+
	
	mysql> select * from al left join br on al.key = br.key and br.key > 3;
	+------+------+------+------+
	| key  | val  | key  | val  |
	+------+------+------+------+
	|    1 |    1 | NULL | NULL |
	|    2 |    2 | NULL | NULL |
	|    3 |    3 | NULL | NULL |
	|    4 |    4 |    4 |    4 |
	+------+------+------+------+
	```

- 谓词下推的遇到做左连接会有一些原则，并不是所有的过滤条件都能提前。ps，有些时候数据库系统优化不够，在不改变逻辑的前提下进行手动进行谓词下推（减少数据量）。

	```sql
	SELECT a.val, b.val FROM a LEFT OUTER JOIN b  ON (a.key=b.key  )
	where a.ds='2009-07-07'
	
	-- 等价于
	
	SELECT a.val, b.val FROM (select * from a where a.ds='2009-07-07' ) a 
	LEFT OUTER JOIN b  ON (a.key=b.key )
	
	```

	这必须符合谓词下推的原则,参考:

	[OuterJoinBehavior - Apache Hive - Apache Software Foundation](https://cwiki.apache.org/confluence/display/Hive/OuterJoinBehavior)

**总结:**

- 在left join，on 条件只是表示是否能连接上，无论是否满足on条件都返回左表的全部行。

- 在 left join中，其后的where条件如果引用右表的列，考虑是否已经改变了leftjoin的语义。



### MySQL

[[MySQL-SQL方言#3. 关联查询]]



### Hive

[LanguageManual Joins - Apache Hive](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Joins#LanguageManualJoins-PredicatePushdowninOuterJoins)

比较重要的

- [left join on 条件的语义](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Joins#LanguageManualJoins-PredicatePushdowninOuterJoins:~:text=Joins%20occur%20BEFORE%20WHERE%20CLAUSES.%20So%2C%20if%20you%20want%20to%20restrict%20the%20OUTPUT%20of%20a%20join%2C%20a%20requirement%20should%20be%20in%20the%20WHERE%20clause%2C%20otherwise%20it%20should%20be%20in%20the%20JOIN%20clause.%20A%20big%20point%20of%20confusion%20for%20this%20issue%20is%20partitioned%20tables%3A)
- [外连接在谓词下推的行为](https://cwiki.apache.org/confluence/display/Hive/OuterJoinBehavior)

### Presto



### PostgreSQL / Greenplum



### ClickHouse