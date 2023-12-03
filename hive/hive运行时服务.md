### Hive 运行时服务

[hive的metadata、metastore 、hiveserver2、beeline 之间的关系](https://blog.csdn.net/iKuboo/article/details/100188410)

[(Hive的Metastore服务和Hiveserver2服务的详细说明_hive的metastore启动_老胡向前冲的博客-CSDN博客](https://blog.csdn.net/weixin_48833605/article/details/111124191?spm=1001.2101.3001.6650.5&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-5-111124191-blog-100188410.235%5Ev27%5Epc_relevant_default_base1&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-5-111124191-blog-100188410.235%5Ev27%5Epc_relevant_default_base1&utm_relevant_index=6)

启动方式

1. 直接使用 `bin/hive` 命令启动 Hive CLI（命令行界面）
   启动的 Hive 客户端进程 内嵌（embedding）入一个 MetaStore 进程。实际上没有启动一个持久的、独立的 metastore 服务。相反，Hive 会在嵌入式模式下运行，这意味着 metastore 运行在同一个 JVM 进程中，并使用 Derby 数据库作为后端存储（默认设置）。
   这种嵌入式模式对于单用户、本地测试和开发很有用，但不适合生产环境。

2. 启动 `hiveserver2`，然后启动beeline 连接。


### metastore

进程名 `HiveMetastore` 端口号 ： `9083`

启动命令

```bash
${HIVE_HOME}/bin/hive --service metastore
```


客户端配置信息（hive-site.xml) `hive.metastore.uris=thrift://IPxxx:9083`

作用：隔离 mysql（元数据） 和 hive服务端（metastore 服务） 



### hiveserver2

进程名 `HiveServer2` 端口号 ： `10000`

启动命令
 ```bash
 ${HIVE_HOME}/bin/hive --service hiveserver2
```

```
metastore  ========> metadata(mysql)
    /\
    ||
    || 9083
    ||
hiveserver2
    /\
    ||
    || 10000
    ||
 Client ：beeline jdbc odbc 
```

hive-site 配置信息 ： `hive.server2.thrift.bind.host=ipxxx` `hive.server2.thrift.port=10000`


###### beeline

hive 的命令行客户端
```bash
 beeline -u jdbc:hive2://ipxxx:10000 -n username
```



如果hive-site.xml 配置了 metastore 服务 （ `hive.metastore.uris=thrift://IPxxx:9083`） ，起hiveserver2 就必须先起来metastore 服务
如果没有配置metastore服务，hiveserver2 会在内部**启动**一个metastore