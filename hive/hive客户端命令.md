### 客户端命令

[HiveServer2 Clients - Apache Hive - Apache Software Foundation ~ HiveServer2 客户端 - Apache Hive - Apache 软件基金会](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-OutputFormats)

##### metastore 服务

进程名 `HiveMetastore` 端口号 ： `9083`

```bash
 ${HIVE_HOME}/bin/hive --service hiveserver2
```

##### **hiveserver2**

进程名 `HiveServer2` 端口号 ： `10000`

启动命令

```bash
 ${HIVE_HOME}/bin/hive --service hiveserver2
```

##### beeline

```bash
hive 的命令行客户端
 beeline -u jdbc:hive2://ipxxx:10000 -n username
```



hive 常用的交互命令

```bash
$ bin/beeline -help

usage: hive

 -d,--define <key=value>          Variable subsitution to apply to hive

                                  commands. e.g. -d A=B or --define A=B

    --database <databasename>     Specify the database to use

 -e <quoted-query-string>         SQL from command line

 -f <filename>                    SQL from files

 -H,--help                        Print help information

    --hiveconf <property=value>   Use value for given property

    --hivevar <key=value>         Variable subsitution to apply to hive

                                  commands. e.g. --hivevar A=B

 -i <filename>                    Initialization SQL file

 -S,--silent                      Silent mode in interactive shell

 -v,--verbose                     Verbose mode (echo executed SQL to the console)****
```



1. 支持 `tab` 补全。





### Example

1 )  SQL from command line

`hive -e 'sql'`

2 )  SQL from files

`hive -f file.sql`

3）hive 输出格式

```shell
beeline  --outputformat=[table/vertical/csv/tsv/dsv/csv2/tsv2]  \
         --delimiterForDSV= DELIMITER  -- 
```

