# 一、Docker 中部署 Nacos

## 1.1 部署 Nacos

首先，从 Docker Hub 下载 

```
docker pull nacos/nacos-server:v2.1.2
```

然后，在 Docker 容器中运行：

```shell
# Linux 系统
docker run \
-d -p 8848:8848 \
-p 9848:9848 \
--name nacos2 \
-e MODE=standalone \
-e TIME_ZONE='Asia/Shanghai' \
nacos/nacos-server:v2.1.2

# Windows 系统 （cmd里面运行，Windows PowerShell里面运行不了）
docker run ^
-d -p 8848:8848 ^
-p 9848:9848 ^
--name nacos2 ^
-e MODE=standalone ^
-e TIME_ZONE='Asia/Shanghai' ^
nacos/nacos-server:v2.1.2
```

相关参数含义解释：
| 参数                           | 含义                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| `docker run`                   | 运行容器命令                                                 |
| `-d`                           | 在后台运行容器                                               |
| `-p 8848:8848`                 | 将宿主机的 8848 端口映射到容器内的 8848 端口                 |
| `-p 9848:9848`                 | 将宿主机的 9848 端口映射到容器内的 9848 端口                 |
| `--name nacos2`                | 指定容器的名称为 `nacos2`                                    |
| `-e MODE=standalone`           | 设置环境变量 `MODE` 为 `standalone`，指定 Nacos 的运行模式为独立模式 |
| `-e TIME_ZONE='Asia/Shanghai'` | 设置环境变量 `TIME_ZONE` 为 'Asia/Shanghai'，指定容器的时区为上海时区 |
| `nacos/nacos-server:v2.1.2`    | 指定要使用的 Docker 镜像，即 Nacos 服务器的镜像版本为 v2.1.2 |



## 1.2 运行

运行成功，稍等几秒启动时间，浏览器输入 http://localhost:8848/nacos/index.html 查看。在默认情况下，Nacos 的用户名为 `nacos`，密码为 `nacos`

