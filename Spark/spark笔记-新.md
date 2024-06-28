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



### Spark API

##### 创建 RDD

- `<SparkContext>.textFile(path, minPartitions)`
- `def makeRDD[T: ClassTag](Seq[T] seq, Int numSlices = defaultParallelism): RDD[T]`   ：Scala 特有
- 





