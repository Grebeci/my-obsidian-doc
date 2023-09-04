如果目标表是 textfile ， spark 使用 rdd 写文件比用dataframe会更快

```
df.repartition(1).rdd.map(_.mkString("\t")).saveAsTextFile("test.txt")  

df.repartition(1).write.text("test.txt")
```


- 可能原因是在数据量很大的时候，dataframe 需要列原信息，而rdd直接转为把每行转为字符串快-- 来自copilot
验证情况
