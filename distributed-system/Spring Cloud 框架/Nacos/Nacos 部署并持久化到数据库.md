### 初始化 mysql 数据库

```
docker run --name mysql ^
-p 3306:3306 ^
-e MYSQL_ROOT_HOST='%' ^
-e MYSQL_ROOT_PASSWORD=root ^
-d mysql:5.7.36
```

在 MySQL 中新建数据库 `nacos-config` ， 然后初始化 [mysql-schema.sql]([nacos/config/src/main/resources/META-INF/mysql-schema.sql at master · alibaba/nacos (github.com)](https://github.com/alibaba/nacos/blob/master/config/src/main/resources/META-INF/mysql-schema.sql))



### 部署 Nacos

```bash
docker run ^
-d -p 8848:8848 ^
-p 9848:9848 ^
--name nacos ^
-e MODE=standalone ^
-e SPRING_DATASOURCE_PLATFORM=mysql ^
-e DB_NUM=1 ^
-e DB_URL_0="jdbc:mysql://host.docker.internal:3306/nacos-config?characterEncoding=utf8&connectTimeout=1000&socketTimeout=30000&autoReconnect=true&useUnicode=true&useSSL=false&serverTimezone=UTC" ^
-e DB_USER=root ^
-e DB_PASSWORD=root ^
-e TIME_ZONE="Asia/Shanghai" ^
nacos/nacos-server:v2.4.2
```

注意， 默认情况下，**nacos 容器和 mysql 容器是在 Docker 网络内部直接通信**。在使用 docker 部署 MySQL 的情况下，MySQL 地址使用 `localhost:3306`，会出现 JDK 报错。 MySQL 地址应该为以下两种情况：

- 使用 `host.docker.internal:3306` ， 这个等于宿主机的 MySQL 服务直接通信， 相当于间接和 Docker 里面的 mysql 容器通信。
- 使用  `mysql:3306` (mysql 是 MySQL 容器在 Docker 里面的 IP 地址)。





### 参考资料

[Nacos支持三种部署模式](https://nacos.io/zh-cn/docs/deployment.html)