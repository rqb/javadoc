---
typora-copy-images-to: 文章\gif
---



# Docker介绍

## Docker出现的背景

Docker是PasS提供商DoctCloud开源的一个基于LXC的高级容器引擎，源代码托管在Github上，基于go语言并遵从Apache2.0协议开源。Docker近期非常火热，无论是从Github上的代码活跃度，还是Redhat在REHEL6.5中集成对Docker的支持，就连Google的Compute Engine也支持docker在意之上运行，百度、阿里、新浪、京东也开始使用Docker作为PaaS基础。

系统架构的改变：

三层架构、部署到有限的物理机上---->分布式、微服务的架构、可能会部署到不同的环境（虚拟机、公有云、私有云）

环境管理复杂：

从各种OS到各种中间件到各种app，一款产品能够成功作为开发者需要关心的东西太多，且难于管理，这个问题几乎在所有现代IT相关行业都需要面对，对此Docker可以简化部署多种应用实例工作，比如Web应用、后台应用、数据库应用、大数据应用比如Hadoop集群、消息队列等等都可以打包成一个 Image部署。

![img](D:\技术文档\面试资料整理\git\文章\gif\laurel-docker-containers.png)



> 集装箱场景完美诠释了docker



## Docker解决了什么问题

1. 解决了运行环境和配置问题，方便做持续集成
2. 减少运维的工作量

## Docker与虚拟机的区别

容器在Linux上本地运行，并与其他容器共享主机的内核。它运行一个相互独立的进程，占用的内存不超过任何其他可执行文件，从而使它变得轻量级。
相反，虚拟机（VM）运行一个完整的“guest”操作系统，通过管理程序虚拟访问主机资源。通常，虚拟机为环境提供的资源比大多数应用程序所需的资源多。

传统的虚拟化技术、比如Vmware、KVM、Xen，目标是创建完整的虚拟机，为了运行应用，除了部署应用本身及其依赖（几十MB）还有安装整个操作系统（几十GB）

- [ ] [![img](https://docs.docker.com/images/Container%402x.png)]()



- [ ] ![img](https://docs.docker.com/images/VM%402x.png)



https://www.docker.com/

https://www.docker-cn.com/



# 安装Docker

## 安装仓库

（1）允许apt命令https访问docker

```shell
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```

（2）添加docker官方的GPG key

```shell
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

（3）将docker的仓库添加到/etc/apt/sources.list

```shell
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

## 安装Docker

```shell
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
```

安装指定版本的docker

```shell
$ sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io
```

## 验证

（1）验证版本

```shell
docker --version

Docker version 17.12.0-ce, build c97c6d6
```

```shell
$ docker info

Containers: 0
 Running: 0
 Paused: 0
 Stopped: 0
Images: 0
Server Version: 17.12.0-ce
Storage Driver: overlay2
...
```

（2）验证安装

```shell
$ sudo docker run hello-world
```

docker  run命令从Docker Hub上下载hello-world镜像，在容器中运行，它打印一个信息然后退出。



# Docker架构

Docker客户端

Docker服务端

Docker镜像

Registry

Docker容器

![1554027991155](D:\技术文档\面试资料整理\git\文章\gif\1554027991155.png)





![1554028126036](D:\技术文档\面试资料整理\git\文章\gif\1554028126036.png)





容器和镜像





# Docker基础命令

## docker image

docker image -a

docker image -q

## docker search

docker search -s 30 tomcat 

查找start超过30的tomcat镜像

## docker pull



## docker rmi 

docker rmi -f 镜像名

## docker run

docker run -i 交互式  -t  

docker run -d  后台方式运行

## docker ps



## docker exec

进入容器

## docker commit











