### 启动 Spark Job

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

`--deploy-mode`  ：