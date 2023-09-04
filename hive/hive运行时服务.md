### Hive 运行时服务

[hive的metadata、metastore 、hiveserver2、beeline 之间的关系](https://blog.csdn.net/iKuboo/article/details/100188410)

[(Hive的Metastore服务和Hiveserver2服务的详细说明_hive的metastore启动_老胡向前冲的博客-CSDN博客](https://blog.csdn.net/weixin_48833605/article/details/111124191?spm=1001.2101.3001.6650.5&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-5-111124191-blog-100188410.235%5Ev27%5Epc_relevant_default_base1&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-5-111124191-blog-100188410.235%5Ev27%5Epc_relevant_default_base1&utm_relevant_index=6)

启动方式

1. 直接启hive服务

	通过 ./bin/hive 启动的hive服务，第一步会先启动metastore服务，然后在启动一个客户端连接到metastore。此时metastore服务端和客户端都在一台机器上，别的机器无法连接到metastore，所以也无法连接到hive。这种方式不常用，一直只用于调试环节。

2. 启动hiveserver2，然后启动beeline 连接。


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