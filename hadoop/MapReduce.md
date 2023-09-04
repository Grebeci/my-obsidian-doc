在hadoop 中，`MapReduce` 就是一个分布式程序的编程框架

# 概述

核心思想： 函数时编程的 map 和 reduce

map : 把输入数据映射成 kv 键值对

reduce： 按照 key 进行归约

MapReduce 实例进程 ： `Mrappmaster` `maptask` `reducetask`


# architecture

![[MapReduce组成.png]]

## MapTask 工作机制
![[maptask工作机制.jpg]]

map函数

```java
/*
* map方法接收四个参数：key表示输入数据的偏移量，value表示输入数据，context表示MapReduce的上下文对象，
* 在 wordCount中：key指的是每行文本数据的偏移量，value指的是每行文本数据。
*/

protected void map(LongWritable key, Text value, Context context) throws IOException, InterruptedException {
        String line = value.toString();
        StringTokenizer tokenizer = new StringTokenizer(line);
        while (tokenizer.hasMoreTokens()) {
            word.set(tokenizer.nextToken());
            context.write(word, one);
        }
    }
```

**以 WordCount 为例， 数据是：**

200M

```
Hello World
Hello Hadoop
....
xxxjob
zookeeper zpplin
```

（1）**Read阶段**：MapTask通过InputFormat获得的RecordReader，从输入InputSplit中解析出一个个key/value。

​    wordcount: key : 文本的偏移量 ， value: 一行数

```
(0,"Hello World")
(11,"Hello Hadoop")
```

（2）**Map阶段**：该节点主要是将解析出的key/value交给用户编写map()函数处理，并产生一系列新的key/value。

​    wordcount key : word ， value: 1

```
(“Hello”, 1)
(“World”, 1)
(“Hello”, 1)
(“Hadoop”, 1)
```

（3）**Collect阶段**：将MapTask生成的 key/value，分区号的序列化数据，并写入一个环形内存缓冲区中。

（4）**Spill阶段**：即“溢写”，当环形缓冲区满后，MapReduce会将数据写到本地磁盘上，生成一个临时文件。将数据写入本地磁盘之前，先要对数据进行一次本地排序，如果配置了combiner，还会将有相同分区号和key的数据进行排序。

	溢写阶段详情：	
	步骤1：利用快速排序算法对缓存区内的数据进行排序，排序方式是，先按照分区编号Partition进行排序，然后按照key进行排序。这样，经过排序后，数据以分区为单位聚集在一起，且同一分区内所有数据按照key有序。
	步骤2：按照分区编号由小到大依次将每个分区中的数据写入任务工作目录下的临时文件output/spillN.out（N表示当前溢写次数）中。如果用户设置了Combiner，则写入文件之前，对每个分区中的数据进行一次聚集操作。
	步骤3：将分区数据的元信息写到内存索引数据结构SpillRecord中，其中每个分区的元信息包括在临时文件中的偏移量、压缩前数据大小和压缩后数据大小。如果当前内存索引大小超过1MB，则将内存索引写到文件output/spillN.out.index中。

```
(partition, (key,value))
(1,(“Hello”, 1))
(1,(“Hello”, 1))
(2,(“World”, 1))
(2,(“Hadoop”, 1))
```

（5）**Merge阶段**：当所有数据处理完成后，MapTask对所有临时文件进行一次合并，以确保最终只会生成一个数据文件。

当所有数据处理完后，MapTask会将所有临时文件合并成一个大文件，并保存到文件output/file.out中，同时生成相应的索引文件output/file.out.index。

```
(1,(“Hello”, 1)) (1,(“Hello”, 1)) (2,(“World”, 1)) (2,(“Hadoop”, 1))
|-------------part1-------------| |-------------part2---------------|
```



让每个MapTask最终只生成一个数据文件，可避免同时打开大量文件和同时读取大量小文件产生的随机读取带来的开销。

### MapTask 并行度决定机制

