##### 1. alter table add column 列的时候，忘记加 cascade

alter table 以后的新分区都会有值，旧分区select 新字段为NULL，覆写之后也为空。内外部表表现相同的行为

修正：

对于外部表：直接drop table , 然后修复元数据 （ `add  partition` 或者`msck repair`
对于内部表：先转为外部表，然后drop table，再修复元数据。

##### 2.alter table add column 大量的字段（原表超过1000个字段，分区超过1000个）

由于表的元数据是按分区为单位的，所以要对所有分区都执行一遍，时间很长而且失败了可能无法保证原子性。导致有的分区添加上了，有的分区没有添加上。

解决办法：

先检查是不是表的类型，如果是内部表，先转为外部表。然后拿到这个表的建表语句后删除这个表，重建这个表（加上要添加的字段）。最后使用 `msck repair` 修复元数据

##### 3. hive执行insert into 插入中文乱码

1.  使用 insert into ... select 代替使用 insert into ... values ,
    
2.  使用insert into ... select 还是乱码，使用decode 解码 （ insert into ... values 不能使用decode等函数
```hive
 insert into table_name partition(part_spec) select decode(binary('汉字') from xxx limit 1;
```
