### 一、Docker

- 统一开发，测试，运维环境。
- 打包应用运行的环境为一个镜像。

解决了运行环境和配置问题的软件容器，方便做持续集成并有助于整体发布的容器虚拟化技术。



容器和虚拟机的比较

linux容器： 进程隔离，实现操作系统层面的虚拟化，复用本地主机的操作系统。

虚拟机 ： 模拟完整的操作系统，硬件层面的虚拟化了。

Docker 容器用起来就像是虚拟机一样。

### 二、重要概念

- 镜像（Image）
- 容器化虚拟化技术（container）
- 仓库（repository）
- Dockerfile
- 容器卷
- Network

## 三、Getting Starting


#### 3.1 安装

[Install Docker Engine on CentOS | Docker Documentation](https://docs.docker.com/engine/install/centos/)

[容器镜像服务 (aliyun.com)-镜像加速器](https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors)


#### 3.2 常用命令

1. 帮助启动类

	```
	启动： systemctl start | stop | restart | status  docker
	
	开机启动： systemctl enable | disable docker
	
	查看概要信息  docker info
	
	查看docker总体帮助文档  docker --help
	查看docker命令帮助文档  docker 具体命令  --help
	```

2. 镜像命令

	**列出本地的镜像**

	```bash
	docker images -aq
	
	-a : 列出把本地的所有你镜像
	-q : 只显示镜像ID
	
	REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
	hello-world   latest    9c7a54a9a43c   2 months ago   13.3kB
	```

	输出：

	REPOSITORY： 表示镜像的仓库源

	TAG： 镜像标签

	IMAGE ID ：镜像ID

	CREATE ：镜像创建时间

	SIZE：镜像大小

	同一个仓库源可有多个TAG版本，代表这个仓库源的不同版本，我们使用 REPOSITORY：TAG 来定义不同的镜像。

	**查询仓库镜像**

	从docker hub

	```
	docker search --limit 5 redis
	```

	**拉取**

	```
	docker pull REPOSITORY:[TAG]
	
	缺省 TAG默认最新
	```

	**查看镜像，容器，数据卷所占空间**

	```
	docker system df
	```

	**删除**

	```
	docker rmi [-f|--forece] ImageID
	```

3. 容器命令

	**运行**

	```
	# 前台交互启动
	docker run --name my-ubuntu  -it ubuntu /bin/bash 
	有些容器比如（ubuntu） 如果后台运行，就必须有一个前台进程，否则就会自动kill
	
	# 后台守护启动 
	docker run --name my-redis  -d redis:6.0.8 
	
	```


	**进入容器**

	```
	docker exec  -it 容器id或者容器名  /bin/bash 
	
	docker attach   -it ubuntu /bin/bash  # 不推荐，因为exit 会停止这个容器
	```


	**日志**

	```
	docker logs 容器id或者容器名 
	```

	**查看容器内运行的进程** 

	```
	docker top 容器id或者容器名 
	```

	**查看容器内部细节**

	```
	docker inspect 容器id或者容器名 
	```

	

	**列出正在运行的容器**

	```
	docker ps  # 正在运行的
	-a 正在运行的，+ 历史上的
	-l 最近的创建的
	-n 最近N个创建的容器
	-q 静默模式，只显示容器编号
	```

	**退出**

	exit :退出容器,容器停止（对于 run ,attach 会有 这个行为）

	ctrl p+q : 退出容器，容器不停止

	**启动、停止，重启**

	```
	docker start | restart | stop | kill 容器id或者容器名 
	
	
	```

	**删除**

	```
	docker  rm  [-f] 容器id或者容器名 
	```

	

	**文件传输**

	容器 => 宿主机

	```
	docker cp 容器ID:容器内的路径 目的主机路径
	```

	容器到处为tar文件

	```
	dcoker export 容器ID  >  x.tar
	```

	把 tar文件导入为 镜像

	```
	cat x.tar | docker import - 镜像用户/镜像名字:镜像版本号
	```






## 四、Dockerfile

Dockeefile 用来构建Docker 镜像的文本文件。

##### 格式

## 五 、Docker Compose 



# DAQ

#### P: 搭建常见的服务

1. 搭建tomcat

	[tomcat - Official Image | Docker Hub](https://hub.docker.com/_/tomcat/)

	重点命令：

	```
	 docker run d -p 8080:8080 --name t1 tomcat
	 docker ps
	 docker exec -it ID /bin/bash
	 
	 #小坑
	 rm -r webapp.dist
	 mv webapp.dist webapp
	```

2. 安装 MySQL

	注意容器卷映射

	```
	docker run -d -p 3306:3306 --privileged=true  \
	-v /var/mysql/log:/var/mysql/log \
	-v /var/mysql/data:/var/lib/mysql \
	-v /etc/mysql/conf:/etc/mysql/conf.d \
	-e MYSQL_ROOT_PASSWORD=123456 \
	 mysql:5.7 
	```

	

	注意： 5.7 的编码问题

	不要再客户端测试，要在服务器上测试。

	```
	show VARIABLES LIKE 'character%'; 
	```

	

	```
	tee /etc/mysql/conf/my.cnf <<-'EOF'
	[client]
	default-character-set=utf8
	
	[mysqld]
	default-storage-engine=INNODB
	character-set-server=utf8
	collation-server=utf8_general_ci
	EOF
	
	#重启docker
	```

	

	主从复制原理

	

3. 安装 Redis

	- 注意 redis.conf 要做容器卷映射

4. ngnix



#### 知识相关

#### dockerfile

A: 什么是dockerfile ？

- dokcerfile是干什么的，解决的问题（功能）？

- dockerfile 文件格式 ?

- dockerfile的编写

	- 读取 dockerhub 提供的 tomcat 的dockerfile， 熟悉保留字

		**[tomcat - Official Image | Docker Hub](https://hub.docker.com/_/tomcat)**

		[Dockerfile reference | Docker Documentation](https://docs.docker.com/engine/reference/builder/)

	- 

A: docker 执行dockerFile 的大致流程

#### docker network

A: 命令

- 列出、查看、删除、创建

A: 有哪些应用场景，举例说明

- 端口映射
- 不同

A: docker 网络模式有哪些，各种模式的应用场景和局限

- bridge 单主机多容器实例，容器可以访问外网，但是容器之前不能通过容器名称互相访问。默认创建容器时候不指定 `--net` 参数，就是 bridge 模式。



#### 容器编排

#### docker compose


## Docker架构


C/S结构

