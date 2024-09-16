### Mysql 镜像默认数据方式

Mysql 镜像默认让 Docker 自动管理用户创建的数据。在 mysql 镜像的 [Dockfile ](https://github.com/docker-library/mysql/blob/ea8ec8343c4540ac52e8bba94b5a531e298ff700/8.4/Dockerfile.oracle) 中， 使用了  `VOLUME /var/lib/mysql`。Dockfile 部分内容如下：

```
FROM oraclelinux:9-sli

....
VOLUME /var/lib/mysql

COPY docker-entrypoint.sh /usr/local/bin/
ENTRYPOINT ["docker-entrypoint.sh"]

EXPOSE 3306 33060
CMD ["mysqld"]
```



启动 mysql 镜像后，我们也可以查看容器的挂载信息

```bash
# Windows 运行镜像
$ docker run --name mysql ^
-p 3306:3306 ^
-e MYSQL_ROOT_HOST=% ^
-e MYSQL_ROOT_PASSWORD=root ^
-d mysql:5.7.36

# 查看配置
$ docker inspect mysql --format "{{json .Mounts}}"
[{"Type":"volume","Name":"2885c561144dd3c20bac47ea870e001f2d932f0496c4be01d35cf45faaf43a0a","Source":"/var/lib/docker/volumes/2885c561144dd3c20bac47ea870e001f2d932f0496c4be01d35cf45faaf43a0a/_data","Destination":"/var/lib/mysql","Driver":"local","Mode":"","RW":true,"Propagation":""}]
```

部分内容的含义如下：

- `Source` 表示宿主机上 Volume 存储数据的路径。

- `Destination` 表示容器内的挂载点（对于 MySQL 一般是 `/var/lib/mysql`）。



### 为什么 docker commit 无法持久化数据

`docker commit`是不会把我们在容器中修改的数据一起打包成镜像的。

docker 官方文档对 VOLUME 这个关键词做了解释，这个路径下的数据或者修改，再重新commit 一个新的镜像时，该路径下的所有变化都会被discard，即所有的修改都会被抛弃，不会被保存到新镜像中。



在 docker commit 的官方文档中，



因此，在你运行 `docker commit` 时，容器的文件系统会被保存，但**挂载到 Volume 中的数据**并没有被包含在镜像中。这是因为 Docker 的卷 (Volume) 是独立于容器和镜像管理的，数据被存储在宿主机的文件系统中，而不是容器的文件系统。





### 如何持久化数据

如果我们不想让 Docker 容器自动管理我们创建的数据，我们可以选择其他的方式来存储数据：





#### 方式一：使用 docker 的挂载

在主机上创建目录 `/my/local/mysql_data`。(每个人建一个自己的目录，拷贝一份基础数据，这样互不影响，各写各的)，

在docker run的时候使用-v参数将宿主机上的目录映射到容器里的/var/lib/mysql。

```
docker run --name mysql \
-p 3306:3306 \
-v /my/local/mysql_data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root \
-d mysql:5.7.36
```

这样，MySQL 的数据会保存在 `/my/local/mysql_data` 目录中，无论你重启容器还是删除容器，数据都会保留。



注意：在还没有向 mysql 容器里面导入数据的时候， `/my/local/mysql_data` 已经有 200 MB 了



方式二：

如果你**不想使用 Volume**，可以手动将数据复制到容器文件系统，再打包成镜像。















## 参考资料

[mysql - Official Image | Docker Hub](https://hub.docker.com/_/mysql)



[mysql 镜像数据_mysql官方镜像数据存储问题-CSDN博客](https://blog.csdn.net/weixin_32556315/article/details/114816788)