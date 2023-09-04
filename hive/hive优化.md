调优必须先明确是不是值得调优，Amdahl定律告诉我们，调优之前一定要先评估需要优化的部分在整个系统的占比。

调优的时候有三个原则

1. 慢SQL的原因，必须明确这点，不要瞎猜，堆配置项（ 数据量大？ shuffle ？ ...)
2. 如何证明优化的效果？不能玄学优化。
3. 对其他部分的影响，是不是会有风险（比如提高资源占比、并行等。


逻辑优化和物理优化是数据库查询优化中的两个重要概念。

逻辑优化是指对查询语句进行重写，以改进查询执行计划，提高查询性能。逻辑优化不会改变查询的结果，只会改变查询的执行方式。例如，查询优化器可能会将一个复杂的嵌套查询重写为一个等价的连接查询，以提高查询性能。

物理优化是指选择最佳的数据访问方法和执行策略来执行查询。物理优化不会改变查询语句本身，只会改变查询的执行方式。例如，查询优化器可能会选择使用索引来加速数据访问，或者选择使用并行执行来提高查询性能。

总之，逻辑优化和物理优化都旨在提高查询性能，但它们采取的方法不同。逻辑优化通过重写查询语句来改进查询执行计划，而物理优化则通过选择最佳的数据访问方法和执行策略来提高查询性能。

##### 执行计划

```sql
EXPLAIN [EXTENDED | DEPENDENCY | AUTHORIZATION] query-sql
```


#### 1. 存储优化

1. 尽量建分区表
2. 列存&压缩

#### 2. 调大资源

1. Mapper/Reducer阶段JVM堆内存溢出参数调优

​	异常信息一般为：java.lang.OutOfMemoryError:Direct buffffer memory

这表示JVM申请使用操作系统的直接内存时，超过了最大限制，导致无法分配更多的空间


目前MapReduce主要通过两个组参数去控制内存：（将如下参数调大）

注意，这是hive翻译成MR任务时候，MR任务所需的资源参数，这是给集群Hadoop的配置。


Maper:

```hive
mapreduce.map.java.opts=-Xmx2048m(默认参数，表示jvm堆内存,注意是mapreduce不是mapred)

mapreduce.map.memory.mb=2304(container的内存）

前者必须小于后者的内存
```


Reducer:

```hive
mapreduce.reduce.java.opts=-=-Xmx2048m(默认参数，表示jvm堆内存)

mapreduce.reduce.memory.mb=2304(container的内存)

前者必须小于后者的内存
```

