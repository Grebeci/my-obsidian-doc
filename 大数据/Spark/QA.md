#### Q1:

为什么说 DataFrame 是无类型的DataSet？

#### N:

说 DataFrame DataSet[Row] 是无类型的，是因为Row类型是一个容器（集合式），没有办法约束元素的类型, 所以他无法在**编译时**提供更多的类型信息进行类型检查（无法在编译时类型参数化）。这意味着spark 只能在**运行时**通过指定的 schema 来检查 DataFrama 中的类型。

类型参数化是指给class或者method一个类型，比如在java，可以创建一个泛型类`List<T>` T 是一个类型参数 ，可以在创建类实例时指定。它允许你创建一个`list<String>` 或 `List<Interger>` 的列表，其中列表的元素类型是编译时已知的。

而 Row 类型无法在编译时参数化是因为它是一个通用容器。可以保存任何类型和列的数据。因此dataframe 的schema 能在运行时定义和动态修改，编译器在编译时无法知道 在Row里面元素确定的类型。

参考 [Spark DataFrame is Untyped vs DataFrame has schema? - Stack Overflow](https://stackoverflow.com/questions/52288412/spark-dataframe-is-untyped-vs-dataframe-has-schema?newreg=fac1332f70454345ba16bafbf59d7c10)

[Dataframes and Datasets in Spark - Stack Overflow](https://stackoverflow.com/questions/54714447/dataframes-and-datasets-in-spark)

[Spark Datasets - strong typing - Stack Overflow](https://stackoverflow.com/questions/40506940/spark-datasets-strong-typing)


