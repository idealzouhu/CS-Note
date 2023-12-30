

```
docker init
```



# 一、Docker镜像



## 1.1 什么是 Docker 镜像



## 1.2 Dockfile 文件

 Dockerfile 是一个文本文件，用于定义如何构建 Docker 镜像。它包含了一系列的指令，每个指令代表一个构建步骤，比如选择基础镜像、安装依赖、拷贝文件等。

 Dockerfile 主要用于自动构建 Docker 镜像，它提供了一种声明式的方式来描述镜像的构建过程。

Dockerfile的基本指令有十三个，分别是：FROM、MAINTAINER、RUN、CMD、EXPOSE、ENV、ADD、COPY、ENTRYPOINT、VOLUME、USER、WORKDIR、ONBUILD。

Dockfile语法具体细节查看[Dockerfile reference | Docker Docs](https://docs.docker.com/engine/reference/builder/)



## 总结

- Dockerfile 用于定义如何构建单个 Docker 镜像。
- Docker Compose 用于定义和运行多容器应用，简化了多容器环境的管理和协调。



# 二、构建Docker镜像

Docker可以通过读取 Dockerfile 中的指令来自动构建镜像。Dockerfile 是一个文本文档，其中包含用户可以在命令行上调用的所有命令来组装镜像。

> [Dockerfile reference | Docker Docs](https://docs.docker.com/engine/reference/builder/)

当你创建好 Dockerfile 后， 可以调用 [docker build](https://docs.docker.com/engine/reference/commandline/build/#usage) 命令从 Dockerfile 构建镜像

```
docker build [OPTIONS] PATH | URL | -
```

事实上，构建Docker镜像的方式主要包含两种：

- **手动操作和提交**： 开发者可以从一个**已有的镜像开始**（通常是一个基础镜像，比如官方的 Ubuntu、Alpine 等），然后在容器中进行各种手动操作和配置，包括安装软件、调整配置等。这些操作会在容器中创建新的文件层，然后通过 `docker commit` 命令将容器的状态提交为一个新的镜像。这种方式类似于在 Git 中手动操作文件并提交新的版本。
- **使用 Dockerfile 自动构建**：构建 Dockerfile ， 自动打包成新的镜像。

无论使用哪种方式，构建 Docker 镜像的核心思想是通过定义清晰的构建步骤，确保最终的镜像包含应用程序所需的一切，并且是可重复、可复制的。



## 2.2 使用 Dockerfile 自动构建

在使用容器时，通常需要创建一个 Dockerfile 来定义镜像，以及一个 `compose.yaml` 文件来定义如何运行容器。

为了帮助你创建这些文件，Docker 提供了一个名为 [docker init](https://docs.docker.com/engine/reference/commandline/init/) 的命令。在项目文件夹中运行这个命令，Docker 将创建所有需要的文件。







# 三、Compose工具

## 3.1 什么是Docker Compose

Docker Compose 是一个用于定义和运行多容器 Docker 应用程序的工具。它通过一个 `compose.yml` 文件来**定义服务、网络、卷等配置**。然后，使用单个命令，我们可以从配置中创建和启动所有服务。

Docker Compose 主要用于简化多容器应用的管理，通过一个文件定义应用的整体架构，包括容器间的连接和通信。使用Compose基本上是一个三步骤的过程：

1. 使用 Dockerfile 定义您的应用程序环境，以便它可以在任何地方被重现。
2. 在`compose.yaml`文件中定义组成您的应用程序的服务，以便它们可以在一个隔离的环境中一起运行。
3. 运行`docker compose up`命令,  启动并运行您的整个应用程序。

Docker Compose 使用手册查看  [Docker Compose overview | Docker Docs](https://docs.docker.com/compose/)



## 3.2 compose.yaml 文件

Compose文件 是一个[YAML文件](https://zh.wikipedia.org/wiki/YAML)，定义了服务（service）、网络、[卷](https://zh.wikipedia.org/w/index.php?title=卷(计算机)&action=edit&redlink=1)（volume）。

- **服务（service）**定义 各容器的配置，定义内容将以命令行参数的方式 传给 `docker run` 命令。
- **网络（network）**，类似地，将定义内容传给 `docker network create` 命令 。
- **卷（volume）**，类似地，将定义内容传给 `docker volume create` 命令。

`compose.yaml` 语法具体细节查看[Overview | Docker Docs](https://docs.docker.com/compose/compose-file/)

## 3.3 Watch

在使用Docker进行开发时，您可能需要在编辑和保存代码时自动更新和预览正在运行的服务。您可以为此使用Docker Compose Watch。

在编辑和保存代码时，使用 [watch](https://docs.docker.com/compose/file-watch/) 自动更新和预览正在运行的Compose服务。

```
docker compose watch
```

## 3.4 简单案例

>  案例教程 [Run multi-container applications | Docker Docs](https://docs.docker.com/guides/walkthroughs/multi-container-apps/)

```
# Comments are provided throughout this file to help you get started.
# If you need more help, visit the Docker compose reference guide at
# https://docs.docker.com/compose/compose-file/

# Here the instructions define your application as two services called "todo-app" and “todo-database”
# The service “todo-app” is built from the Dockerfile in the /app directory,
# and the service “todo-database” uses the official MongoDB image 
# from Docker Hub - https://hub.docker.com/_/mongo. 
# You can add other services your application may depend on here.

services:
  todo-app:
    build:
      context: ./app
    depends_on:
      - todo-database
    environment:
      NODE_ENV: production
    ports:
      - 3000:3000
      - 35729:35729
    develop:
      watch:
        - path: ./app/package.json
          action: rebuild
        - path: ./app
          target: /usr/src/app
          action: sync

  todo-database:
    image: mongo:6
    #volumes: 
    #  - database:/data/db
    ports:
      - 27017:27017

#volumes:
  #database:
```

运行以下指令

```
docker compose up -d
```

应用程序堆栈里面存在两个容器 todo-database 和 todo-app



# 四、容器化应用程序

使用容器时，您通常需要创建一个 Dockerfile 来定义您的镜像，并创建一个 compose. yaml 文件来定义如何运行它。

```
docker run
```

```
docker compose up
```



## 4.1 docker run

在[How do I run a container? | Docker Docs ](https://docs.docker.com/guides/walkthroughs/run-a-container/#step-2-view-the-dockerfile-in-your-project-folder)的https://github.com/docker/welcome-to-docker 代码里，要在容器中运行代码，您需要的最基本的东西是Dockerfile。Dockerfile描述了进入容器的内容。此示例已包含Dockerfile。对于您自己的项目，您需要创建自己的Dockerfile。您可以在代码或文本编辑器中打开Dockerfile并浏览其内容。

```
# Start your image with a node base image
FROM node:18-alpine

# The /app directory should act as the main application directory
WORKDIR /app

# Copy the app package and package-lock.json file
COPY package*.json ./

# Copy local directories to the current local directory of our docker image (/app)
COPY ./src ./src
COPY ./public ./public

# Install node packages, install serve, build the app, and remove dependencies at the end
RUN npm install \
    && npm install -g serve \
    && npm run build \
    && rm -fr node_modules

EXPOSE 3000

# Start the app using serve command
CMD [ "serve", "-s", "build" ]
```





## 4.2 docker compose up

案例教程 [Run multi-container applications | Docker Docs](https://docs.docker.com/guides/walkthroughs/multi-container-apps/)





# 五、发布和共享镜像

[Publish your image | Docker Docs](https://docs.docker.com/guides/walkthroughs/publish-your-image/)



在发布映像之前，您需要重命名它，以便Docker Hub知道该映像是您的。在终端中，运行以下命令来重命名您的映像。将 `YOUR-USERNAME` 替换为您的Docker ID。

```
docker tag docker/welcome-to-docker YOUR-USERNAME/welcome-to-docker
```

然后，push to Hub 即可





# 参考链接

[Overview of Docker Build | Docker Docs](https://docs.docker.com/build/)

[Dockerfile reference⁠](https://docs.docker.com/engine/reference/builder/)

 [Compose file reference](https://docs.docker.com/compose/compose-file/)