客户端在提交job时的切片数决定。

client 在提交job时候，会对数据集每一个文件进行逻辑切片，生成切片规划，提交到yarn上。yarn 根据切片规划（job.xml），一个split切片生成一个maptask , 默认情况下切片大小=blockSize。

>数据块（block）： hdfs存储数据的单位，一般是128M
>数据切片（split）: 对数据集的逻辑切片，是MR的程序计算的输入单位，一个切片启动一个maptask


源码层面：
切片的具体实现是由 `inputFormat  `抽象类的具体子类决定的，默认是`FileInputFormat`

```java
public abstract class InputFormat<K, V> {

 /**
     * 仅仅是逻辑分片，并没有物理分片，所以每一个分片类似于这样一个元组 <input-file-path, start, offset>
     */
    public abstract List<InputSplit> getSplits(JobContext context)
            throws IOException, InterruptedException;

}
```
##### 切片流程：

1. 遍历数据目录的每个文件
2. 针对每个文件
	1. 得到文件大小
	2. 计算切片的大小
	3. 生成切片规划，写入inputSplit中
	4. 提交到yarn上，MrAppmaster 根据切片规划启动对应的maptask

#### 具体策略

1. FileInputFormat ： 

	**切片机制**
	1. 按照文件长度进行切片，切片大小等于blockSize
	2. 不考虑数据集整体，逐一对每个文件切片

	**计算切片**

	`computeSliteSize(Math.max(minSize,Math.min(maxSize,blocksize)))`

	mapreduce.input.fileinputformat.split.`minsize`=1 默认值为1
	mapreduce.input.fileinputformat.split.`maxsize`= Long.MAXValue 默认值Long.MAXValue
	默认切片大小=BlockSize，计算公式是在 minSize blockSize maxSize 中取一个中间值，min、max 类似上下限的作用

```
------------ ==> maxSize
------------ ==> BlockSize
------------ ==> minSize
```

1） 提高切片大小，增大minSize，让 minSize 大于BlockSize

2） 减小切片大小，减小maxSize, 让 maxSize 小于BlockSize

```java
// 根据文件类型获取切片信息
FileSplit inputSplit = (FileSplit) context.getInputSplit();
// 获取切片的文件名称
String name = inputSplit.getPath().getName();
```

2. TextInputFormat 

	TextInputFormat是默认的FileInputFormat实现类。按行读取每条记录。键是存储该行在整个文件中的起始字节偏移量， LongWritable类型。值是这行的内容，不包括任何行终止符（换行符和回车符），Text类型。

3. CombineTextInputFormat

	针对小文件的切片机制，看文档


## ReduceTask 工作机制
![[reduceTask工作机制.png]]



reduce 函数

```java
protected void reduce(Text key, Iterable<IntWritable> values, Context context) throws IOException, InterruptedException {
        // 定义一个变量sum，用于累加每个value
        int sum = 0;
        // 遍历每个value，并将其累加到sum中
        for (IntWritable val : values) {
            sum += val.get();
        }
        // 设置result的值为累加后的结果
        result.set(sum);
        // 输出一个key/value对，其中key为输入数据的key，value为累加后的结果
        context.write(key, result);
    }
```

以wordcount为例 (**这个文件200M**)

```
Hello World
Hello Hadoop
....
xxxjob
zookeeper zpplin
```

会启动两个MapTask， 产生两个文件

文件1

```
(“Hello”, 1))(“Hello”, 1)   (“World”, 1) (“Hadoop”, 1)
|----------part1---------| |----------part2------------|
```

文件2

```
(“xxxjob”, 1))             (“zookeeper”, 1) (“zeeplin”, 1)
|----------part1---------| |----------part2------------|
```



1）**Copy阶段**：ReduceTask从各个MapTask上远程拷贝一片数据，并针对某一片数据，如果其大小超过一定阈值，则写到磁盘上，否则直接放到内存中。

