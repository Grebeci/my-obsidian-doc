##### 以下需求：

> ##### 背景
>
> 有 SparkSQL 数据处理若干条，需要梳理这些分析SQL使用了哪些字段，这些字段属于哪些表。

这是类似实现血缘追溯的功能，如果人力梳理当然可以，但是比较费时费力，需要自动化这个过程。

##### 存在的问题

- 如果这个 SQL 对数据源字段的引用都是 **完全限定列名** ，例如 `[db.]tb_name.field` ，字段和数据源的关系就清楚了。那么可以用 SQLFlow 提供的功能去解析。

- 不完全限定列名，例如SQL

	```sql
	SELECT name, price FROM user, order WHERE user.id = order.uid
	```

	无法知道 `name` ,`price` 来源自哪张表，只能查表结构来确定。

	- 这里有一个技巧，如果可以导出DML，可以直接提供给 SQLFlow ，这样就能判断出来了。



既然 SQL是Spark执行的，就可以借助Spark的 Parser 的功能，拿到解析后的逻辑树，逻辑树有属性字段，这个属性包含了表名、字段名、数据类型、别名等信息。

##### 最终需求：

借助Spark Parser的功能，拿到 逻辑树（Logical Plan) ,  拿到对应关系。



##### Demo

```sql
    
    val spark = SparkSession.builder.appName("Spark SQL Field Resolution").master("local[*]").getOrCreate()


    // 创建两个测试表user和order
    val user = spark.createDataFrame(Seq((1, "Alice", 20), (2, "Bob", 25), (3, "Charlie", 30))).toDF("id", "name", "age")
    val order = spark.createDataFrame(Seq((1, 1, 100), (2, 2, 200), (3, 3, 300))).toDF("id", "uid", "price")
    user.createOrReplaceTempView("user")
    order.createOrReplaceTempView("order")

    // 用户提交的SQL语句
    val query = "SELECT name, price FROM user, order WHERE user.id = order.uid"

    // 将SQL语句转换为未分析未绑定的逻辑树
    val unresolved_plan = spark.sessionState.sqlParser.parsePlan(query)

    // 对逻辑树进行分析和绑定，得到分析绑定后的逻辑树
    val resolved_plan = spark.sessionState.analyzer.execute(unresolved_plan)

    // 在逻辑树中查找Project节点，获取它的输出属性列表
    val project = resolved_plan.find(_.isInstanceOf[Project]).get.asInstanceOf[Project]
    val output = project.output

    // 遍历输出属性列表，提取每个属性的表名、字段名、别名等信息
    val fields = output.map {
      case attr: Attribute =>
        val table_name = attr.qualifier.head // 表名是属性的限定符列表中的第一个元素
        val field_name = attr.name // 字段名是属性的名称
        val field_type = attr.dataType // 数据类型是属性的数据类型
        import org.apache.spark.sql.types.Metadata
        val field_alias = if (attr.metadata.contains("alias")) attr.metadata.getString("alias")
        else ""
        (table_name, field_name, field_type, field_alias)
    }


    // 根据这些信息，生成一个展示结果字段来源的表格或者文本
    val result = fields.map {
      case (table_name, field_name, field_type, field_alias) =>
        s"结果字段${if (field_alias.isEmpty) field_name else field_alias}来自${table_name}表的${field_name}字段，数据类型为${field_type}"

    }.mkString("\n")

    println(result)
```







以后要研究的问题和做的工作



- 把这个功能函数化，集成到基于Spark的血缘项目上。
- 增加到sparkPlus上

学习内容



spark Parser 所做的工作，每一步都生成了哪些东西，

- 分析（Analysis
- 绑定（Binding

资料

- 涉及到sparkSQL parser的 论文，文档，演讲

