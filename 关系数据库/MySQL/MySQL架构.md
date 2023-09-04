## MySQL的目录结构

作为系统软件，MySQL在linux在有约定的一些目录结构。

对于本地，确定方法

确定本地的MySQL数据目录

```shell
find / -name mysql
```

查询系统变量

```mysql
show variables like '系统变量名称';
```

##### 1. 数据目录

**数据目录**：MySQL数据文件的存放路径，  `/var/lib/mysql/` 

对应的系统变量：`datadir`

##### 2. 相关命令目录

/usr/bin/ : 存放的常见命令mysqladmin, mysqlbinlog, mysqldump

/usr/sbin/: root权限下执行

##### 3.配置文件目录

配置文件目录：/etc/mysql（如my.cnf） ， /usr/share/mysql-8.0（命令及配置文件



mysql 的默认数据库

- mysql : 存储系统信息

- information_schema 

	其他数据库的元数据信息

	在系统数据库 information_schema 中提供了一些以 innodb_sys 开头的表，用于表示内部系统表。

- performance_schema

	保存MySQL运行时的状态信息，用户监控 MySQL 服务的各类性能指标

- sys

	通过视图的形式把 information_schema 和 performance_schema 结合起来。





### 数据目录和文件系统的关系

这部分mysql 5.6 、 mysql5.7 ， mysql8 都有不同的策略，下面只讨论mysql8.0的默认策略。

##### InnoDB 存储引擎模式

默认使用 **独立表空间(file-per-table tablespace)** 存储数据,  每个表创建一个对应数据库目录下的 `表明.ibd` 文件。

ibd 描述了 `表的结构信息`（元数据）和数据。

##### MyISAM 存储引擎模式

默认使用 **独立表空间(file-per-table tablespace)** 存储数据,  每个表存储三个文件

```
test.frm 存储表结构 #MySQL8.0 改为了 b.xxx.sdi
test.MYD 存储数据 (MYData) 
test.MYI 存储索引 (MYIndex
```

