2）**Sort阶段**：在远程拷贝数据的同时，ReduceTask启动了两个后台线程对内存和磁盘上的文件进行合并，以防止内存使用过多或磁盘上文件过多。按照MapReduce语义，用户编写reduce()函数输入数据是按key进行聚集的一组数据。为了将key相同的数据聚在一起，Hadoop采用了基于排序的策略。由于各个MapTask已经实现对自己的处理结果进行了局部排序，因此，ReduceTask只需对所有数据进行一次归并排序即可。

reduce1

```
(“Hello”, 1))(“Hello”, 1)     (“xxxjob”, 1)) 
|--------文件1-part1--------| |------文件2-part1---------|
```

3）**Reduce阶段**：reduce()函数将计算结果写到HDFS上。

```
("hello",2) ("xxxjob",2)
```

#### reduceTask 数量决定

在MR程序中，reduceTask 数量是手动设置的

取值为 1 or 1 < reduceTask <= partition

reduceTask 数量决定输出文件的个数

>如果分区数不是1，但是reducetask为1，是否执行分区过程。
  答案是：不执行分区过程。因为在MapTask的源码中，执行分区的前提是先判断ReduceNum个数是否大于1。不大于1肯定不执行。

## Shuffle

Map方法之后，Reduce方法之前的数据处理过程称之为Shuffle。

一般把从 Map 产生输出开始到 Reduce 取得数据作为输入之前的过程称作 shuffle。shuffle 是 Mapreduce 的核心，它分布在 Mapreduce 的 map 阶段和 reduce 阶段。

shuffle阶段分为四个步骤：依次为：分区，排序，规约，分组，其中前三个步骤在map阶段完成，最后一个步骤在reduce阶段完成。

##### shuffle流程描述

（1）**Collect阶段** ：MapTask收集我们的map()方法输出的kv对，放到内存缓冲区中

（2）**Spill阶段** ：从内存缓冲区不断溢出本地磁盘文件，可能会溢出多个文件

（3）**Merge阶段**：多个溢出文件会被合并成大的溢出文件

> 在溢出过程及合并的过程中，都要调用Partitioner进行分区和针对key进行排序

（6）**Copy阶段** : ReduceTask会抓取到同一个分区的来自不同MapTask的结果文件，ReduceTask会将这些文件再进行合并（归并排序）

> ReduceTask根据自己的分区号，去各个MapTask机器上取相应的结果分区数据

（7）合并成大文件后，Shuffle的过程也就结束了，后面进入ReduceTask的逻辑运算过程（从文件中取出一个一个的键值对Group，调用用户自定义的reduce()方法）



以上是处理流程，下面是数据的变化





**注意：**

（1）Shuffle中的缓冲区大小会影响到MapReduce程序的执行效率，原则上说，缓冲区越大，磁盘io的次数越少，执行速度就越快。

（2）缓冲区的大小可以通过参数调整，参数：mapreduce.task.io.sort.mb默认100M

##### 1.partition

1.  partition 存在的意义
    

根据key重新划分数据，让相同key的数据进入一个reduce，让reduce并行化，partition

2.  HashPartition 是默认实现
    
     `(key.hashCode() & Integer.MAX_VALUE) % numReduceTask`
    
3.  partition 数量决定因素
    
    在默认实现 `HashPartition 中`，取决于`ReduceTask`设置的个数，（如果是默认实现，大多情况下两者相等）
    
4.  数据倾斜
    
    本质：某些key对应的数据量大，被拉到了同一分区下
    
    由于分区的原因，某些分区数据量大，导致某些reduce 处理数据量很大
    

##### 2. 排序

这是`shuffle`默认的功能，在shuffle过程中进行了三次

1.  在缓冲区其实是对（key,value,partition) 元组，现根据 Partition 排序，再根据 Key 排序
    
2.  mapTask缓冲区溢写到磁盘的文件(如果文件过多)会进行归并排序
    
3.  ReduceTask 对拉取到数据进行一次归并排序
    