> 注意：因为在yarn container这种模式下，map/[reduce](https://so.csdn.net/so/search?q=reduce&spm=1001.2101.3001.7020) task是运行在Container之中的，所以上面提到的`mapreduce.map(reduce).memory.mb` 数值 都大于`mapreduce.map(reduce).java.opts`值的大小。`mapreduce.{map|reduce}.java.opts`能够通过Xmx设置JVM最大的heap的使用，一般设置为0.75倍的memory.mb，因为需要为java code等预留些空间

2. 设置 mapper 个数

	一般这个不常用。控制mapper数，就需要通过控制 split 文件的大小实现的，有一个应用场景是针对小文件合并，但是hive 可以通过设置CombineHiveInputFormat 来实现小文件合并。

	```hive
	 set mapreduce.input.fileinputformat.split.minsize =  # byte
	 set mapreduce.input.fileinputformat.split.maxsize =  # byte
	```

3. 设置reduce个数
一般来说，设置reduce个数的目的是为了控制输出数据的分区数和并行度，以提高作业的执行效率和负载平衡

```hive
set mapreduce.job.reduces = 15;
```

#### 3. 数据规模

分区表不要全表扫描，不要读取无关的列

#### 4. 底层的优化

##### 1) Vectorization (矢量化)

```bash
set hive.vectorized.execution.enabled = true;
set hive.vectorized.execution.reduce.enabled = true
```

##### 2) 谓词下推

hive可以自动进行谓词下推，但是在写SQL时候，尽量自己做到这一点

1. 两个表join时候，先用where 过滤各自的条件，然后再join。这样逻辑清晰

#####  3) Map 端聚合

```sql
 set hive.map.aggr=true; # 默认true 相当于 map 端执行 combiner
```

#### 5. Join 优化

###### 1) mapjoin

（1）设置自动选择 MapJoin 

```bash
set hive.auto.convert.join=true; #默认为 true
```

（2）大表小表的阈值设置（默认 25M 以下认为是小表）：

```bash
 set hive.mapjoin.smalltable.filesize=25000000;
```

可以在执行计划里面看到 Mapjoin ， 默认join 方式是 Common Join

###### 2) 笛卡尔积

join 两个key如果存在多对多，这时候会产生笛卡尔积，这时候依赖 代码/业务逻辑 上的优化。


#### 6. 数据倾斜

##### 原因

首先，数据倾斜的本质是，某个key对应的数据过多，被拉到的同一个分区上，造成某些reduce负载过高。

这是由于SQL实现上导致的， `group by`  、 `join`

这个过程伴随着 partition 和 shuffle ，如果因为数据特征（就是某些key数据多），可以尝试对这些入手

##### 现象

在 yarn 的ui上，可以看到大多数reduce 很快，只有个别的很慢

##### 解决

最好的办法是找到那个key引起的数据倾斜，进行针对性的优化。

```sql
select * from table where key = '倾斜的'
union 
select * from table where key != '倾斜的'
```

如果找不到，尝试在 partition & shuffle 做操作

###### group by

（1）是否在 Map 端进行聚合，默认为 True 

```bash
set hive.map.aggr = true; 
```

（2）在 Map 端进行聚合操作的条目数目 

```bash
set hive.groupby.mapaggr.checkinterval = 100000; 
```

（3）有数据倾斜的时候进行负载均衡（默认是 false） 

```bash
set hive.groupby.skewindata = true
# 用添加前缀方式进行负载均衡，但会生成两个MR JOB
```

###### join

1. map join 

2. 倾斜自动拆

	```sql
	# join 的键对应的记录条数超过这个值则会进行分拆，值根据具体数据量设置
	set hive.skewjoin.key=100000;
	# 如果是 join 过程出现倾斜应该设置为 true
	set hive.optimize.skewjoin=true;
	```

	如果开启了，在 Join 过程中 Hive 会将计数超过阈值 hive.skewjoin.key（默认 100000）的 倾斜 key 对应的行临时写进文件中，然后再启动另一个 job 做 map join 生成结果。通过 hive.skewjoin.mapjoin.map.tasks 参数还可以控制第二个 job 的 mapper 数量，默认 10000。 `set hive.skewjoin.mapjoin.map.tasks=10000;`


###### 通用的

加大map， reduce 的资源。

这里有一个常见的误解，就是增大reduce个数，如果是因为特定某些key导致的倾斜，那么增大reduce个数，依然改变不了这些key被拉到同一个分区上。


### 动态分区相关

[mapreduce - Specify minimum number of generated files from Hive insert - Stack Overflow](https://stackoverflow.com/questions/55372028/specify-minimum-number-of-generated-files-from-hive-insert)

灌数据使用在动态分区特性，可能：

1. 如果某些partition 量级大，会产生数据倾斜。

```
DISTRIBUTE BY  part_col, FLOOR(RAND()*100.0)%10; --10 files per partition
```

控制map 到reduce每个分区的文件数，然后看是否还调大reduce个数，可以：

```
set hive.exec.reducers.bytes.per.reducer=67108864; 
```

也可以强制设定reduce个数

```
mapreduce.job.reduces
```

### Trick

代码级别优化

##### count(distinct)

count(distinct)  =>  group by 



##### in/exist

用left join 改写

更高效的替代方案

left semi join

##### 过滤不必要的空值

过滤所有标签同时为空空值





问题 1： 输入时多个小文件，输出时多个小文件吗

问题 2： 如何快速的知道使用哪个优化

