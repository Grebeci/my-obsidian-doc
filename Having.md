Having



含义：对分组后的每组进行过滤。

标准SQL语法

```
 [HAVING search condition]
```



search condition :

`groupby_colname operator 常量值`

​	operator : in , not in , `=,<...`

`groupby_colname operator (subquery)` : 

​	groupby_colname  只能引用group by 后的列或者常量（ 看例子的第二种种解法） 

`aggregate_func(col_name) operator 常量值;`

`aggregate_func(col_name) operator (sub_query);` 



>SQL92
>
>7.8  <having clause> ............................................189
>
>```
>         7.8  <having clause>
>
>         Function
>
>         Specify a grouped table derived by the elimination of groups from
>         the result of the previously specified clause that do not meet the
>         <search condition>.
>
>         Format
>
>         <having clause> ::= HAVING <search condition>
>
>
>         Syntax Rules
>
>         1) If neither a <where clause> nor a <group by clause> is speci-
>            fied, then let T be the result of the preceding <from clause>;
>            if a <where clause> is specified, but a <group by clause> is
>            not specified, then let T be the result of the preceding <where
>            clause>; otherwise, let T be the result of the preceding <group
>            by clause>. Each <column reference> directly contained in the
>            <search condition> shall unambiguously reference a grouping
>            column of T or be an outer reference.
>
>            Note: Outer reference is defined in Subclause 6.4, "<column
>            reference>".
>
>         2) Each <column reference> contained in a <subquery> in the <search
>            condition> that references a column of T shall reference a
>            grouping column of T or shall be specified within a <set func-
>            tion specification>.
>
>         3) The <having clause> is possibly non-deterministic if it contains
>            a reference to a column C of T that has a data type of character
>            string and:
>
>            a) C is specified within a <set function specification> that
>              specifies MIN or MAX, or
>
>            b) C is a grouping column of T.
>
>         Access Rules
>
>            None.
>
>         General Rules
>
>         1) Let T be the result of the preceding <from clause>, <where
>            clause>, or <group by clause>. If that clause is not a <group
>            by clause>, then T consists of a single group and does not have
>            a grouping column.
>
>         2) The <search condition> is applied to each group of T. The result
>            of the <having clause> is a grouped table of those groups of T
>            for which the result of the <search condition> is true.
>
>
>         3) When the <search condition> is applied to a given group of T,
>            that group is the argument or argument source of each <set func-
>            tion specification> directly contained in the <search condition>
>            unless the <column reference> in the <set function specifica-
>            tion> is an outer reference.
>
>            Note: Outer reference is defined in Subclause 6.4, "<column
>            reference>".
>
>         4) Each <subquery> in the <search condition> is effectively exe-
>            cuted for each group of T and the result used in the application
>            of the <search condition> to the given group of T. If any exe-
>            cuted <subquery> contains an outer reference to a column of T,
>            then the reference is to the values of that column in the given
>            group of T.
>
>            Note: Outer reference is defined in Subclause 6.4, "<column
>            reference>".
>
>         Leveling Rules
>
>         1) The following restrictions apply for Intermediate SQL:
>
>              None.
>
>         2) The following restrictions apply for Entry SQL in addition to
>            any Intermediate SQL restrictions:
>
>              None.
>```



MySQL

[13.2.7. SELECT语法_MySQL 中文文档 (mysqlzh.com)](https://www.mysqlzh.com/doc/126/646.html)

>SQL标准要求HAVING必须引用GROUP BY子句中的列或用于总计函数中的列。不过，MySQL支持对此工作性质的扩展，并允许HAVING因为SELECT清单中的列和外部子查询中的列。









例子： [mysql - 销售分析 II - 力扣（LeetCode）](https://leetcode.cn/problems/sales-analysis-ii/solution/mysql-by-hanxin_hanxin-pxw8/)



```sql
题目：

Table: Product
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| product_id   | int     |
| product_name | varchar |
| unit_price   | int     |
+--------------+---------+
product_id 是这张表的主键

Table: Sales

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| seller_id   | int     |
| product_id  | int     |
| buyer_id    | int     |
| sale_date   | date    |
| quantity    | int     |
| price       | int     |
+------ ------+---------+
这个表没有主键，它可以有重复的行.
product_id 是 Product 表的外键.


  编写一个 SQL 查询，查询购买了 S8 手机却没有购买 iPhone 的买家。注意这里 S8 和 iPhone 是 Product 表中的产品。


答案：


with temp as (select buyer_id, product_name
                from product p 
                right join sales s 
                on p.product_id = s.product_id)
-- 方法（1）：判断每个人的买的产品列表里是否有S8 和 没iphone，两个子查询，速度最慢
select buyer_id 
from sales
group by buyer_id
having  (buyer_id,'S8') in (select buyer_id, product_name from temp)
        and (buyer_id,'iPhone')  not in (select buyer_id, product_name from temp)
        



-- 方法（2）：从买了S8的人中除掉买了iphone的
select buyer_id 
from temp
where product_name = 'S8'
group by buyer_id 
having buyer_id not in (select buyer_id
                        from temp
                        where product_name='iPhone')





-- 方法（3）： 太巧妙了， 使用sum计数，判断是否买了s8和iphone，来自题解区大佬
select buyer_id
from temp
group by buyer_id
having sum(if(product_name = 'S8', 1, 0)) > 0
       and sum(if(product_name = 'iPhone', 1, 0)) = 0
```



在用left join 时,对左表限制用where,对右表限制用on



