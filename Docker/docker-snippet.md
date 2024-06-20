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

docker run -d --name my-nginx -p 8080:80 nginx
```

