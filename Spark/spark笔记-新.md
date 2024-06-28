## 启动 Spark Job

Spark 部署模式，参考 ：[Launching on a Cluster](https://spark.apache.org/docs/3.0.0/#launching-on-a-cluster)

#### Yarn方式

**1. 配置**

保证 `HADOOP_CONF_DIR`或 `YARN_CONF_DIR`指向包含Hadoop集群(客户端)配置文件的目录。



**2.启动**

```
$ ./bin/spark-submit --class org.apache.spark.examples.SparkPi \
    --master yarn \
    --deploy-mode cluster \
    --driver-memory 4g \
    --executor-memory 2g \
    --executor-cores 1 \
    --queue thequeue \
    examples/jars/spark-examples*.jar \
    10
```

`--deploy-mode cluster | client `   ： 主要区别在于：Driver程序的运行节点。

- cluster 模式下由于Driver节点不确定。如果想引用 `spark-submit` 所在进程下文件，可以使用 `--file` 参数。

[Maven – Maven Getting Started Guide (apache.org)](https://maven.apache.org/guides/getting-started/index.html)



## Spark 数据结构

### 1. RDD

##### 数据分区

数据被分为多个块，这些块可以分布在不同的节点上，从而允许多个任务并行执行。很多算子都有整体 Partition 操作签名，要考虑是否有必要使用这个分区特性提高性能。

**分区规则**

- 继承上一个分区规则，
- 有 `XxxByKey` 的算子默认使用 `HashPartitioner` 来分区，也可以自定义分区规则。



##### 序列化

RDDs 引用的函数（如传递给 `map`、`filter` 等的函数）和











### Spark API

`SparkContext`

- 提供 `Java`，`Scala` , `Python` 等 API。在 java 中使用 `JavaSparkContext` ，在 Scala 用 `SparkContext`

##### 创建 RDD

```
<SparkContext>.textFile(path, minPartitions)
```

从外部文件创建RDD



```
def makeRDD[T: ClassTag](
	seq: Seq[T],
  numSlices: Int = defaultParallelism
): RDD[T]
```

从 Scala `Seq` 原生集合上创建。



```
<SparkContext>  JavaRDD<T> parallelize(List<T> list, int numSlices)

<SparkContext>  def parallelize[T: ClassTag](
    seq: Seq[T],
    numSlices: Int = defaultParallelism
): RDD[T]
```

从java `List`  / scala `Seq` 中创建。



##### TransFormation

分为 Value 型，和 key-value型



``` 
<JavaRDD> :  <R> JavaRDD<R> map(Function<T,R> f)
<RDD>:       defmap[U](f: (T) ⇒ U)(implicit arg0: ClassTag[U]): RDD[U]
```

map 函数 ： 1对1 



```
<U> JavaRDD<U> flatMap(FlatMapFunction<T,U> f)
flatMap[U](f: (T) ⇒ TraversableOnce[U])(implicit arg0: ClassTag[U]): RDD[U]
```

flatMap : 1对多



```
<U> JavaPairRDD<U,Iterable<T>>	groupBy(Function<T,U> f)

def  groupBy[K](f: (T) ⇒ K): RDD[(K, Iterable[T])]
```

- 多个重载版本

- 宽依赖操作，涉及重分区。

  

```
public JavaRDD<T> filter(Function<T,Boolean> f)

def filter(f: (T) ⇒ Boolean): RDD[T]
```



```
JavaRDD<T>	distinct()
def distinct(): RDD[T]
```

- 宽依赖操作，涉及重分区



```
<S> JavaRDD<T>	sortBy(Function<T,S> f, boolean ascending, int numPartitions)
def sortBy[K](f: (T) ⇒ K, ascending: Boolean = true, numPartitions: Int = this.partitions.length): RDD[T]
```

- 宽依赖，设计重分区



**key-value型**

有 key，则一些重分区的操作（shuffle）分区规则和key相关。

java api下，需要保证 RDD 是 `PariRDD`，



```
<JavaPairRDD> :     public <U> JavaPairRDD<K,U> mapValues(Function<V,U> f)

<PairRDDFunctions> : def mapValues[U](f: (V) ⇒ U): RDD[(K, U)]
```



```
public JavaPairRDD<K,Iterable<V>> groupByKey()
public JavaPairRDD<K,Iterable<V>> groupByKey(int numPartitions)
public JavaPairRDD<K,Iterable<V>> groupByKey(Partitioner partitioner)

def groupByKey(): RDD[(K, Iterable[V])]
def groupByKey(numPartitions: Int): RDD[(K, Iterable[V])]
def  groupByKey(partitioner: Partitioner): RDD[(K, Iterable[V])]
```

默认采取  `HashPartitioner` 策略，也可以自定义分区器。



```
public JavaPairRDD<K,V> reduceByKey(Function2<V,V,V> func)
public JavaPairRDD<K,V> reduceByKey(Partitioner partitioner, Function2<V,V,V> func)
public JavaPairRDD<K,V> reduceByKey(Function2<V,V,V> func, int numPartitions)

def reduceByKey(func: (V, V) ⇒ V): RDD[(K, V)]
def reduceByKey(func: (V, V) ⇒ V, numPartitions: Int): RDD[(K, V)]
def reduceByKey(partitioner: Partitioner, func: (V, V) ⇒ V): RDD[(K, V)]
```



Action 算子

所有数据都会被拉取到 Driver 端



```
java.util.List<T> collect()

def collect(): Array[T]
```



```
void foreach(VoidFunction<T> f)

def foreach(f: (T) ⇒ Unit): Unit
```



```
saveAsTextFile
saveXxx
```



```
count, first, take
```

















