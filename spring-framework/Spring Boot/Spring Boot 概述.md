### 什么是 Spring Boot

Spring Boot 是由 Pivotal 团队提供的全新框架，其设计目的是用来简化 Spring 应用的初始搭建以及开发过程。该框架使用了特定的方式来进行配置，从而使开发人员不再需要定义样板化的配置。

Spring Boot 其实不是什么新的框架，它默认配置了很多框架的使用方式，就像 Maven 整合了所有的 Jar 包，Spring Boot 整合了所有的框架。

从本质上来说，Spring Boot 就是 Spring，它做了那些没有它你自己也会去做的Spring Bean配置。



## Spring Boot 在 Spring 的基础上所作的改进

Spring Boot 是基于 Spring 的项目，其中最重要的核心有：

- 自动化配置：Spring Boot 使用 ”约定大于配置 “ 的思想，根据项目的依赖和环境自动配置应用程序
- 起步依赖 ：起步依赖帮助你专注于应用程序需要的功能类型，而非提供该功能的具体库和版本。 Spring Boot起步依赖基本都以spring-boot-starter打头，随后是直接代表其功能的名字，比如web、test。
- 命令行界面
- Actutaor





## 优缺点

#### 1）优点

- 快速构建项目。
- 对主流开发框架的无配置集成。
- 项目可独立运行，无须外部依赖Servlet容器。
- 提供运行时的应用监控。
- 极大地提高了开发、部署效率。
- 与云计算的天然集成。

#### 2）缺点

- 版本迭代速度很快，一些模块改动很大。
- 由于不用自己做配置，报错时很难定位。
- 网上现成的解决方案比较少。





### Spring Boot 集成的 starter

[Build Systems :: Spring Boot](https://docs.spring.io/spring-boot/reference/using/build-systems.html#using.build-systems.starters)

比较常用的有:

- spring-boot-starter-web

- spring-boot-starter-aop
- spring-boot-starter-data-redis
- spring-boot-starter-jdbc
- 