[TOC]

# 一、Docker概述

## 1.1 什么是 Docker

Docker是**一个用于开发、发布和运行应用程序的开放平台**。

Docker使您能够将应用程序与基础架构分开，以便快速交付软件。使用Docker，您可以<font color="red">**以管理应用程序的相同方式管理基础架构**</font>。通过利用Docker的发布、测试和部署代码的方法，您可以显着减少编写代码和在生产中运行代码之间的延迟。

Docker提供了在称为容器的松散隔离环境中打包和运行应用程序的能力，并提供工具和平台来管理容器的生命周期。



## 1.2 Docker 如何工作

Docker<font color="red">**帮助开发人员在任何地方 构建、共享 和 运行  应用程序，无需繁琐的环境配置或管理**</font> 。

> 正如 [Docker: Accelerated Container Application Development ](https://www.docker.com/) 所言， **Accelerate how you build, share, and run applications**

其中，docler具体工作方式为：

- Build ： 快速启动新环境；与您现有的工具集成；容器化应用程序以实现一致性
- Share：使用经过验证的可信内容构建；与您的团队合作，从Hub拉取和发布镜像； 保护您的工作空间；
- Run： 一致的应用程序交付； 多功能性发展； 使用单个命令部署





## 1.3 Docker的优势





## 1.4 底层技术

 Docker 最初是基于 Linux 内核特性构建的。其中的关键特性包括 **命名空间（Namespace）**和控制组（cgroup），它们使得容器能够在隔离的环境中运行，有自己的文件系统、网络和进程空间。

> [The underlying technology](https://docs.docker.com/get-started/overview/#the-underlying-technology)



# 二、Docker架构

## 2.1 Docker 整体架构

Docker 使用**客户端-服务器架构**。

这意味着有两个主要组件：Docker 客户端 和 Docker 守护进程（daemon）。架构细节如下：

- Docker 客户端用于与用户交互和发送命令。而守护进程负责处理这些命令的执行，即负责构建、运行和分发 Docker 容器。
- Docker 客户端和守护进程可以在同一系统上运行，也可以将 Docker 客户端连接到远程 Docker 守护进程。这种架构允许用户在本地或远程环境中管理 Docker 容器。

- Docker 客户端与 Docker 守护进程( Docker daemon ) 使用  REST API、UNIX 套接字或网络接口进行通信。
- 另一个 Docker 客户端是 Docker Compose，它允许您使用由一组容器组成的应用程序。Docker Compose 允许用户通过简化的配置文件定义和管理多容器应用。

![Docker Architecture diagram](images/docker-architecture.webp)



## 2.2 Docker daemon

[Docker daemon](https://docs.docker.com/get-started/overview/#the-docker-daemon) （`dockerd`）监听 Docker API 请求并管理 Docker 对象，包括镜像、容器、网络和卷。它是整个 Docker 引擎的核心组件。

Docker 守护进程可以与其他 Docker 守护进程通信，以协同管理 Docker 服务。这种分布式特性使得可以在多个主机上创建和管理 Docker 容器，并通过 Docker Swarm 或 Kubernetes 等工具进行集群管理。

总体而言，Docker 守护进程充当着 Docker 引擎的控制中心，负责协调和执行各种 Docker 操作，从而实现容器化应用的构建、部署和管理。



## 2.3 Docker client

[Docker client ](https://docs.docker.com/get-started/overview/#the-docker-client)（`docker`）是许多 Docker 用户与 Docker 交互的主要方式。当您使用诸如 `docker run` 等命令时，客户端将这些命令发送到 `dockerd`，由它来执行。`docker` 命令使用 Docker API。

Docker 客户端具备与多个 Docker 守护进程进行通信的能力。这种特性使得用户可以管理位于不同主机上的 Docker 守护进程，从而实现更为复杂的容器编排和集群管理。



## 2.4 Docker registries

[Docker registries](https://docs.docker.com/get-started/overview/#docker-registries)  是用于存储和管理Docker镜像的中央存储库。它允许用户共享和访问Docker镜像。

Docker Hub 是一个公共的、**官方托管的 Docker 注册表**。用户可以在 Docker Hub 上找到和分享各种Docker镜像。默认情况下，当用户未指定镜像来源时，Docker会从Docker Hub上检索镜像。

 用户可以搭建自己的**私有 Docker 注册表**，用于存储和管理自定义的 Docker 镜像。这种私有注册表可以在内部网络中使用，提供更高的安全性和控制。

当您使用`docker pull`或`docker run`命令时，Docker 会从您配置的注册表中拉取所需的镜像。当您使用`docker push`命令时，Docker 会将您的镜像推送到配置的注册表中。通过这些命令，Docker 用户能够方便地管理镜像的获取和发布，无论是使用公共注册表还是自建私有注册表。



## 2.5 Docker objects

[Docker objects](https://docs.docker.com/get-started/overview/#docker-objects)  主要包括 镜像、容器、网络、卷、插件等对象

**（1）镜像（Images）**

<font color="blue">**[Images](https://docs.docker.com/get-started/overview/#images)是一个只读的模板，包含创建 Docker 容器的指令**</font>。您可以创建自己的镜像，也可以仅使用由其他人创建并发布在注册表中的镜像。要构建自己的镜像，您需要创建一个 Dockerfile，其中包含用于定义创建镜像和运行它所需步骤的简单语法。

镜像的性质主要有：

- **只读性质：** 镜像是只读的，一旦创建就不可更改。任何对容器的修改都是在镜像的基础上进行的，而不会影响原始镜像。

- **基于其他镜像：** 通常，镜像是基于其他镜像构建的，添加一些额外的定制。例如，您可以构建一个基于ubuntu镜像的镜像，但安装了Apache web服务器和您的应用程序，以及使您的应用程序运行所需的配置细节。
- **分层结构：** 镜像是由多个文件系统层（layers）组成的，每个层都包含了特定的文件和配置。Dockerfile 中的每个指令都会在镜像中创建一个层。当您更改Dockerfile并重新构建镜像时，只有更改的那些层会被重新构建。这是使镜像相对于其他虚拟化技术如此**轻量**、小巧和快速的一部分。
- **标签（Tags）：** 镜像可以通过标签来标识和区分，标签通常表示版本、特定用途等信息。例如，`ubuntu:20.04` 表示了一个基于Ubuntu 20.04版本的镜像。

**(2)容器（Containers）**

<font color="blue">**[Containers](https://docs.docker.com/get-started/overview/#containers)是镜像的可运行实例**</font> 。您可以使用Docker API或CLI创建、启动、停止、移动或删除容器。您可以将容器连接到一个或多个网络，附加存储，甚至基于其当前状态创建一个新的镜像。

> 镜像和容器之间的关系，类似于面向对象程序设计中的 类 和 对象

默认情况下，容器与其他容器及其主机机器相对隔离。您可以控制容器的网络、存储或其他底层子系统与其他容器或主机机器的隔离程度。



## 2.6 Docker Desktop

[Docker Desktop](https://docs.docker.com/desktop/) 是一个**一键安装的应用程序**，适用于您的Mac、Linux或Windows环境，它允许您构建、分享和运行容器化的应用程序和微服务。

它提供了一个直观的GUI（图形用户界面），让您可以直接从您的计算机管理您的容器、应用程序和镜像。您可以将Docker Desktop作为独立工具使用，也可以作为CLI的补充工具使用。





# 参考资料

[Docker overview | Docker Docs](https://docs.docker.com/get-started/overview/#containers)

[Docker 架构 | 菜鸟教程 (runoob.com)](https://www.runoob.com/docker/docker-architecture.html)