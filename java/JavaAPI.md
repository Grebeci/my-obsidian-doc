### 日期时间

这个主题注意两个核心问题，java8 前后 对日期时间怎么设计的？ JDBC（SQL）的日期时间和Java的日期时间的对应关系？

java8 之后 统一用 `java.time` 的API。

核心问题：

Q: java8之前和之后日期时间设计？

**java8 之前**

`java.util.Date` 没有时区支持，由 `java.util.Calendar `和 `java.util.TimeZone` 类 提供时区支持。

JDBC 支持 ： `java.sql.Date` `java.sql.Timestamp` 继承自 `java.util.Date` , 以便和java的交互

java8**之后**

新包 `java.time`

- Local XXX  − 根据zoneId 来定义的时间。
- Zoned (时区) − 通过制定的时区处理日期时间。
- Instance 表示UTC时间轴的一点 ，是时刻的抽象。

JDBC的支持，`java.sql.Date` 提供 toXXX 转到 java.time 的LocalXXX， `java.time` 提供 toXXX 转换 为`java.sql` 的时间



Q: java.time API 和JDBC的怎么交互，JDBC提供了哪些支持?

参考：

[Should I use java.util.Date or switch to java.time.LocalDate - Stack Overflow](https://stackoverflow.com/questions/28730136/should-i-use-java-util-date-or-switch-to-java-time-localdate)





