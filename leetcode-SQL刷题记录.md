#### 逐题记录

| 题号 | 描述                                          | 题解                                                         |
| ---- | --------------------------------------------- | ------------------------------------------------------------ |
|      |                                               |                                                              |
|      |                                               |                                                              |
| 182  | 查找重复的电子邮箱                            | 简单 Group by                                                |
| 183  | 从不订购的客户                                | 子查询，（not in, not exists), left join 都可以，关键是大数据量下，是否走索引 |
| 184  | 部门工资最高的员工                            | 最高/最低问题（且有并列数据的） 是比较特殊的一类 TopN 问题，<br />既然是TopN问题，当然可以用窗口函数这样通用的方式来解决。<br />而这里的特殊是可以用一些 巧妙的思路解决 .<br />1. 先找出最大的，然后反查其他字段<br />2. 除了我自己，，没人比我大的思路找出最大的 （ <= ) |
| 185  | 部门工资前三高的所有员工                      | TOPN 问题，两种思路，一种思路是窗口函数，一种思路是，找这个工资所在的位置（比我多的和跟我一样的就3人） |
| 196  | 删除重复的电子邮箱                            | 没啥难度                                                     |
| 197  | 上升的温度                                    | 这种需要前N行，后N行的计算，一般考虑窗口函数。<br />SQL的计算模式分为三种，1. 单行，2 多行，这种是借助于聚合函数  3 单行下（不改变粒度）前/后N行 |
| 262  | 行程和用户                                    | 题意理解                                                     |
| 511  | 游戏玩法分析 I                                | 聚合函数应用                                                 |
| 511  | 游戏玩法分析 II                               | 关于引用group by 后的其他字段，统称为聚合函数引起的粒度问题，这类问题有两种方法<br />1. 反查<br />2. 窗口函数保留最小的粒度 |
| 534  | 游戏玩法分析 III                              | 累加问题，属于前N后N行的一种类型                             |
| 550  | 游戏玩法分析 IV                               | 连续问题，也属于前N行后N行问题                               |
| 569  | 员工薪水中位数                                | 这种属于 技巧问题                                            |
| 570  | 至少有5名直接下属的经理                       | 入门题                                                       |
| 571  | 给定数字的频率查询中位数                      | 思路技巧                                                     |
| 574  | 当选者                                        |                                                              |
| 577  | 员工奖金                                      |                                                              |
| 578  | 查询回答率最高的问题                          | 简单分组后最值问题                                           |
| 579  | 查询员工的累计薪水                            | 窗口函数的语法细节                                           |
| 580  | 统计各专业学生人数                            | left join 问题                                               |
| 584  | 寻找用户推荐人                                | NULL 比较                                                    |
| 585  | 2016投资                                      | 题目理解                                                     |
| 586  | 订单最多的客户                                | 简单语法                                                     |
| 595  | 大的国家                                      | 简单语法                                                     |
| 596  | 超过5名学生的课                               | 简单语法                                                     |
| 597  | 好友申请 I：总体通过率                        | 题目理解                                                     |
| 601  | 体育馆的人流量                                | 连续问题                                                     |
| 602  | 好友申请 II ：谁有最多的好友                  | 思路巧妙                                                     |
| 603  | 连续空余的座位                                | 连续问题                                                     |
| 607  | 销售员                                        | 这种属于，key 对应的多个状态，即 key -> list[] <br />根据 list 不含有（都不是）某个值为判断条件来过滤key<br />这种题目一般有两种思路<br />1. 针对 “不含有的” 这种问题的，先查出含有某个值的key，然后用原表排除这些key。<br />2. 用状态法，符合条件赋状态0，不符合为1 ，然后 分组累加聚合，累加为0的对应key保留。 |
| 608  | 树节点                                        | NULL 和 NOT in  ， in 的问题                                 |
| 610  | 判断三角形                                    |                                                              |
| 612  | 平面上的最近距离                              | 排列组合                                                     |
| 613  | 直线上的最近距离                              | 排列组合                                                     |
| 614  | 二级关注者                                    | 题意理解                                                     |
| 615  | 平均工资：部门与公司比较                      | 关于不同粒度下的汇总问题，可以使用窗口函数保留最细粒度，然后汇总。 |
| 618  | 学生地理信息报告                              | 透视表                                                       |
| 619  | 只出现一次的最大数字                          |                                                              |
| 620  | 有趣的电影                                    |                                                              |
| 626  | 换座位                                        | 思路巧妙                                                     |
| 1045 | 买下所有产品的客户                            | 简单分组                                                     |
| 1050 | 合作过至少三次的演员和导演                    |                                                              |
| 1068 | 产品销售分析 I                                |                                                              |
| 1069 | 产品销售分析 II                               |                                                              |
| 1070 | 产品销售分析 III                              | 最值问题                                                     |
| 1075 | 项目员工 I                                    |                                                              |
| 1076 | 项目员工II                                    | 最值问题                                                     |
| 1077 | 项目员工 III                                  | 每组的最值问题，也可理解成粒度问题                           |
| 1082 | 销售分析 I                                    |                                                              |
| 1083 | 销售分析 II                                   | 都不含有的问题                                               |
| 1084 | 销售分析III                                   |                                                              |
| 1097 | 游戏玩法分析 V                                | 窗口函数的汇总                                               |
| 1098 | 小众书籍                                      | 题目理解                                                     |
| 1107 | 每日新用户统计                                | group by 过滤条件                                            |
| 1112 | 每位学生的最高成绩                            | 分组最值问题                                                 |
| 1113 | 报告的记录                                    |                                                              |
| 1126 | 查询活跃业务                                  | 理解题意                                                     |
| 1127 | 用户购买平台                                  | 仅含有问题，虚拟表的问题<br />对于仅含有问题，一种状态法，一种是比较list某个值的个数和list全体的个数 |
| 1132 | 报告的记录 II                                 |                                                              |
| 1141 | 查询近30天活跃用户数                          |                                                              |
| 1142 | 过去30天的用户活动 II                         |                                                              |
| 1148 | 文章浏览 I                                    |                                                              |
| 1149 | 文章浏览 II                                   |                                                              |
| 1158 | 市场分析 I                                    | left join 语义                                               |
| 1159 | 市场分析 II                                   |                                                              |
| 1164 | 指定日期的产品价格                            | 窗口函数取最新的数据                                         |
| 1173 | 即时食物配送 I                                |                                                              |
| 1174 | 即时食物配送 II                               | 最值问题                                                     |
| 1179 | 重新格式化部门表                              | 行转列问题，这个问题 可以抽象成 key => list , 把list展平，打成多列 |
| 1193 | 每月交易 I                                    |                                                              |
| 1194 | 锦标赛优胜者                                  | 每组取最高                                                   |
| 1204 | 最后一个能进入电梯的人                        | 窗口函数累加                                                 |
| 1205 | 每月交易II                                    | 模拟fulljoin                                                 |
| 1211 | 查询结果的质量和占比                          |                                                              |
| 1212 | 查询球队积分                                  |                                                              |
| 1225 | 报告系统状态的连续日期                        | 连续问题                                                     |
| 1241 | 每个帖子的评论数                              |                                                              |
| 1251 | 平均售价                                      |                                                              |
| 1264 | 页面推荐                                      |                                                              |
|      | 向公司CEO汇报工作的所有人                     |                                                              |
| 1280 | 学生们参加各科测试的次数                      |                                                              |
| 1251 | 平均售价                                      |                                                              |
| 1264 | 页面推荐                                      |                                                              |
| 1270 | 向公司CEO汇报工作的所有人                     | 递归的用法                                                   |
| 1280 | 学生们参加各科测试的次数                      | left join                                                    |
| 1285 | 找到连续区间的开始和结束数字                  | 连续问题                                                     |
| 1294 | 不同国家的天气类型                            |                                                              |
| 1321 | 餐馆营业额变化增长                            | 累加问题                                                     |
| 1378 | 使用唯一标识码替换员工ID                      | left join                                                    |
| 1398 | 购买了产品 A 和产品 B 却没有购买产品 C 的顾客 | key => list ，根据list含有的元素过滤key                      |
| 1407 | 排名靠前的旅行者                              | left join 语义                                               |
| 1412 | 查找成绩处于中游的学生                        | key => list 所有状态的问题                                   |
| 1445 | 苹果和橘子                                    | sun 里面的case的原理                                         |
| 1454 | 活跃用户                                      | 连续问题                                                     |
| 1479 | 周内每天的销售情况                            | 粒度，行转列                                                 |
| 1532 | 最近的三笔订单                                | topN问题                                                     |
| 1549 | 每件商品的最新订单                            | 分组取最值                                                   |
| 1596 | 每位顾客最经常订购的商品                      | 分组取最值                                                   |
| 1613 | 找到遗失的ID                                  | 建立虚拟表，生成子序列                                       |
| 1633 | 各赛事的用户注册率                            |                                                              |
| 1635 | Hopper 公司查询 I                             | 累加的边界问题                                               |
| 1651 | Hopper 公司查询 III                           | 窗口函数细节                                                 |
| 1693 | 访问日期之间最大的空档期                      | 前N后N条                                                     |
| 1767 | 寻找没有被执行的任务对                        | 虚拟子表，递归生成，重点是不要直接生成1-20 序列，然后过滤    |
| 1795 | 每个产品在不同商店的价格                      | 列转行                                                       |
| 1892 | 页面推荐Ⅱ                                     | left join 和 in 、 notin 的性能问题                          |
| 2066 | 账户余额                                      | 窗口函数的累加问题                                           |
| 2118 | 建立方程                                      | case when 繁琐题                                             |
| 2142 | 每辆车的乘客人数 I                            | 前N行，后N行题目                                             |
| 2173 | 最多连胜的次数                                | 连续问题的求解                                               |
| 2175 | 世界排名的变化                                | 窗口函数，细节是 排名函数（rank）返回值是无符号的            |
| 2199 | 找到每篇文章的主题                            | mysql 字符串处理函数                                         |
| 2205 | 有资格享受折扣的用户数量                      | 时间格式处理函数                                             |
| 2228 | 7 天内两次购买的用户                          | 时间处理函数，前后N天                                        |
| 2238 | 司机成为乘客的次数                            | 去重                                                         |
|      |                                               |                                                              |



