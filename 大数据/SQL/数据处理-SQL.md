#### 例子

A -> B ->C

子查询

join

group by 后 select 只能引用到分组列和聚合函数引用的列

### 4.4 相关更新

 UPDATE table1 alias1  
 SET    column = (SELECT expression  
                  FROM   table2 alias2  
                  WHERE  alias1.column = alias2.column);

使用相关子查询依据一个表中的数据更新另一个表的数据。

### 4.4 相关删除

  DELETE FROM table1 alias1  
  WHERE column operator (SELECT expression  
                         FROM   table2 alias2  
                         WHERE  alias1.column = alias2.column);

使用相关子查询依据一个表中的数据删除另一个表的数据。

构造虚拟表