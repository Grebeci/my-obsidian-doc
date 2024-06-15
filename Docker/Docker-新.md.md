## 为什么要有Docker？

docker可以类比虚拟机的概念。只不过它在实现上更轻量，这种方式叫做容器。为了方便部署软件，我们可以将应用程序所需的环境，如操作系统、依赖库和其他配置，打包成一个镜像。而镜像的分发主要依赖于仓库，类似于应用市场的概念。这样，我们就可以在安装 `Docker Engine` 任意节点上拉取镜像并部署软件。由此而来的三个概念是容器、镜像和仓库。以下是它们的严格定义。

## Key Concept

**Docker** 包括三个基本概念

- **镜像**（`Image`）：容器使用隔离的文件系统，是包含应用程序及其全部依赖和运行环境的轻量级、只读的模板。
- **容器**（`Container`）：镜像的独立的运行时。
- **仓库**（`Repository`）：集中的存储、分发镜像的服务。

## install
参考 doc.docker.com | Manual -> Docker Engine -> install
> 可能需要更改 yum 源; 
> 可能需要更改 docker 源

整理安装步骤
```shell 
# 移除旧版本docker
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine

# 配置docker yum源。
sudo yum install -y yum-utils
sudo yum-config-manager \
--add-repo \
http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo


# 安装 最新 docker
sudo yum install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# 启动& 开机启动docker； enable + start 二合一
systemctl enable docker --now

# 配置加速
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://82m9ar63.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## 常用命令

命令帮助
> Docker 命令风格是“子命令风格” 一般是：
>
> `docker [OPTIONS] COMMAND [arg...]`

```shell
docker [command,subcomand,,,] --help
```

Docker 的 `--help`  可以在任意级别使用。无论用户处于命令结构的哪一层，都可以通过添加 --help 来获取当前命令或子命令的详细说明和可用选项。


### 工作流 

以安装 Nginx 为例。

1. 搜索镜像

```shell
docker search nginx
```
这个搜索简单地从 Docker Hub 检索镜像。实际需要从 hub.docker.com 搜索。

2. 拉取镜像
```bash
docker pull nginx:1.26.0
```
镜像名 ： name:tag

3. 查看镜像

```bash
docker images
```

4. 启动容器

```bash
docker run -d --name mynginx -p 8080:80 nginx:1.26.0
```
- `-d` 后台运行
- `--name` 容器名
- `-p` 8080:80 端口映射, 本地端口:容器端口
- `nginx:1.26.0` 镜像名
- `mynginx` 容器名

5. 查看容器

```bash
docker ps
```
```bash
docker ps -a
```

6. 进入容器
```bash
docker exec -it mynginx /bin/bash
```
- `-it` 交互式
- `mynginx` 容器名
- `/bin/bash` 进入容器后执行的命令


7. 容器停止，启动，重启
```bash
docker stop mynginx
docker start mynginx
docker restart mynginx
```

8. 删除容器
```bash
docker rm mynginx
```

9. 删除镜像
```bash
docker rmi nginx:1.26.0
```

10. 查看日志
```bash
docker logs mynginx
```

### 制作镜像
以 nginx 为例

1. docker commit
```bash
docker commit -m "add index.html" -a "author" mynginx nginx:v1.0
```
- `-m` 提交信息
- `-a` 作者
- `mynginx` 容器名
- `nginx:v1.0` 镜像名
- `nginx:v1.0` 新镜像名

2. 镜像打包
```bash
docker save -o nginx.tar nginx:v1.0
```
- `-o` 输出文件
- `nginx.tar` 输出文件名
- `nginx:v1.0` 镜像名

3. 镜像导入
```bash
docker load -i nginx.tar
```
- `-i` 输入文件
只是恢复到 images 阶段，还需要 docker run 启动。


### 镜像分发
1. 登录
```bash
docker login
```

2. 命名
```bash
docker tag nginx:v1.0 username/mynginx:v1.0
```
也就是
```bash
docker tag image username/repository:tag
```

3. 推送
```bash
docker push username/mynginx:v1.0
```

4. 拉取
```bash
docker pull username/mynginx:v1.0
```

## docker storage

容器的运行时状态是临时的，当容器被删除时，容器内部的数据也会被删除。为了持久化数据，有 bind mount 和 volume 两种方式。

1. bind mount
```bash
docker run -d --name mynginx -p 8080:80 -v /data:/usr/share/nginx/html nginx:1.26.0
```
- `-v` local_absoulte_path:container_path .
- `/data` 本地目录, 会映射到容器内的 `/usr/share/nginx/html` 目录

  1. 在容器更改或者宿主机更改都会同步。
  2. 本地目录会覆盖容器内非空目录。这不同于 volume。

2. volume
```bash
docker run -d --name mynginx -p 8080:80 -v ngconf:/etc/nginx nginx:1.26.0
```
默认 volume 存放在 `/var/lib/docker/volumes/<volume-name>` 目录下。

  1. 查看卷位置
  ```bash
  docker volume inspect ngconf
  ```
  2. 删除卷
  ```bash
  docker volume rm ngconf
  ```
  3. 本地目录不会覆盖容器内的非空目录。

## docker network
docker在创建的时候。 就会。 就会创建一个默认网络。 可以用i p a。 奶茶看。 下面是他的代码。 

```bash
ip a
```
```
