1. env

	三台虚拟机新建一个一般用户，配置免密登录

	配置所有节点的ip和hostname映射

2. 解压hadoop，配置环境变量

3. 规划集群节点 NameNode SercondNameNode ResourceManager JobHistoryServer

4. 配置文件，同步到各个节点

	core-site.xml hdfs-site.xml mapped.xml yarn-site.xml work

5. 初始化namenode

	```bash
	 hdfs namenode --format
	```

6. 群起集群

	```bash
	start-dfs.sh/stop-dfs.sh
	
	# yarn 在RM 节点上运行
	start-yarn.sh/stop-yarn.sh
	```


配置文件

core-site.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>

<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<configuration>

    <!-- 指定NameNode的地址 -->

    <property>

        <name>fs.defaultFS</name>

        <value>hdfs://NN_IP:8020</value>

    </property>

    <!-- 指定hadoop数据的存储目录 -->

    <property>

        <name>hadoop.tmp.dir</name>
       
        <value>${HADOOP_HOME}/data</value>

    </property>

    <!-- 配置HDFS网页登录使用的静态用户为atguigu -->

    <property>

        <name>hadoop.http.staticuser.user</name>

        <value>username</value>

    </property>

</configuration>
```

配置hdfs-site.xml

```xml


文件内容如下：

<?xml version="1.0" encoding="UTF-8"?>

<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<configuration>

   <!-- nn web端访问地址-->

   <property>

        <name>dfs.namenode.http-address</name>

        <value>hadoop102:9870</value>

    </property>

   <!-- 2nn web端访问地址-->

    <property>

        <name>dfs.namenode.secondary.http-address</name>

        <value>hadoop104:9868</value>

    </property>

</configuration>
```


配置yarn-site.xml

```xml

<?xml version="1.0" encoding="UTF-8"?>

<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<configuration>

    <!-- 指定MR走shuffle -->

    <property>

        <name>yarn.nodemanager.aux-services</name>

        <value>mapreduce_shuffle</value>

    </property>

    <!-- 指定ResourceManager的地址-->

    <property>

        <name>yarn.resourcemanager.hostname</name>

        <value>RM_IP</value>

    </property>

    <!-- 环境变量的继承 -->

    <property>

        <name>yarn.nodemanager.env-whitelist</name>

        <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>

    </property>

</configuration>
```

配置mapred-site.xml

```xml


<?xml version="1.0" encoding="UTF-8"?>

<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<configuration>

   <!-- 指定MapReduce程序运行在Yarn上 -->

    <property>

        <name>mapreduce.framework.name</name>

        <value>yarn</value>

    </property>

</configuration>
```



### 阿里云安装

[[阿里云集群初始化]]







