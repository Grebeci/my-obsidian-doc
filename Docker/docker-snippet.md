

## Docker

###### Dokcer 进程、客户端状态

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



todo docker 建立SSH 服务

```
sh-keygen -A 
sed -i '/^Host/a\ \ \ \ \ \ \ \ StrictHostKeyChecking no' /etc/ssh/ssh_config 
ssh-keygen -t rsa -N '' -q -f /root/.ssh/id_rsa 
cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys 
sed -i 's/^#UseDNS yes/UseDNS no/' /etc/ssh/sshd_config 
```











## VSCODE

####  终端

在 Visual Studio Code 的终端中删除整行文本，可以使用快捷键：

- **Linux 和 Windows**：`Ctrl+U`
- **macOS**：`Ctrl+U`

这个快捷键会删除从当前光标位置到行首的所有文本。如果你的光标已经在行首，它会删除整行文本。如果你想删除从光标位置到行尾的文本，可以使用：

- **Linux 和 Windows**：`Ctrl+K`
- **macOS**：`Ctrl+K`

请注意，这些快捷键在大多数 shell 环境中都是有效的，但它们的行为可能会根据你使用的具体 shell（如 bash、zsh 等）或终端配置有所不同。

这个快捷键是Bash快捷键



#### Eidt 

在 Visual Studio Code 中删除当前行的快捷键是：

- Windows/Linux: `Ctrl + Shift + K`
- macOS: `Cmd + Shift + K`

将光标放在你想要删除的行上，然后使用上述快捷键即可删除该行。



## Linux

centos 下查找特定的包是否安装

```
rpm -q [包名1,包名2，，，]
```

```
yum list installed | grep [包名]
```

```
yum info [包名]

1. Installed Packages 还是 Available Packages
2. Repo        : installed    还是   Repo        : base/7/x86_64


解释
`yum info` 命令可以显示软件包的详细信息，包括它是否已经被安装。在你提供的输出中，关键信息在于 `Repo` 字段和是否有 `Installed Packages` 部分的出现。

- 如果软件包已安装，`yum info` 输出会包含一个 `Installed Packages` 部分，显示已安装包的详细信息。
- 如果软件包未安装，输出中会显示 `Available Packages` 部分，如你的示例所示，表示该包可用于安装但当前未安装。

在你的示例中，`net-tools` 显示在 `Available Packages` 部分，说明 `net-tools` 包可用于安装，但尚未安装在系统上。如果 `net-tools` 已安装，你会看到一个额外的部分，标记为 `Installed Packages`，其中包含已安装版本的详细信息。
```



curl -LO download_url

```
-L 这个选项用于让 curl 在服务器返回重定向时，自动跟随新的 URL。
-O curl 根据获取的 URL 在本地生成文件，文件名与 URL 中指定的文件名相同
```



##### SSH

```
ssh-keygen -A
```

在通过容器化技术（如 Docker）部署 SSH 服务时，通常在构建容器镜像或启动容器时运行此命令，以自动配置 SSH 密钥。



##### Sed

替换某个字段

```
sed -i -e '/mirrors.cloud.aliyuncs.com/d' -e '/mirrors.aliyuncs.com/d' /etc/yum.repos.d/CentOS-Base.repo && \
    localedef -c -f UTF-8 -i zh_CN zh_CN.UTF-8 
    
 -i ：直接修改文件
 -e 后面跟sed脚本 '/mirrors.cloud.aliyuncs.com/d'  
 				/mirrors.cloud.aliyuncs.com/ 是一个模式，用来匹配文件中包含 mirrors.cloud.aliyuncs.com 这个字符串的行。
				d 是一个 sed 命令，用于删除匹配到的行。
```

sed 测试

[Seddy - test sed in the browser](https://seddy.dev/)



docker下centos 支持中文

```
yum install -y fonts-dejavu fontconfig glibc-common kde-l10n-Chinese

localedef -c -f UTF-8 -i zh_CN zh_CN.UTF-8 && \
echo 'LANG="zh_CN.UTF-8"' > /etc/locale.conf && \
echo 'export LANG="zh_CN.utf8"' > /etc/profile.d/language.sh && \
```





# qustion

mysql 包都是从哪搞得?

```
curl -LO https://downloads.mysql.com/archives/get/p/23/file/mysql-community-client-8.0.31-1.el7.x86_64.rpm
curl -LO https://downloads.mysql.com/archives/get/p/23/file/mysql-community-client-plugins-8.0.31-1.el7.x86_64.rpm
curl -LO https://downloads.mysql.com/archives/get/p/23/file/mysql-community-common-8.0.31-1.el7.x86_64.rpm
curl -LO https://downloads.mysql.com/archives/get/p/23/file/mysql-community-libs-8.0.31-1.el7.x86_64.rpm
curl -LO https://downloads.mysql.com/archives/get/p/23/file/mysql-community-libs-compat-8.0.31-1.el7.x86_64.rpm
```



### TODO

##### 1. SSH 服务

```
ssh-keygen -A
```



##### 2. VSCODE-Docker服务









### windows function

##### Windows Frame

```
# [partition start, current row]
Rows BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
			<=> over(order by expr) 
			<=> rows BETWEEN UNBOUNDED
```

```
expr PRECEDING | expr FOLLOWING  

UNBOUNDED PRECEDING | UNBOUNDED FOLLOWING
```

```
{Range | rows} BETWEEN ,,, AND ,,,
```



##### windows Functions

aggregate functions

```
AVG()
COUNT()
SUM()
MAX()
MIN()
```



```
RANK()
ROW_NUMBER() 
DENSE_RANK() 
```



```
LAG(expr [, N[, default]])
LEAD(expr [, N[, default]])
NTH_VALUE(expr, N)
FIRST_VALUE(expr)
LAST_VALUE(expr) 
```





