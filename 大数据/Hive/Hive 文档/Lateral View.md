## Lateral View Syntax

```
Lateral View ： LATERAL VIEW udtf(expression) tableAlias AS columnAlias (',' columnAlias)*
fromClause   ： FROM baseTable (lateralView)*
```



## 说明

横向视图与用户定义的表生成函数（如 explode()）结合使用。如内置表格生成函数所述，用户自定义表格生成函数会为每条输入行生成零或多条输出行。横向视图首先将 UDTF 应用于基本表的每一行，然后将生成的输出行与输入行连接起来，形成一个具有所提供的表别名的虚拟表。



## 示例

考虑以下名为 pageAds 的基表。它有两列：pageid（页面名称）和 adid_list（页面上出现的广告数组）：

| Column name | Column type |
| :---------- | :---------- |
| pageid      | STRING      |
| adid_list   | Array<int>  |

有两行的示例表：

| pageid       | adid_list |
| :----------- | :-------- |
| front_page   | [1, 2, 3] |
| contact_page | [3, 4, 5] |



用户想计算一个广告在所有页面上出现的总次数。

可以使用带有 explode() 的横向视图，通过查询将 adid_list 转换成单独的行：

```
SELECT pageid, adid
FROM pageAds LATERAL VIEW explode(adid_list) adTable AS adid
```

输出结果将是

| pageid（字符串） | adid（int） |
| :--------------- | :---------- |
| "front_page      | 1           |
| "front_page      | 2           |
| "front_page      | 3           |
| "contact_page    | 3           |
| "contact_page"   | 4           |
| "contact_page"   | 5           |

然后，为了计算特定广告出现的次数，可以使用 count/group by：

```
SELECT adid, count(1)
FROM pageAds LATERAL VIEW explode(adid_list) adTable AS adid
GROUP BY adid;
```

| int adid | count(1) |
| -------- | -------- |
| 1        | 1        |
| 2        | 1        |
| 3        | 2        |
| 4        | 1        |
| 5        | 1        |

## 多个侧视图

一个 FROM 子句可以包含多个 LATERAL VIEW 子句。后面的侧视图可以引用侧视图左边任何表中的列。

例如，下面的查询可能是有效的：

```
SELECT * FROM exampleTable
LATERAL VIEW explode(col1) myTable1 AS myCol1
LATERAL VIEW explode(myCol1) myTable2 AS myCol2;
```

LATERAL VIEW 子句按照出现的顺序应用。例如，基表如下

| `Array<int> col1` | `Array<int> col1` |
| ----------------- | ----------------- |
| [1, 2]            | [a","b","c"］     |
| [3, 4]            | ["d", "e", "f"]   |

查询

```
SELECT myCol1, col2 FROM baseTable
LATERAL VIEW explode(col1) myTable1 AS myCol1；
```

将产生

| int mycol1 | `Array<string> col2` |
| ---------- | -------------------- |
| 1          | ["a", "b", "c"］     |
| 2          | ["a", "b", "c"]      |
| 3          | ["d", "e", "f"]      |
| 4          | ["d", "e", "f"]      |

增加一个 "矢量视图 "的查询：

```
SELECT myCol1, myCol2 FROM baseTable
LATERAL VIEW explode(col1) myTable1 AS myCol1
LATERAL VIEW explode(col2) myTable2 AS myCol2；
```

将产生

| int myCol1 | 字符串 myCol2 |
| ---------- | ------------- |
| 1          | "a"           |
| 1          | "b"           |
| 1          | "c"           |
| 2          | "a"           |
| 2          | "b"           |
| 2          | "c"           |
| 3          | "d"           |
| 3          | "e"           |
| 3          | "f"           |
| 4          | "d"           |
| 4          | "e"           |
| 4          | "f"           |

## 外侧视图

用户可以指定可选的 OUTER 关键字来生成行，即使通常情况下 LATERAL VIEW 不会生成行。当所使用的 UDTF 不生成任何行时，就会发生这种情况，当要爆炸的列为空时，爆炸很容易发生。在这种情况下，源行永远不会出现在结果中。可以使用 OUTER 来避免这种情况，这样就会在来自 UDTF 的列中生成带 NULL 值的记录。

例如，下面的查询会返回一个空结果：

```
SELEC * FROM src LATERAL VIEW explode(array()) C AS a limit 10；
```

但使用 OUTER 关键字

```
SELECT * FROM src LATERAL VIEW OUTER explode(array()) C AS a limit 10；
```

将产生

```
238 val_238 NULL
86 val_86 NULL
311 val_311 NULL
27 val_27 NULL
165 val_165 NULL
409 val_409 NULL
255 val_255 NULL
278 val_278 NULL
98 val_98 NULL
...
```

