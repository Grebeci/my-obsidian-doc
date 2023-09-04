调度工具

QuickStart

[官方录制：手把手教你如何《快速上手 Apache DolphinScheduler 教程》来啦_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1d64y1s7eZ/?vd_source=b820ad274d9cfbd9b19967441f219fa7)



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





核心概念：



租户： 对应linux用户，DS使用这个身份登录到目标服务器执行任务。

> 如果使用hdfs文件系统，创建的租户必须也有hdfs的访问权限



用户：DolphinScheduler登录账户，使用DS的人。



项目