1083



简单考察语法

 

|                                   |                         |      |
| --------------------------------- | ----------------------- | ---- |
| 175 1148                          | 简单join                |      |
| 182                               | 分组计数                |      |
| 183 580  1158 1241 1280 1350      | 左连接语义              |      |
| 511                               | 分组计数                |      |
| 570                               | 分组计数                |      |
| 574                               |                         |      |
| 584                               | NULL处理（ ！= 忽略null |      |
| 586                               | 分组取最高              |      |
| 595                               | where                   |      |
| 596                               | 分组having              |      |
| 602                               | union                   |      |
| 608                               | NULL问题                |      |
| 619                               | 分组计数                |      |
| 620                               | where                   |      |
| 627                               | update                  |      |
| 1045，1050，1069                  | 分组计数                |      |
| 1075，1132                        | 分组聚合                |      |
| 1068                              | join                    |      |
| 1098，1107， 1113，1126 1141 1142 | 分组计数                |      |
| 1149  1173 1193 1211 1251 1294    | 分组计数                |      |
| 1308 1322 1327 1341               |                         |      |
| 1212                              | case                    |      |
|                                   |                         |      |



开窗函数

|                                                              |                             |
| ------------------------------------------------------------ | --------------------------- |
| 176，177，178，1070，1077，1082 1159 1164 1174 1194 1285 1355 1369 | topN                        |
| 180 550 1225                                                 | 连续问题                    |
| 184，185，512，1076 ，1112                                   | 分组后取非分组字段 + 取topN |
| 197                                                          | 跨行取数（lag,lead)         |
| 534 1204                                                     | 累加取和                    |
| 579 1321                                                     | 窗口范围（ row，range       |
| 585，1097                                                    | 跨行查询                    |
| 601                                                          | 连续且连续频率              |
| 603                                                          | 连续且列出                  |
| 1303                                                         | 不改变粒度                  |
|                                                              |                             |



理解题目逻辑

|      |                    |
| ---- | ------------------ |
| 181  |                    |
| 262  |                    |
| 607  | not in / left join |
| 1083 | 组内不存在         |
| 1084 | 组内仅存在         |



技巧题

|           |                |
| --------- | -------------- |
| 196       | 删除重复的数据 |
| 569，571  | 中位数         |
| 612， 613 | 全排列         |
| 615       | 不同粒度的汇总 |
| 626       |                |
| 1127 1336 | 构造虚拟表     |
| 1179      | 行专列         |
|           |                |
| 1270      | 递归           |



语义题



|      |      |
| ---- | ---- |
| 577  |      |
| 614  |      |
| 1205 |      |





todo 总结：



连续问题的求解

1. 









MySQL局限

差集不能直接表达，只能模拟

NULL 的 not in 问题 





笔记：



差集模拟





