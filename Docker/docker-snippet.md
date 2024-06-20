### Dokcer 进程、客户端状态

版本

```shell
docker --version
```

docker engine，docker client信息

```shell
docker info
```

运行一个 `Helloworld`容器，证明Dokcer 安装成功

```
docker run hello-world
```



Docker 容器

```shell
docker images

docker run -it --name base centos:centos7.9.2009 /bin/bash  # 交互式方式运行

docker run -d --name base centos:centos7.9.2009 /bin/sh -c "while true; do sleep 1000; done" # 不停止

docker ps   # 正在运行的容器
docker ps -a # 所有的容器，不管是exit还是run

docker exec -it base /bin/bash  
```



