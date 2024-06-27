# 1.Apache Kafka 是什么？

Apach Kafka 是一款 **分布式流处理框架**，用于实时构建流处理应用。在大数据上，用于构建 **实时数据管道** 和 **流式应用程序** 的工具；在后端上，作为 **消息队列**中间件使用。

> 消息队列作用：缓存，异步，削峰，解耦



#### Kafka 配置

kafka 端口 ：9092

目录结构和关键的配置文件

```
`-- kafka
    |-- bin
    |   |-- zookeeper-server-start.sh
    |   |-- zookeeper-server-stop.sh
    |   `-- zookeeper-shell.sh
    |-- config
    |   |-- connect-console-sink.properties
    |   |-- connect-console-source.properties
    |   |-- connect-distributed.properties
    |   |-- connect-file-sink.properties
    |   |-- connect-file-source.properties
    |   |-- connect-log4j.properties
    |   |-- connect-mirror-maker.properties
    |   |-- connect-standalone.properties
    |   |-- consumer.properties
    |   |-- kraft
    |   |-- log4j.properties
    |   |-- producer.properties
    |   |-- server.properties
    |   |-- tools-log4j.properties
    |   |-- trogdor.conf
    |   `-- zookeeper.properties  # zookeeper 配置文件
    |-- libs
    |   |-- activation-1.1.1.jar
    |   |-- aopalliance-repackaged-2.6.1.jar
    |   |-- argparse4j-0.7.0.jar
    |   |-- audience-annotations-0.12.0.jar
    |   |-- caffeine-2.9.3.jar
    |   |-- checker-qual-3.19.0.jar
    |   |-- commons-beanutils-1.9.4.jar
    |   |-- commons-cli-1.4.jar
    |   |-- commons-collections-3.2.2.jar
    |   |-- commons-digester-2.1.jar
    |   |-- commons-io-2.11.0.jar
    |   |-- commons-lang3-3.8.1.jar
    |   |-- commons-logging-1.2.jar
    |   |-- commons-validator-1.7.jar
    |   |-- connect-api-3.6.1.jar
    |   |-- connect-basic-auth-extension-3.6.1.jar
    |   |-- connect-file-3.6.1.jar
    |   |-- connect-json-3.6.1.jar
    |   |-- connect-mirror-3.6.1.jar
    |   |-- connect-mirror-client-3.6.1.jar
    |   |-- connect-runtime-3.6.1.jar
    |   |-- connect-transforms-3.6.1.jar
    |   |-- error_prone_annotations-2.10.0.jar
    |   |-- hk2-api-2.6.1.jar
    |   |-- hk2-locator-2.6.1.jar
    |   |-- hk2-utils-2.6.1.jar
    |   |-- jackson-annotations-2.13.5.jar
    |   |-- jackson-core-2.13.5.jar
    |   |-- jackson-databind-2.13.5.jar
    |   |-- jackson-dataformat-csv-2.13.5.jar
    |   |-- jackson-datatype-jdk8-2.13.5.jar
    |   |-- jackson-jaxrs-base-2.13.5.jar
    |   |-- jackson-jaxrs-json-provider-2.13.5.jar
    |   |-- jackson-module-jaxb-annotations-2.13.5.jar
    |   |-- jackson-module-scala_2.12-2.13.5.jar
    |   |-- jakarta.activation-api-1.2.2.jar
    |   |-- jakarta.annotation-api-1.3.5.jar
    |   |-- jakarta.inject-2.6.1.jar
    |   |-- jakarta.validation-api-2.0.2.jar
    |   |-- jakarta.ws.rs-api-2.1.6.jar
    |   |-- jakarta.xml.bind-api-2.3.3.jar
    |   |-- javassist-3.29.2-GA.jar
    |   |-- javax.activation-api-1.2.0.jar
    |   |-- javax.annotation-api-1.3.2.jar
    |   |-- javax.servlet-api-3.1.0.jar
    |   |-- javax.ws.rs-api-2.1.1.jar
    |   |-- jaxb-api-2.3.1.jar
    |   |-- jersey-client-2.39.1.jar
    |   |-- jersey-common-2.39.1.jar
    |   |-- jersey-container-servlet-2.39.1.jar
    |   |-- jersey-container-servlet-core-2.39.1.jar
    |   |-- jersey-hk2-2.39.1.jar
    |   |-- jersey-server-2.39.1.jar
    |   |-- jetty-client-9.4.52.v20230823.jar
    |   |-- jetty-continuation-9.4.52.v20230823.jar
    |   |-- jetty-http-9.4.52.v20230823.jar
    |   |-- jetty-io-9.4.52.v20230823.jar
    |   |-- jetty-security-9.4.52.v20230823.jar
    |   |-- jetty-server-9.4.52.v20230823.jar
    |   |-- jetty-servlet-9.4.52.v20230823.jar
    |   |-- jetty-servlets-9.4.52.v20230823.jar
    |   |-- jetty-util-9.4.52.v20230823.jar
    |   |-- jetty-util-ajax-9.4.52.v20230823.jar
    |   |-- jline-3.22.0.jar
    |   |-- jopt-simple-5.0.4.jar
    |   |-- jose4j-0.9.3.jar
    |   |-- jsr305-3.0.2.jar
    |   |-- kafka_2.12-3.6.1.jar
    |   |-- kafka-clients-3.6.1.jar
    |   |-- kafka-group-coordinator-3.6.1.jar
    |   |-- kafka-log4j-appender-3.6.1.jar
    |   |-- kafka-metadata-3.6.1.jar
    |   |-- kafka-raft-3.6.1.jar
    |   |-- kafka-server-common-3.6.1.jar
    |   |-- kafka-shell-3.6.1.jar
    |   |-- kafka-storage-3.6.1.jar
    |   |-- kafka-storage-api-3.6.1.jar
    |   |-- kafka-streams-3.6.1.jar
    |   |-- kafka-streams-examples-3.6.1.jar
    |   |-- kafka-streams-scala_2.12-3.6.1.jar
    |   |-- kafka-streams-test-utils-3.6.1.jar
    |   |-- kafka-tools-3.6.1.jar
    |   |-- kafka-tools-api-3.6.1.jar
    |   |-- lz4-java-1.8.0.jar
    |   |-- maven-artifact-3.8.8.jar
    |   |-- metrics-core-2.2.0.jar
    |   |-- metrics-core-4.1.12.1.jar
    |   |-- netty-buffer-4.1.100.Final.jar
    |   |-- netty-codec-4.1.100.Final.jar
    |   |-- netty-common-4.1.100.Final.jar
    |   |-- netty-handler-4.1.100.Final.jar
    |   |-- netty-resolver-4.1.100.Final.jar
    |   |-- netty-transport-4.1.100.Final.jar
    |   |-- netty-transport-classes-epoll-4.1.100.Final.jar
    |   |-- netty-transport-native-epoll-4.1.100.Final.jar
    |   |-- netty-transport-native-unix-common-4.1.100.Final.jar
    |   |-- osgi-resource-locator-1.0.3.jar
    |   |-- paranamer-2.8.jar
    |   |-- pcollections-4.0.1.jar
    |   |-- plexus-utils-3.3.1.jar
    |   |-- reflections-0.10.2.jar
    |   |-- reload4j-1.2.25.jar
    |   |-- rocksdbjni-7.9.2.jar
    |   |-- scala-collection-compat_2.12-2.10.0.jar
    |   |-- scala-java8-compat_2.12-1.0.2.jar
    |   |-- scala-library-2.12.18.jar
    |   |-- scala-logging_2.12-3.9.4.jar
    |   |-- scala-reflect-2.12.18.jar
    |   |-- slf4j-api-1.7.36.jar
    |   |-- slf4j-reload4j-1.7.36.jar
    |   |-- snappy-java-1.1.10.5.jar
    |   |-- swagger-annotations-2.2.8.jar
    |   |-- trogdor-3.6.1.jar
    |   |-- zookeeper-3.8.3.jar
    |   |-- zookeeper-jute-3.8.3.jar
    |   `-- zstd-jni-1.5.5-1.jar
    |-- LICENSE
    |-- licenses
    |   |-- argparse-MIT
    |   |-- CDDL+GPL-1.1
    |   |-- checker-qual-MIT
    |   |-- DWTFYWTPL
    |   |-- eclipse-distribution-license-1.0
    |   |-- eclipse-public-license-2.0
    |   |-- jline-BSD-3-clause
    |   |-- jopt-simple-MIT
    |   |-- jsr305-BSD-3-clause
    |   |-- paranamer-BSD-3-clause
    |   |-- pcollections-MIT
    |   |-- slf4j-MIT
    |   `-- zstd-jni-BSD-2-clause
    |-- NOTICE
    `-- site-docs
        `-- kafka_2.12-3.6.1-site-docs.tgz

8 directories, 188 files
```























#### key concepts

官网的 [Introduction](https://kafka.apache.org/intro) 部分 介绍了 Event， Producer , Consumer ,Topic , Partition 的概念，没有 `consumer group` 概念。

下面是总结的

**Producers** 

**consumers** 

**topics** : 一个主题就是一张表，表中有分区。

**partitioned** 主题是分区的，partition 分布在broker中，partition 使用副本机制，每个分区的多个副本选出一个leader，其他都是 follower 。producer 和 consumers 操作对象都是leader。

注意leader和broker没关系，每个broker都可能是leader，换句话说，leader有很多台。

consumers group （CG）： 每个消费者组和Topic对应，每个consumer消费一个或者多个分区，一个分区只能被一个消费者（同组）消费。

> 这个总结结合尚硅谷文档看。






#### Getting Start 

##### 部署

依赖 zookeeper ， 理解`broker.id`的含义。启动集群。

Kafka是一个分布式系统，由服务器和客户端组成，通过高性能TCP网络协议进行通信。它可以部署在本地和云环境中的裸机硬件、虚拟机和容器上

Kafka作为一个或多个服务器的集群运行，可以跨越多个数据中心或云区域。

