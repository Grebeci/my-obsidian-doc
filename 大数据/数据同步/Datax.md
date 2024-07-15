### 项目

[GitHub - alibaba/DataX](https://github.com/alibaba/DataX)

### 介绍

[DataX/introduction.md at master · alibaba/DataX · GitHub](https://github.com/alibaba/DataX/blob/master/introduction.md)

### 概述

- 异构数据源离线同步

	异构： 支持DNMS（mySQL，Oracle，GP等) 和 文件系统（HDFS），数据仓库底层（hive） 的数据同步功能

	离线： 相对于CDC而言，Datax是快照的方式同步数据。

- 插件化

	Framework + plugin架构构建。将数据源读取和写入抽象成为Reader/Writer插件。



##### 特点：

- 单机多进程，不支持分布式。如果多台机器并发同步，一个解决办法是在多台机器上部署，然后借助调度系统或者自己实现分布式同步的逻辑。单机也会有限制，比如单机压力大，但是读写粒度容易控制。
- 支持ORC格式
- 可以定制统计信息
- 有部分校验功能
- 没有监控，需要定制



##### 配置：

查看帮助

```
python2 datax.py --help
```



查看配置模板

```
python2 datax.py -r streamreader -w streamwriter
```



### Datax可视化

[Datax3.0+DataX-Web打造分布式可视化ETL系统 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/549780333)



- [ ] 任务
	- [ ] mysql => hdfs
	- [ ] mysql => hive
	- [ ] GP , Oracle , => hive
	- [ ] hive, hdfs => MySQL
- [ ] 文档
	- [ ] reader，writer
	- [x] MysqlReader