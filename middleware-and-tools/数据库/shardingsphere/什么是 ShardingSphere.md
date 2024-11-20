## 一、ShardingSphere  概述

### 1.1 什么是 ShardingSphere  

Apache ShardingSphere 是一款分布式 SQL 事务和查询引擎，可通过数据分片、弹性伸缩、加密等能力对任意数据库进行增强。



### 1.2 设计哲学

ShardingSphere 采用 **Database Plus 设计哲学**，该理念致力于构建数据库上层的标准和生态，在生态中补充数据库所缺失的能力。

![Design](images/design_cn-1731997282113.png)



### 1.3 产品功能

| 特性       | 定义                                                         |
| :--------- | :----------------------------------------------------------- |
| 数据分片   | 数据分片，是应对海量数据存储与计算的有效手段。ShardingSphere 基于底层数据库提供分布式数据库解决方案，可以水平扩展计算和存储。 |
| 分布式事务 | 事务能力，是保障数据库完整、安全的关键技术，也是数据库的核心技术。基于 XA 和 BASE 的混合事务引擎，ShardingSphere 提供在独立数据库上的分布式事务功能，保证跨数据源的数据安全。 |
| 读写分离   | 读写分离，是应对高压力业务访问的手段。基于对 SQL 语义理解及对底层数据库拓扑感知能力，ShardingSphere 提供灵活的读写流量拆分和读流量负载均衡。 |
| 数据迁移   | 数据迁移，是打通数据生态的关键能力。ShardingSphere 提供跨数据源的数据迁移能力，并可支持重分片扩展。 |
| 联邦查询   | 联邦查询，是面对复杂数据环境下利用数据的有效手段。ShardingSphere 提供跨数据源的复杂查询分析能力，实现跨源的数据关联与聚合。 |
| 数据加密   | 数据加密，是保证数据安全的基本手段。ShardingSphere 提供完整、透明、安全、低成本的数据加密解决方案。 |
| 影子库     | 在全链路压测场景下，ShardingSphere 支持不同工作负载下的数据隔离，避免测试数据污染生产环境。 |



## 二、ShardingSphere  生态

Apache ShardingSphere 是一款分布式的数据库生态系统，它包含两大产品：

- ShardingSphere-Proxy： 定位为轻量级 Java 框架，在 Java 的 JDBC 层提供的额外服务
- ShardingSphere-JDBC： 定位为透明化的数据库代理端，通过实现数据库二进制协议，对异构语言提供支持。



### 2.1 ShardingSphere-JDBC 

ShardingSphere-JDBC 定位为<font color="blue">轻量级 Java 框架，在 Java 的 JDBC 层提供的额外服务</font>。它使用客户端直连数据库，以 jar 包形式提供服务，无需额外部署和依赖，**可理解为增强版的 JDBC 驱动**，完全兼容 JDBC 和各种 ORM 框架。

- 适用于任何基于 JDBC 的 ORM 框架，如：JPA, Hibernate, Mybatis, Spring JDBC Template 或直接使用 JDBC；
- 支持任何第三方的数据库连接池，如：DBCP, C3P0, BoneCP, HikariCP 等；
- 支持任意实现 JDBC 规范的数据库，目前支持 MySQL，PostgreSQL，Oracle，SQLServer 以及任何可使用 JDBC 访问的数据库。

<img src="../../../../java-demos/middleware-demos/spring-boot-shardingsphere/shardingsphere-jdbc/src/main/resources/docs/images/shardingsphere-jdbc_v3.png" alt="img" style="zoom:80%;" />ShardingSphere-JDBC 提供的是在 **JDBC 层之上的增强功能**（如分库分表、读写分离、数据加密等）。它不包含具体数据库的连接实现，而是依赖底层的原生 JDBC 驱动去与数据库通信。

### 2.2 ShardingSphere-Proxy

ShardingSphere-Proxy 定位为<font color="blue">透明化的数据库代理端，通过实现数据库二进制协议，对异构语言提供支持</font>。 目前提供 MySQL 和 PostgreSQL 协议，透明化数据库操作，对 DBA 更加友好。

- 向应用程序完全透明，可直接当做 MySQL/PostgreSQL 使用；
- 兼容 MariaDB 等基于 MySQL 协议的数据库，以及 openGauss 等基于 PostgreSQL 协议的数据库；
- 适用于任何兼容 MySQL/PostgreSQL 协议的的客户端，如：MySQL Command Client, MySQL Workbench, Navicat 等。

<img src="../../../../java-demos/middleware-demos/spring-boot-shardingsphere/shardingsphere-jdbc/src/main/resources/docs/images/shardingsphere-proxy_v2.png" alt="img" style="zoom:80%;" />

### 2.3 不同产品的区别

|            | ShardingSphere-JDBC | ShardingSphere-Proxy |
| :--------- | :------------------ | -------------------- |
| 数据库     | 任意                | MySQL/PostgreSQL     |
| 连接消耗数 | 高                  | 低                   |
| 异构语言   | 仅 Java             | 任意                 |
| 性能       | 损耗低              | 损耗略高             |
| 无中心化   | 是                  | 否                   |
| 静态入口   | 无                  | 有                   |





## 参考资料

[概览 :: ShardingSphere](https://shardingsphere.apache.org/document/current/cn/overview/)