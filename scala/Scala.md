### Getting Start

- IntelliJ for Scala [Getting Started with Scala in IntelliJ | Scala Documentation (scala-lang.org)](https://docs.scala-lang.org/getting-started/intellij-track/getting-started-with-scala-in-intellij.html)

	- REPL

		[The Scala REPL | Scala Book | Scala Documentation (scala-lang.org)](https://docs.scala-lang.org/overviews/scala-book/scala-repl.html#inner-main)

	- Scala worksheet 

	- 在线REPL [* Scastie (scala-lang.org)](https://scastie.scala-lang.org/)

- ScalaDoc

	- 如何分别找到 Object , Trait 的文档



#### 基本语法

这个是 快学Scala的 前几章

[Introduction | Scala Book | Scala Documentation (scala-lang.org)](https://docs.scala-lang.org/overviews/scala-book/introduction.html#)

#### OO的支持

###### class

- 构造器
	- 用 var/val 在 `Scala` 实现了 **读写权限** 的控制,对应 java 的 getter setter
	- 在scala中，`private` 是只允许在本类上访问，把getter,setter 也给private了，而java的private 主要配合 getter,setter实现访问控制的。
	- 主构造器，辅构造器。主构造器类似函数参数的语法，定义在类上。
- object：
	- 构造单例对象，OO的范式，调用一个代码块必须构造一个对象，而大多数情况下整个程序只持有一个对象就行了。
- package
	- 
	- 包对象：包 只能包含 类，对象，特质，不能包含函数或者变量的定义。包对象就是克服这个局限的。
	- import：本质是把 特定的对象，函数，变量带入对应的作用域。
		- 随时引入：比如在函数内import，import 带入的名称限定在函数内。
		- 重命名和隐藏方法：选取器(selector) 引入和重命名特定的成员。隐藏特定成员,避免二义性。
	- java的 package 语法定义规定该文件要打包的位置，并和源文件目录对应。scala 的 package不和目录源文件目录绑定，嵌套标记法和文件顶部标记法会影响作用域。

###### 继承



#### 操作符

Scala operator 本质上是一个函数名，所以可以实现C++类似的 ”<操作符重载>“ 的效果。

由于不是语言内置。所以

- 运算符优先级，依赖于约定：除赋值操作符外，优先级由操作符的首字符决定。
- 结合性：依赖于约定： 除赋值操作符外，以 冒号（:）结尾的任何方法都是右结合的，而以其他字符结尾的都是左结合的。

在操作符实现上

- infix  ： 
- prefix : 被转换为 unary_operator 的方法调用
- *postfix*： 不推荐



#### 函数式

函数作为值

- 类，对象 def定义的是方法，不是函数
- 类型标注，或者用 _ 引用

支持高阶函数，分为两类：函数参数是函数、 返回值为函数。注意他们的`类型标注`  

- 类型标注参考 [Types | Scala 文档 (scala-lang.org)](https://docs.scala-lang.org/style/types.html)

匿名函数类似于lambda 表达式，在 类型推断的辅助下，传递函数 可以非常简化。

支持闭包：

控制抽象：本质上是call-by-name ，其中，快学Scala ，关于必须要



#### 集合库






语法糖

apply,update,unapply





#### 隐式转换

Q: 怎么查询当前的语句是否使用了隐式转换，使用了哪些隐式转换? 

IDE支持： 

```
Show implicits hints    :  Ctrl + Alt + Shift + =   : 把隐式调用的方法显示出来，一般是阅读代码使用
Expand implicits hints  :  Ctrl + Alt + Shift + -   ：关闭
Show implicits arguments : Ctrl + Shift + P         ：圈住这个函数，他会提示需要那些隐式参数
Show implicits arguments :  Ctrl + Shift + Q        : 这个是提示当前作用域下有哪些隐式转换， 注意是当前作用域，如果隐式转换没有导入，查不到的
```











ScalaDoc

StringDoc



设计哲学（tips)

- 丰富的API，让所有操作都语义化
- 几乎构造出来的语法结构都有值 - 快学Scala的 Chapter2 部分
- 



Java的异常？

- 异常在Java 是什么作用，为什么必须要对受检异常进行catch，这样设计的初衷是什么
- 如果在一个web的接口服务，这个接口服务就是拿到参数去查数据库，那数据库连不上了，程序该退出吗 ?
	- web 里面异常都是怎么处理的

##### String

string interpolation

[String Interpolation | Scala 3 — Book | Scala Documentation (scala-lang.org)](https://docs.scala-lang.org/scala3/book/string-interpolation.html)



OO范式的演变

[面向对象编程（oop）从诞生到现在理念上经历了几次怎样大的变迁和转化？ - 知乎 (zhihu.com)](https://www.zhihu.com/question/34018003)



### Key-Question

Q: call-by-name ，Call-by-value

Scala 两种都支持。

call-by-value : 调用函数之前，先计算表达式的值，然后将这个值传递给函数

call-by-name : 调用函数时，不是将参数表达式的值传递给函数，而是将参数表达式本身传递给函数。在函数内部，每次访问该参数时都会重新计算表达式的值。

参考： [Scala | Functions Call-by-Name - GeeksforGeeks](https://www.geeksforgeeks.org/scala-functions-call-by-name/)



### 练习题

- [Scala Exercises](https://www.scala-exercises.org/)是一个开源项目，旨在帮助您学习基于Scala编程语言的各种技术。该网站提供了许多不同类型的练习题，涵盖了从基础到高级的各种知识点。
- [w3resource](https://www.w3resource.com/scala-exercises/index.php)网站上也有一些Scala练习题，这些练习题旨在帮助您通过解决从基础到更复杂的练习题来练习Scala编程语言概念。每个练习题都提供了一个示例解决方案。
- [Exercism](https://exercism.org/tracks/scala)上也有一个Scala练习题库，其中包含了92道练习题，可以帮助您写出更好的代码。您可以在这里发现新的练习题，并沉浸在学习新概念和改进您当前编写方式的过程中。



###### Spark

- [GitHub](https://github.com/nivdul/spark-in-practice-scala)上有一个名为`spark-in-practice-scala`的项目，它提供了一些使用Spark核心API、Spark Streaming API和DataFrame进行数据处理的练习题。这些练习题既有Java版本，也有Scala版本。您只需克隆该项目即可开始练习。

PySpark

- [Machine Learning Plus](https://www.machinelearningplus.com/pyspark/pyspark-exercises-101-pyspark-exercises-for-data-analysis/)网站上提供了101道PySpark练习题，旨在帮助您锻炼逻辑思维能力并掌握使用Python进行数据分析的技能。这些练习题难度分为三个等级，L1最简单，L3最困难。
- [Databricks](https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/6244269837918943/3546103630347710/4066658260255490/latest.html)网站上也提供了一些Spark DF、SQL和ML练习题，可以帮助您练习使用Spark进行数据处理和机器学习。
- [Kaggle](https://www.kaggle.com/code/gilbertly/practice-pyspark)网站上也有一些PySpark练习题，可以帮助您练习使用PySpark进行数据分析。

















## 集合库API- 背诵

整体继承关系

![image-20230811152211250](https://images-note-wang.oss-cn-beijing.aliyuncs.com/note/java/image-20230811152211250.png)

- Seq： 有序表的抽象
- Set： 无序列表
- Map： 键值对偶



统一的特质Iterator，意味着统一的遍历方式

```
val iterator = Iterable(1,2,3).iterator  // 某种Iterable的迭代器

while (iterator.hasNext) {
  iterator.next()  // 执行某种操作
}
```

统一创建原则 ： 每个Scala集合都实现伴生对象和apply 方法

集合间的转换使用 to[C] 的泛型方法

统一返回类型原则 ： 例如，Iterable 类中的 map 方法会返回另一个 Iterable 作为其结果。但这种结果类型会在子类中被重写。例如，在 List 上调用 map 会再次返回 List，在 Set 上调用 map 会再次返回 Set，依此类推。



Scala Doc

[Mutable and Immutable Collections | Collections | Scala Documentation (scala-lang.org)](https://docs.scala-lang.org/overviews/collections-2.13/overview.html)

下图显示了 scala.collection 包中的所有集合。这些都是高级抽象类或特质，一般都有可变和不可变的实现。



![General collection hierarchy](https://docs.scala-lang.org/resources/images/tour/collections-diagram-213.svg)





### 1. 可变和不可变集合

集合层次结构中的大多数类都有三种变体：根类、可变类和不可变类。唯一的例外是 `Buffer` 特质，它只作为可变集合存在。

`scala.collection`, `scala.collection.immutable`, and `scala.collection.mutable` 三套集合框架

- `scala.collection` 可以是可变的，也可使不可变的，一般是  `immutable` ，`mutable` 包下的超类。

Scala优先采用不可变集合， `scala.collection` 包伴生对象产出的是不可变集合。通用的约定是，如果构建可变集合，加上前缀引用.

```scala
import scala.collection.mutable;

val set = mutable.Set(1, 2, 3)
```

#### 继承结构

**`scala.collection.immutable`**



![](https://docs.scala-lang.org/resources/images/tour/collections-immutable-diagram-213.svg)



`scala.collection.mutable`



![mutable](https://docs.scala-lang.org/resources/images/tour/collections-mutable-diagram-213.svg)



### 2. Seq

不可变的序列

![image-20230811165639245](https://images-note-wang.oss-cn-beijing.aliyuncs.com/note/java/image-20230811165639245.png)



IndexedSeq 是支持 快速的随机访问的序列

注意： 这和 Java的 List 两个实现 ArrayList LinkedList 不同，List下面虽然都支持随机，但是时间代价不同。



- Vector 是 ArrayBuffer 的不可变版本。一个带下表的序列，其支持快速的随机访问。以树的结构存储
- Range 表示一个整数序列。在实现上只存储 start，end，step



可变序列

![image-20230811173421830](https://images-note-wang.oss-cn-beijing.aliyuncs.com/note/java/image-20230811173421830.png)



- stack，Queue，PriorityQueue 是标准的数据结构，用来实现特定的算法。

   

### 3. List

列表要么是 Nil （空表），要么是 head元素 + tail列表。 

`::` 操作符从给定的头和尾创建一个新的列表 例如

```
9 :: List(4, 2) 
9 :: 4 :: 2 :: Nil // 有结合的， 等价于 9 :: (4 :: (2::Nil ))  
```

### 4.Set

不重复的集合

默认实现的Set 不保证插入顺序。LinkedHashSet 是维护插入顺序的一个实现。

要求常数级别的 Contains ， 常用操作时 union intersect，diff



### 5.API

用于 添加和移除元素的操作符

![image-20230811191740546](https://images-note-wang.oss-cn-beijing.aliyuncs.com/note/java/image-20230811191740546.png)



方法



![image-20230811213853297](https://images-note-wang.oss-cn-beijing.aliyuncs.com/note/java/image-20230811213853297.png)



![image-20230811214116230](https://images-note-wang.oss-cn-beijing.aliyuncs.com/note/java/image-20230811214116230.png)



针对Seq



![image-20230811214135781](https://images-note-wang.oss-cn-beijing.aliyuncs.com/note/java/image-20230811214135781.png)

![image-20230811214150412](https://images-note-wang.oss-cn-beijing.aliyuncs.com/note/java/image-20230811214150412.png)
