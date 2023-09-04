##### 1.基本查询

https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Select

查询语句语法：

```sql
SELECT [ALL | DISTINCT] select_expr, select_expr, ...

 FROM table_reference

 [WHERE where_condition]

 [GROUP BY col_list]

 [ORDER BY col_list]

 [CLUSTER BY col_list

  | [DISTRIBUTE BY col_list] [SORT BY col_list]

 ]
 [LIMIT number]

```

##### 分支

##### Join

Hive支持通常的SQL JOIN语句，但是只支持等值连接，不支持非等值连接（hive2.2+支持

联接发生在 WHERE 子句之前。因此，如果要限制连接的输出，则要求应位于 WHERE 子句中，否则应位于 JOIN 子句中

left join : on 后条件只对右表有效，where 条件只对左表有效





[OuterJoinBehavior - Apache Hive - Apache Software Foundation](https://cwiki.apache.org/confluence/display/Hive/OuterJoinBehavior)

[LanguageManual Joins - Apache Hive - Apache Software Foundation](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Joins)

[左外连接 where条件 on条件_rudy_gao的博客-CSDN博客_左连接加条件](https://blog.csdn.net/rudygao/article/details/29645221)



```sql
# 返回 S1表全集
select s1.key, s2.key from src s1 left join src s2 on s1.key = s2.key and s1.key > '2';

#右表S2.key <= 2 记录都会成为NULL记录
select s1.key, s2.key from src s1 left join src s2 on s1.key = s2.key and  s2.key > '2';

# 过滤出 s1.key<=2的记录
select s1.key, s2.key from src s1 left join src s2 s1.key = s2.key where s1.key > '2';

# 等价于 inner join
select s1.key, s2.key from src s1 left join src s2 s1.key = s2.key where s2.key > '2';
```



`ON` ：



左外连接：JOIN操作符左边表中符合WHERE子句的所有记录将会被返回。

右外连接：JOIN操作符右边表中符合WHERE子句的所有记录将会被返回。

满外连接：将会返回所有表中符合WHERE语句条件的所有记录。



多表连接（内外混合）是左结合的（从左到右执行）



##### 窗口函数

##### Operator & UDF 

###### 谓词操作符

1. 日期时间转换
2. NULL值函数
3. 字符串截取，拼接

##### UDTF

##### 自定义函数



