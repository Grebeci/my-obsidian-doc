### 启动 Spark Job

Spark 部署模式，参考 ：[Launching on a Cluster](https://spark.apache.org/docs/3.0.0/#launching-on-a-cluster)

##### Yarn方式

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

