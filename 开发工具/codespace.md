codespace 是一个github提供的云上开发环境。是 基于 【配置即代码】来创建可重复使用的，即时的开发环境。



文档：

[GitHub Codespaces 概述 - GitHub 文档](https://docs.github.com/zh/codespaces/overview)



### 自定义 CodeSpaces 容器环境


参考：[开发容器简介 - GitHub 文档](https://docs.github.com/zh/codespaces/setting-up-your-project-for-codespaces/adding-a-dev-container-configuration/introduction-to-dev-containers#creating-a-custom-dev-container-configuration)  基本上是配合 `devcontainer.json` 来定义开发环境的配置， github 提供了一些构建好的镜像 ，如：  [`devcontainers/images`](https://github.com/devcontainers/images/tree/main/src/universal) 存储库。


#### 容器 Feature

1. SSH Server （sshd）： 在容器中添加 SSH 服务器，以便使用外部终端、sftp 或 SSHFS 与之交互。参考：  [SSHD Feature](https://github.com/devcontainers/features/tree/main/src/sshd#usage) 

2. 

