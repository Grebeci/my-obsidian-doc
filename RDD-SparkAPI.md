### Spark API （常见的转化和行动操作）

RDD 还有一个 collect() 函数，可以用来获取整个 RDD 中的数据。如果你的程序把 RDD 筛选到一个很小的规模，并且你想在本地处理这些数据时，就可以使用它。记住，只有当你的整个数据集能在单台机器的内存中放得下时，才能使用 collect()，因此，collect() 不能用在大规模数据集上。



在大多数情况下，RDD 不能通过 collect() 收集到驱动器进程中，因为它们一般都很大。此时，我们通常要把数据写到诸如 HDFS或 Amazon S3 这样的分布式的存储系统中。你可以使用 saveAsTextFile()、saveAsSequenceFile()，或者任意的其他行动操作来把 RDD 的数据内容以各种自带的格式保存起来。



#### 1. 转换操作

##### 1.1 针对各个元素的操作

###### map(func)

函数定义：` def map[U](f: T => U)(implicit scala.reflect.ClassTag[U]): RDD[U]`

功能： 将RDD中的每个元素进行映射。（将函数应用于 RDD 中的每个元素，将返回值构成新的 RDD

解释： 既可以转化数据，也可以转换类型。映射前后元素数量不变，是一对一的映射。

```scala
sc.parallelize(List(1,2,3,4,5)).map(x => x * x ).collect().mkString(",")

1,4,9,16,25
```



###### flatMap(func)

函数定义 ： `def flatMap[U](f: T => TraversableOnce[U])(implicit scala.reflect.ClassTag[U]): RDD[U]` 

功能：将RDD中的每个元素映射成0个或多个元素。

解释：可以转换类型，映射可以一对多。

```scala
var lines = sc.parallelize(List("hello world","hi"))
var words = lines.flatMap(line => line.split(" "))
words.collect()

words.collect()
res4: Array[String] = Array(hello, world, hi)
```



###### filter()

函数定义：def filter(f: T => Boolean): RDD[T]

功能：过滤元素，保留 函数f 为真的元素。



##### 1.2 伪集合操作

###### distinct()

函数定义： def distinct(numPartitions: Int)(implicit ord: scala.math.Ordering[T]): RDD[T] 

功能：去重RDD的元素

解释：distinct 会经过shuffle过程。



###### union(otherRDD)

函数定义：def union(other: RDD[T]): RDD[T]

功能： 合并两个RDD。

解释： 不会去重元素，必须相同的类型的RDD



###### intersection(otherRDD)

函数定义 ： def intersection(other: RDD[T]): RDD[T] 

功能：返回两个RDD都有的元素

解释：需要经过shuffle



###### subtract(otherRDD)

函数定义：def subtract(other: RDD[T]): RDD[T] 

功能：移除另一个 RDD 中的内容。a.subtract(b) ， 返回集合a-b

解释： 要经过shuffle



###### cartesian(otherRDD)

函数定义：`def cartesian[U](other: RDD[U])(implicit scala.reflect.ClassTag[U]): RDD[(T, U)]` 

功能：与另一个RDD的笛卡尔积。

解释： 要经过 shuffle。



#### 2. 行动操作

##### 2.1 单值规约操作

###### reduce(func)

函数定义：`def reduce(f: (T, T) => T): T` 

功能：并行整合RDD中的所有元素。它接收一个函数作为参数，这个函数要操作两个相同元素类型的RDD 数据并返回一个同样类型的新元素。

解释： 

```scala
// 集合累加：
sc.parallelize(List(1,2,3,4)).reduce((x,y) => x+y)
```



###### fold(zeroValue)(func)

函数定义：`def fold(zeroValue: T)(op: (T, T) => T): T` 

功能：  泛化的reduce，接收一个与 `reduce()` 接收的函数签名相同的函数，再加上一个“初始值”来作为每个分区第一次调用时的结果。

你所提供的初始值应当是你提供的操作的单位元素；也就是说，使用你的函数对这个初始值进行多次计算不会改变结果（例如 + 对应的 0，* 对应的 1，或拼接操作对应的空列表。

```scala
var lines = sc.parallelize(List(1,2,3,4))

lines.fold(0)((x,y) => x+y)
res7: Int = 10

lines.fold(2)((x,y) => x+y)
res8: Int = 16

lines.partitions.size
// 每个分区初始值是2 总共两个分区。
```



###### aggregate(zeroValue)(func)(func)

函数定义：`def aggregate[U](zeroValue: U)(seqOp: (U, T) => U, combOp: (U, U) => U)(implicit scala.reflect.ClassTag[U]): U `

功能：fold加强，aggregate()函数则把我们从返回值类型必须与所操作的 RDD 类型相同的限制中解放出来。聚合每个分区的元素，然后是所有分区的结果，使用给定的组合函数和中性的“零值”。

解释：

```scala
// 求平均值    
   val input = sc.parallelize(List(1, 2, 3, 4))
    val result = input.aggregate((0, 0))(
      (acc, value) => (acc._1 + value, acc._2 + 1),
      (acc1, acc2) => (acc1._1 + acc2._1, acc1._2 + acc2._2)
    )
    val avg = result._1 / result._2.toDouble
```



##### 2.2 查看RDD元素

RDD 的一些行动操作会以普通集合或者值的形式将 RDD 的部分或全部数据返回驱动器程序中

###### collect() 

功能： 返回RDD的所有元素

解释： 将整个RDD的内容返回。数据会整体放进Driver的内存中，关注下driver此时内存的大小。

场景： 如果你的程序把 RDD 筛选到一个很小的规模，并且你想在本地处理这些数据时，就可以使用它。记住，只有当你的整个数据集能在单台机器的内存中放得下时，才能使用 collect()，因此，collect() 不能用在大规模数据集上。



###### take(num) 

返回RDD的num个元素，注意不保证顺序。



###### top(num)

前num个元素。

###### takeOrdered(num)(ordering)

按照RDD中提供的顺序返回最前面的num个元素 。



###### count()

返回RDD元素的额数量

场景： 经常用户在spark-shell里面触发RDD的执行。



###### countByValue()

countByValue() :返回一个从各值到值对应的计数的映射表。



###### foreach(func)

foreach(func) 对RDD的每个元素使用特定的函数。

场景： 有时我们会对 RDD 中的所有元素应用一个行动操作，但是不把任何结果返回到驱动器程序中，这也是有用的。比如可以用JSON 格式把数据发送到一个网络服务器上，或者把数据存到数据库中。不论哪种情况，都可以使用 foreach() 行动操作来对RDD 中的每个元素进行操作，而不需要把 RDD 发回本地。





