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

数据分区

数据被分为多个块，这些块可以分布在不同的节点上，从而允许多个任务并行执行。

分区规则









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

分为 Value 型，和 





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

deffilter(f: (T) ⇒ Boolean): RDD[T]
```





















