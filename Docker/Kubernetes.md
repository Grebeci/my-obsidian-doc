# Kubernetes

## Overview 

最初是是解决容器编排问题的，最后发展成为完整的容器管理平台。

官网 [概述 | Kubernetes](https://kubernetes.io/zh-cn/docs/concepts/overview/) 写的很好

```
关于官网上的 容器的优势一些解释
- 《开发和运维分离》：开发人员可以发布和部署镜像，不用在部署的时候创建。运维人员可以只管部署。

```



Borg是Kubernetes的前身，Borg论文地址：[Large-scale cluster management at Google with Borg](https://storage.googleapis.com/pub-tools-public-publication-data/pdf/43438.pdf)

中文版 ：[Google 大规模集群管理器 Borg (opskumu.com)](https://blog.opskumu.com/borg.html#org9e00d15)

中文版二： [(66条消息) Large-scale cluster management at Google with Borg_琦彦的博客-CSDN博客](https://blog.csdn.net/fly910905/article/details/120840292)



关于docker和 kubernetes 的相互关系，参考： [K8S 弃用 Docker 了？Docker 不能用了？别逗了！ - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/334787180)



## 架构和组件

AI-prompt: kubenetes的架构和组件图你可以在官网上找到吗?

架构图 :

[Understanding Kubernetes Architecture with Diagrams ~ 通过图表了解 Kubernetes 架构 (phoenixnap.com)](https://phoenixnap.com/kb/understanding-kubernetes-architecture-diagrams)

官网上的解释 ：

[Kubernetes Components ](https://kubernetes.io/docs/concepts/overview/components/)

RedHat 的解释 ：

[Introduction to Kubernetes architecture ~ Kubernetes 架构介绍 (redhat.com)](https://www.redhat.com/en/topics/containers/kubernetes-architecture)



学习K8S,主要是了解概念，所谓概念就是K8S如何对资源抽象，资源指的是 存储， 容器（类似虚拟机）， 管理（如何编排，监控 容器）。

#### Pod

[Pod | Kubernetes](https://kubernetes.io/zh-cn/docs/concepts/workloads/pods/)

逻辑主机的概念，一组容器的集合，这些容器共享存储，网络。



#### Label



#### Cotroller

控制器是一种用于管理 Pod 副本数量和确保所需数量的副本始终运行的资源。不同类型的控制器适用于不同的用例，具有不同的功能和行为。

#### Volume

对存储（磁盘）的抽象

Volume 是 Pod 中能够被多个容器访问的共享目录。Kubernetes 的 Volume 定义在 Pod 上， 它被一个 Pod 中的多个容 器挂载到具体的文件目录下。



**持久卷（PersistentVolume，PV）**: 是集群中的一块存储

**持久卷申领（PersistentVolumeClaim，PVC）** 表达的是用户对存储的请求



#### Secret

对各种系统认证所需的密码，令牌，密钥的抽象。

#### ConfigMap

对配置的抽象，ConfigMap 是一种 API 对象，用来将非机密性的数据保存到键值对中。

#### Namespace

多用户资源隔离，对集群内部资源（Pod，RC，Service）进行分组。

#### service

Service 是 Kubernetes 最核心概念，通过创建 Service,可以为一组具有相同功能的容器应 用提供一个统一的入口地 址，并且将请求负载分发到后端的各个容器应用上。



#### 容器探针

**probe** 是由 [kubelet](https://kubernetes.io/zh-cn/docs/reference/command-line-tools-reference/kubelet/) 对容器执行的定期诊断

#### 调度器

Kubernetes 则采用了 Pod 和 Label 这样的概念把容器组合成一个个的互相存在依赖关系 的逻辑单元。相关容器被组合成 Pod 后被共同部署和调度，形成服务（Service）。

调度器区分两种类型，可压缩资源，不可压缩资源



##### 角色控制 

RBAC(Role-Based Access Control，基于角色的访问控制)



## 搭建K8S



## CLT

kubectl : [命令行工具 (kubectl) | Kubernetes](https://kubernetes.io/zh-cn/docs/reference/kubectl/) 了解格式，功能。

备忘单：[kubectl 备忘单 | Kubernetes](https://kubernetes.io/zh-cn/docs/reference/kubectl/cheatsheet/)





## QAP

##### k8s 架构

Q: AI-Prompt 我在学习K8s，请告诉我K8s各种组件的名称和作用。

##### 组件

1. Pod

Q:  k8s 为什么要有pod的概念，它是为了解决什么问题的？



##### Getting Started

集群搭建-kubeadmin 方式



## 资料

尚硅谷Kubernetes教程(K8s入门到精通)

这个视频先讲概念，而且比较突兀。经常蹦出没有任何前置条件的概念，讲的比较空泛。让人听不下去。

