Q: 为什么引入 DataFrame？ 

N : 
- 语义化的API，DSL表达的逻辑更清晰。可以借助SQL数据处理概念，选择、过滤、分组、聚合。
- 数据结构的支持。带有Schema的DataFrame，每列的数据类型由DataTypes 类的支持。
	- 创建DataFrame，schema方式和API方式。
	- 列的支持：col对象，Column API的支持，更加方便implicit  `$` 
	- 行的支持：Row对象,
	- 统计方法的支持，`import org.apache.spark.sql.functions` 

Q: 为什么引入 DataSet

- 有类型的 DataFrame , `DataFrame = DataSet[Row]` ， 这个类型是指 Scala 中强类型的 JVM 对象，一般是case class。
- 除了对二维表结构的支持，数据类型还支持更加通用的对象。

