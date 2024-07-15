发现new bing 在中文和英文语境下的表现明显不同
![[spark-qa-dataframe-en.png]]

看 《spark快速大数据分析第二版》  时候看到一段话

> Dataset 主要区分两种特性：有类型和无类型 。 Scala 中的每个 Dataset 都有一个无类型的版本——DataFrame，后者是 由 Row 对象组成的 Dataset。

按照它的意思，DataFrame可以看作 `DataSet[Row]` ，那说明DataFrame 的类型是Row，为什么没有类型呢？求助 bing


![[Pasted image 20230418171605.png]]

## 英文下

##### 他开始搜索，然后生成答案

```
正在搜索 spark dataframe untyped
```

他搜到了三个链接：

[Dataframes and Datasets in Spark - Stack Overflow](https://stackoverflow.com/questions/54714447/dataframes-and-datasets-in-spark)

[Spark Datasets - strong typing - Stack Overflow](https://stackoverflow.com/questions/40506940/spark-datasets-strong-typing)

https://blog.knoldus.com/spark-type-safety-in-dataset-vs-dataframe/

其中，第二个链接提到了一个问题：

>If, _Dataframe_ is actually _Dataset[Row]_ why is _Dataframe_ called untyped? Isnt the type here supposed to be _Row_ [defined [here](https://spark.apache.org/docs/2.1.0/api/java/org/apache/spark/sql/Row.html)]?

基本和我问的差不多，new bing 准确归纳了下面的答案，从编译时和运行时对类型的处理的角度来解释问题。

###### 追问 类型参数化的含义

他已经不用搜索了， 准确给出了在这个语境下的含义，甚至结合了例子进行解答

###### 回到问题，解释row类型不能进行类型参数化的原因

回答了问题后，甚至联系第一个问题，说明 **row类型不能被编译器类型参数化** 对dataframe的影响。

至此，已经回答的很清晰了。


## 中文下

![[spark-qa-dataframe-zh.png]]


![[spark-qa-dataframe-zh.png]]
##### 他开始搜索，

```
spark dataframe 无类型row 类型
spark dataframe 无类型

```

得到7个链接

(1) spark进阶（五）：DataFrame和DataSet使用 - CSDN博客. https://blog.csdn.net/tonydz0523/article/details/120635568 
(2) Spark之Row_spark中的row_SunnyRivers的博客-CSDN博客. https://blog.csdn.net/Android_xue/article/details/102825672 
(3) Spark DF:关于Row中的数值获取问题_冽夫的博客-CSDN博客. https://blog.csdn.net/helloworld0906/article/details/108966919 
(4) 【spark源码系列】pyspark.sql.Row介绍和使用示例 - CSDN博客. https://blog.csdn.net/helloxiaozhe/article/details/103443028
(5) Spark SQL, DataFrame 和 Dataset 编程指南 — Spark 2.2.x 中文文档 2.2.1 文档. http://spark-reference-doc-cn.readthedocs.io/zh_CN/latest/programming-guide/sql-guide.html 
(6) Spark学习之Dataset (DataFrame) 的基础操作 - xppp11 - 博客园. https://www.cnblogs.com/xp-thebest/p/14300479.html 
(7) Spark Dataset (DataFrame) 常见操作 - CSDN博客. https://blog.csdn.net/justlpf/article/details/113102628 

链接 (1), (4), (6),(7) 都是使用API的 **练习记录** ， (2) 提到了自己对Row的理解，泛泛而谈而已。
只有 **(5)**  是对spark官方文档的翻译，里面有对dataframe和dataset 概念的解释，不幸的是，这个get-starting 性质的，没有深入理解


因此他的回答先解释了dataframe 概念，然后解析了是相对于Dataset使用强类型的原因，这也是官方文档提到的。

##### 尝试提示它从编译和运行的角度回答

bing 的回答已经靠谱了，但是还是重复SQL优化器相关的答案，虽然这和问题相关性不大。。。

##### 
注意到第一个问题它搜索的是 `spark dataframe 无类型` ， 看来 `无类型` 这个prompts 或者 keyword 已经不行了，用 untype 的prompts 来触发它。

然后它回答的已经靠谱了，提到了 dataframe无法确定每一列的类型（和类型参数化意思差不多）

###### 继续追问 类型参数化的问题

虽然bing 解释清楚了，但是并没有像英文结合例子来解释

##### 结合Row类型来解释类型参数化对dataframe影响





## 结论

暂时认为new bing 是一个根据搜索引擎搜索的内容，然后自动归纳的工具，他的质量取决于

- 第一次搜索的keyword 是否能准确的命中命题，这次搜索为后续的回答提供一个 `上下文语境` ，这个基本决定了后续回答的质量

- keyword 和 prompts 非常重要，基本决定了话题的方向和答案的准确程度。

- 中文搜索受语料不足的影响。csdn都是转载和练习的记录，搜索到博客园内容也很少。

- stackOverFlow 有完善的问题链接，new bing 能很好的把他们归纳在一起进行解释。

