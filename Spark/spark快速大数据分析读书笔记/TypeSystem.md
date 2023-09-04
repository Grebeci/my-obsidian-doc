Spark基本数据类型与Scala数据类型

| Spark数据类型 | 描述                    | Scala数据类型        | 用于实例化API           |
| ------------- | ----------------------- | -------------------- | ----------------------- |
| ByteType      | 表示1字节有符号整数。   | Byte                 | `DataTypes.ByteType`    |
| ShortType     | 表示2字节有符号整数。   | Short                | `DataTypes.ShortType`   |
| IntegerType   | 表示4字节有符号整数。   | Int                  | `DataTypes.IntegerType` |
| LongType      | 表示8字节有符号整数。   | Long                 | `DataTypes.LongType`    |
| FloatType     | 表示4字节单精度浮点数。 | Float                | `DataTypes.FloatType`   |
| DoubleType    | 表示8字节双精度浮点数。 | Double               | `DataTypes.DoubleType`  |
| BooleanType   | 表示布尔值。            | Boolean              | `DataTypes.BooleanType` |
| StringType    | 表示字符字符串值。      | String               | `DataTypes.StringType`  |
| DecimalType   |                         | java.math.BigDecimal | `DecimalType`           |

除了 DecimalType，其他 数据类型都是 DataTypes 类的子类。



spark 结构化数据类型 和scala 数据类型



| Spark数据类型 | 描述                           | Scala数据类型                                      | 实例化API                                             |
| ------------- | ------------------------------ | -------------------------------------------------- | ----------------------------------------------------- |
| BinaryType    | 表示字节序列值。               | Array[Byte]                                        | `DataTypes.BinaryType`                                |
| DateType      | 表示日期值。                   | java.sql.Date                                      | `DataTypes.DateType`                                  |
| TimestampType | 表示时间戳值。                 | java.sql.Timestamp                                 | `DataTypes.TimestampType`                             |
| ArrayType     | 表示由相同类型元素组成的序列。 | scala.collection.Array                             | `DataTypes.createArrayType(elementDataType)`          |
| MapType       | 表示键值对的映射。             | scala.collection.Map                               | `DataTypes.createMapType(keyDataType, valueDataType)` |
| StructField   | 表示结构化数据中的一个字段。   | 表示字段类型定义的值<br />Tuple2[String, DataType] | `StructType(ArrayType[fieldTypes])`                   |
| StructType    | 表示字段类型定义的值           | org.apache.spark.sql.Row                           | `StructField(name, dataType, [nullable])`             |



实例化这些数据类型也可以使用 `DataTypes.createXXX` 来创建



## DataFrame API

##### 定义 DataFrame 的表结构

- API方式

	```scala
	val schema = StructType(
	  Array(
	    StructField("name", StringType, true),
	    StructField("age", StringType, true)
	  )
	)
	```
	
- DDL方式

  ```scala
  val schema = "name String, age int"
  ```

- spark推断

	```scala
	spark.read.option("samplingRatio", 0.001)  // 让spark用较小的代价去推断表结构
	```

	

##### 列和表达式

列是具有公共方法的对象

scala 中 Column 是列对象的类名， col 是标准的内建函数，返回一个Column对象

```scala
df.col("name")

df.select(col(id))

df.select(col(id).desc)
```

借助 $"columnName" 隐式转换

```scala
import spark.implicits._  
df.col($"name")  // StringToColumn
```

直接引用

```scala
df("name")
```



**表达式**

可以用 expr("ColumnName * 5") 或 (expr("columnName - 5") > col(anothercolumnName)) 创建表达式

等价于 col(columnName) * 5



**虚拟列**

withColumn



##### 行

DataFrame <=> DataSet[Row]

Row : 有序字段的集合。

构造 ： def apply(values: Any*): Row = new GenericRow(values.toArray)



#### 构造DataFrame

1. seq, list => toDF

2. spark读取外部文件，创建df

3. rdd 转换 

	spark.sparkContext.parallelize => rdd => RDD[Row] 

	RDD[Row] + schema => dataframe



#### read & write

**DataFrameReader 类API** 

```scala
DataFrameReader.format(args).option("key", "value").schema(args).load()
```

- `DataFrameReader` 实例只能从 `SparkSession` 中取到

```scala
SparkSession.read
SparkSession.readStream
```



![DataFrameReader args 简单解释](https://s2.loli.net/2023/04/20/xKk47yPS8LNaMRs.png)



- 静态Parquet数据读数据不需要提供表结构。Parquet文件的元数据通常包含表结构信息。



**DataFrameWriter 类API**

```scala
DataFrameWriter.format(args)
 .option(args)
 .bucketBy(args)
 .partitionBy(args)
 .save(path)

DataFrameWriter.format(args).option(args).sortBy(args).saveAsTable(table)
```

实例

```
DataFrame.write
DataFrame.writeStram
```

![DataFrameWriter args解释](https://s2.loli.net/2023/04/20/HtkrdZb8Fu41O5A.png)



#### DSL风格API

执行顺序是从上到下的执行，和SQL的执行顺序不同

- 投影： select
- 过滤： where
- 聚合： agg, groupBY, OrderBy, count(), 描述性聚合函数（min,max,sum,avg)

- 别名： 或者修改列名

	修改一列或几列  alias ，withColumnRenamed

	全部修改：toDF重新映射

	```scala
	// 所有的列加后缀
	val suffix = "_new"
	val newColumns = df.columns.map(_ + suffix)
	val newDF = df.toDF(newColumns: _*)
	```

	

DatasetAPI

在 Spark 中，Dataset 有两种特性：有类型和无类型。例如：DataFrame 是一个无类型的 Dataset，等价于DataSet[Row]。

有类型还是无类型取决于是不是在编译时检查类型还是在运行时检查类型。



用 case class 或者javaBean来指定表结构

#### SparkSQL

API方式 ： `cacheTable`

这个函数是lazy的，SQL方式加关键字 `lazy`即可

SQL方式

```
CACHE [LAZY] TABLE UNCACHE TABLE
UNCACHE TABLE
```







todo

1. scala 的import的方式，包系统

2. 泛型方法和函数上下文约定

	```
	  implicit def localSeqToDatasetHolder[T : Encoder](s: Seq[T]): DatasetHolder[T] = {
	    DatasetHolder(_sqlContext.createDataset(s))
	  }
	```

3. 



