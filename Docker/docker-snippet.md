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









### vscode 终端快捷键

在 Visual Studio Code 的终端中删除整行文本，可以使用快捷键：

- **Linux 和 Windows**：`Ctrl+U`
- **macOS**：`Ctrl+U`

这个快捷键会删除从当前光标位置到行首的所有文本。如果你的光标已经在行首，它会删除整行文本。如果你想删除从光标位置到行尾的文本，可以使用：

- **Linux 和 Windows**：`Ctrl+K`
- **macOS**：`Ctrl+K`

请注意，这些快捷键在大多数 shell 环境中都是有效的，但它们的行为可能会根据你使用的具体 shell（如 bash、zsh 等）或终端配置有所不同。

这个快捷键是Bash快捷键





#### Linux

centos 下查找特定的包是否安装

```
rpm -q [包名1,包名2，，，]
```

