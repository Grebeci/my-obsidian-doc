调度工具

QuickStart



基本功能

定时调度

日志功能

回溯功能

重新执行（执行的是历史脚本）





任务依赖

- 最近一次
- 周期内所有
- 文件依赖（done文件
- 分区依赖（hive）
- 不同频率的任务依赖 （五分钟 => 一小时 => t+1



任务执行配置

- 自动重试（次数，间隔
- 超时策略（报警、失败

告警配置

- 重试/超时报警
- 报警组
- 报警个人















--------------------

### 资料

使用手册：

[快速上手 - 《Apache DolphinScheduler v3.0.1 使用手册》 - 书栈网 · BookStack](https://www.bookstack.cn/read/dolphinscheduler-3.0.1-zh/快速上手.md?wd=python快速上手)

QuickStart 视频：

[官方录制：手把手教你如何《快速上手 Apache DolphinScheduler 教程》来啦_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1d64y1s7eZ/?vd_source=b820ad274d9cfbd9b19967441f219fa7)



[思否 ： Apache DolphinScheduler 3.1.8 保姆级教程【安装、介绍、项目运用、邮箱预警设置】轻松拿捏！](https://segmentfault.com/a/1190000044558917)



### 名词解释

项目： 业务的划分，组织和管理 任务 和 工作流
工作流 ： 有序的任务组合。
    工作流定义： 工作流定义是指定如何组织和执行一系列任务或作业的规则和配置。
    工作流实例： 触发的一次工作流执行；
任务： 具体的执行的命令，Shell脚本，SQL脚本，MR，Spark任务。
租户： 对应linux用户，DS使用这个身份登录到目标服务器执行任务。

> 如果使用hdfs文件系统，创建的租户必须也有hdfs的访问权限

用户：DolphinScheduler登录账户，使用DS的人。

补数：补历史数据，支持**区间并行和串行**两种补数方式。（重刷历史数据）










### 系统管理









### 问题



补数与定时配置的关系：（来自官方文档

    1. 未配置定时：当没有定时配置时默认会根据所选时间范围进行每天一次的补数，比如该工作流调度日期为7月 7号到7月10号，未配置定时，流程实例为：
    
    2. 已配置定时：如果有定时配置则会根据所选的时间范围结合定时配置进行补数，比如该工作流调度日期为7月 7号到7月10号，配置了定时（每日凌晨5点运行），流程实例为：

这个貌似只是调度时间不同，待验证。

定时策略的环境名称



### 具体案例

如何查看一个工作流定义。

在 工作流定义 => 工作流实例 => 点开































### 调度概述









用户数，打点数



