1. 分支结构
  case when  if
2. 行专列，反第一范式（反规范化），
3. 列转行，规范花 explore函数 不加 laterval 和 加上laterval的表现
4. 对json的处理

### SQL 方言增强

#### 1. 分支结构

​	[条件函数](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-ConditionalFunctions)

#### 2. 行转列

多行转为一列，反规范化（属性不可再分）

score (student_id, class, score)， 把每个人所有科目分数捏在一起

```mysql
select name ,  collect_set(score) 
from score group by name 
```

把每个人分数组成 (class , score) 的键值对

```mysql
select name , concat_ws(",",class, cast(score as string)) from score;
```

相关函数

`CONCAT(string A/col, string B/col…)：`返回输入字符串连接后的结果，支持任意个输入字符串;

`CONCAT_WS(separator, str1, str2,...)：`它是一个特殊形式的 CONCAT()。第一个参数剩余参数间的分隔符。分隔符可以是与剩余参数一样的字符串。如果分隔符是 NULL，返回值也将为 NULL。这个函数会跳过分隔符参数后的任何 NULL 和空字符串。分隔符将被加到被连接的字符串之间;

`COLLECT_SET(col)：`函数只接受基本数据类型，它的主要作用是将某字段的值进行去重汇总，产生array类型字段。



#### 3.列转行

一行转多行，这是一个规范化的过程。使用到的这些函数被称为UDTF函数



EXPLODE(col)：将hive一列中复杂的array或者map结构拆分成多行。

在select 单独一个字段时候会拆成多行，如果select 还要引用其他字段，则需要配合lateral view 



LATERAL VIEW

​	用法：LATERAL VIEW udtf(expression) tableAlias AS columnAlias

[LanguageManual LateralView - Apache Hive ](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView)



