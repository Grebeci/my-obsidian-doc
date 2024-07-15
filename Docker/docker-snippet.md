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

运行一个 `Helloworld`容器，证明 Dokcer 安装成功

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







好的，让我们来讨论一下 Linux 上的包管理。

### 什么是包管理？

在 Linux 系统中，包管理是一种管理软件包（即软件程序及其依赖关系）的系统。软件包是一个包含应用程序或库及其元数据的档案文件。包管理系统（Package Management System, PMS）则负责安装、更新、配置和移除软件包，确保软件及其依赖关系能正确地安装和运行。

### 包管理系统的核心概念

1. **软件包**：一个软件包通常是一个归档文件（如 `.deb` 或 `.rpm`），包含了应用程序的所有必要文件，包括二进制文件、库、配置文件和文档。

2. **依赖关系**：许多软件需要其他软件才能运行，这种依赖关系可以非常复杂。包管理系统会自动处理这些依赖关系，确保在安装或升级软件时，所有依赖项都得到满足。

3. **仓库（Repository）**：这是存放软件包的服务器或位置。包管理系统可以从这些仓库下载和安装软件包。大多数 Linux 发行版都有自己的官方仓库。

4. **元数据**：每个软件包都带有元数据，描述了包的内容、版本、依赖关系和其他信息。包管理系统利用这些元数据来管理软件包。

5. **安装和卸载**：包管理系统使得软件的安装和卸载变得简单且安全。它会确保在卸载软件时，也能处理好依赖关系，不会影响系统的其他部分。

### 常见的 Linux 包管理系统

不同的 Linux 发行版使用不同的包管理系统。以下是一些常见的包管理系统：

1. **APT (Advanced Package Tool)**：主要用于 Debian 和基于 Debian 的发行版（如 Ubuntu）。它使用 `.deb` 包格式。
   - 命令示例：`apt-get install <package-name>`，`apt-get update`，`apt-get upgrade`。

2. **YUM (Yellowdog Updater, Modified)** 和 **DNF (Dandified YUM)**：主要用于 Red Hat、CentOS 和 Fedora，使用 `.rpm` 包格式。
   - 命令示例：`yum install <package-name>`，`dnf update`。

3. **Zypper**：用于 openSUSE 和 SUSE Linux Enterprise，使用 `.rpm` 包格式。
   - 命令示例：`zypper install <package-name>`。

4. **Pacman**：用于 Arch Linux，使用 `.pkg.tar.xz` 包格式。
   - 命令示例：`pacman -S <package-name>`。

5. **Snap**：一种跨发行版的包管理系统，由 Canonical 开发，提供一种独立于发行版的方式来分发和更新软件。
   - 命令示例：`snap install <package-name>`。

6. **Flatpak**：另一个跨发行版的包管理系统，允许应用程序在不同的 Linux 发行版上运行。
   - 命令示例：`flatpak install <repository> <package-name>`。

7. **Homebrew (Linuxbrew)**：最初为 macOS 设计，现在也支持 Linux，提供了一种灵活的包管理方式。
   - 命令示例：`brew install <package-name>`。

### 包管理系统的文档和资源

你可以参考以下文档和资源来深入了解不同的包管理系统：

1. **APT (Debian 系)**：
   - [Debian APT 入门指南](https://wiki.debian.org/Apt)
   - [Ubuntu APT 文档](https://help.ubuntu.com/community/AptGet/Howto)

2. **YUM/DNF (Red Hat 系)**：
   - [YUM 官方文档](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/deployment_guide/ch-yum)
   - [DNF 官方文档](https://dnf.readthedocs.io/en/latest/)

3. **Zypper (SUSE 系)**：
   - [Zypper 文档](https://en.opensuse.org/SDB:Zypper_manual)

4. **Pacman (Arch Linux)**：
   - [Pacman 文档](https://wiki.archlinux.org/index.php/Pacman)

5. **Snap**：
   - [Snapcraft 文档](https://snapcraft.io/docs)

6. **Flatpak**：
   - [Flatpak 文档](https://docs.flatpak.org/en/latest/)

7. **Homebrew (Linuxbrew)**：
   - [Linuxbrew 官方文档](https://docs.brew.sh/Homebrew-on-Linux)

### 小结

包管理系统是 Linux 操作系统中不可或缺的一部分，它们简化了软件安装、升级和维护的过程，使用户能够方便地管理系统上的应用程序和库。

如果你有特定的包管理系统想要深入了解，或者需要某个系统的实际操作示例，请告诉我！



