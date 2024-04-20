# 一、MySQL 基础架构

> [MySQL常见面试题总结 | JavaGuide](https://javaguide.cn/database/mysql/mysql-questions-01.html#mysql-基础架构)

![img](images/13526879-3037b144ed09eb88.png)

MySQL架构主要分为**Server层**和**[存储引擎层](https://www.zhihu.com/search?q=存储引擎层&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A2579554549})**。

- 服务器层是 MySQL 数据库的核心部分，负责处理客户端的连接、权限认证、SQL 解析、查询优化、执行计划生成、事务管理等功能。
- 存储引擎层负责将数据存储到磁盘上，并提供对数据的 CRUD（创建、读取、更新、删除）操作。



**Server层**又分为[连接器](https://www.zhihu.com/search?q=连接器&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A2579554549})、缓存、分析器、优化器、执行器。所有跨存储引擎的功能都在这层实现，比如：函数、[存储过程](https://www.zhihu.com/search?q=存储过程&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A2579554549})、触发器、视图等。

**存储引擎**是可插拔式的，常见的存储引擎有MyISAM、InnoDB、Memory等，MySQL5.5之前默认的是MyISAM，之后默认的是InnoDB。







