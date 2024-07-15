### SQL 执行顺序

````
FROM 
Lateral view
JOIN
WHERE
GROUP BY
CUBE | ROLLUP:
HAVING
SELECT ： 表达式，窗口函数，聚合函数
DISTINCT
ORDER BY
LIMIT
````



### lateral view

 将生成的 **virtual table** 和 **base table** 做笛卡尔积操作。

```
LATERAL VIEW udtf(expression) tableAlias AS columnAlias
```

支持多个 `lateral view` 字句。结果是所有表的笛卡尔积。



### hive 解析 json

##### get_json_object

```
get_json_object(string json_string, string path)   => string
```

根据指定的 json 路径从 json 字符串中提取 json 对象，并返回提取的 json 对象的 json 字符串。如果输入的 json 字符串无效，则返回 null。

注意：json 路径只能包含 [0-9a-z_] 字符，即不能包含大写或特殊字符。此外，键*不能以数字开头*，这是由于 Hive 列名的限制。



A limited version of JSONPath is supported:

- $ : Root object
- . : Child operator
- [] : Subscript operator for array
- \* : Wildcard for []

Syntax not supported that's worth noticing:

- : Zero length string as key
- .. : Recursive descent
- @ : Current object/element
- () : Script expression
- ?() : Filter (script) expression.
- [,] : Union operator
- [start:end.step] : array slice operator



##### json_tuple

```
json_tuple(string jsonStr,string k1,...,string kn)   => string1,...,stringn
```

接受 JSON 字符串和一组 n 个键，并返回一个包含 n 个值的元组。这是 get_json_object UDF 的更高效版本，因为它只需一次调用即可获取多个键。



```json
{
  "store": {
    "fruit":[
      { "weight": 8, "type": "apple"},
      {"weight": 9, "type": "pear"}
    ],
    "bicycle": {"price": 19.95, "color": "red"}
  },
  "email": "amy@only_for_json_udf_test.net",
  "owner": "amy"
}
```



1. 提取  `email`, `owner`

```
SELECT j.email, j.owner
FROM your_table
LATERAL VIEW json_tuple(json_column, 'email', 'owner') j AS email, owner;
```

2. 提取  `email`, `owner`, `store.bicycle.price`，`store.fruit[0].weight` , `store.fruit.[weight...]`

```
SELECT 
  get_json_object(json_column, '$.email') AS email,
  get_json_object(json_column, '$.owner') AS owner,
  get_json_object(json_column, '$.store.bicycle.price') AS bicycle_price,
  get_json_object(json_column, '$.store.fruit[0].weight') AS fruit_0_weight,
  get_json_object(json_column, '$.store.fruit[*].weight') AS fruit_array_weight
FROM 
  your_table;
```

> 待验证



3. 提取  `email`, `owner`，`store.bicycle.price` （性能更高

```
select 
    t.email, t.owner, get_json_object(t.store, '$.bicycle.price') as price
from(
    SELECT j.email as email, j.owner as owner,  j.store as store
    FROM your_table
    LATERAL VIEW json_tuple(json_column, 'email', 'owner', 'store') j AS email, owner, store 
)t
```



### 日期函数

##### 转换函数
- from_unixtime(bigint unixtime[, string pattern]) => string
  
  将 `epoch`（1970年1月1日00:00:00 UTC）以来的秒数转换成当前时区的时间戳字符串，可以指定格式。默认格式：'yyyy-MM-dd HH:mm:ss'。
  
- **unix_timestamp() => bigint**  
  
  返回当前的Unix时间戳（秒）。
  
- **unix_timestamp(string date[, string pattern]) => bigint**  

  根据指定格式将日期字符串转换为Unix时间戳。

##### 当前时间函数
- **current_date() => date**  

  返回当前日期。

- **current_timestamp() => timestamp**  

  返回当前时间戳。

##### 日期部分提取函数
- **to_date(string timestamp) => date**  
  
  将时间戳字符串转换为日期对象。
  
- **year(string date) => int**  
  
  返回日期或时间戳的年份部分。
  
- **quarter(date/timestamp/string) => int**  
  
  返回年份的季度。
  
- **month(string date) => int** 
  
  返回日期或时间戳的月份部分。
  
- **day(string date) => int** 
  返回日期或时间戳的日份部分。

- **hour(string date) => int** 
  返回时间戳的小时数。

- **minute(string date) => int** 
  返回时间戳的分钟数。

- **second(string date) => int** 
  返回时间戳的秒数。

- **weekofyear(string date) => int** 
  返回日期的周数。

- **extract(field FROM source) => int** 
  从日期、时间戳或间隔中提取指定字段（如天、小时、月份等）。

##### 日期操作函数
- datediff(string enddate, string startdate) => int
  返回两个日期之间的天数。

- date_add(date/timestamp/string startdate, int days) => date
  向开始日期添加指定天数。

- date_sub(date/timestamp/string startdate, int days) => date
  从开始日期减去指定天数。

- add_months(string start_date, int num_months[, string output_date_format]) => string
  返回在开始日期之后几个月的日期。

- last_day(string date) => string
  返回给定日期月份的最后一天。

- next_day(string start_date, string day_of_week) => string
  返回在开始日期之后第一个符合指定星期几的日期。

- trunc(string date, string format) => string
  按指定单位截断日期。

- months_between(date1, date2) => double
  返回两个日期之间的月数。

- date_format(date/timestamp/string ts, string pattern) => string
  使用指定的格式将日期、时间戳或字符串转换为格式化的字符串。

##### 时区相关函数
- from_utc_timestamp({any primitive type} ts, string timezone) => timestamp
  将UTC时间戳转换为指定时区的时间戳。

- to_utc_timestamp({any primitive type} ts, string timezone) => timestamp
  将指定时区的时间戳转换为UTC时间戳。







