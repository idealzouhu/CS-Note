### Redis 基础

[Redis 安装和部署](Redis 安装和部署.md) ：给出 Redis 在 windows 、Linux、docker 里面的安装和部署的流程

[Redis 概述](./Redis 概述.md): 介绍 redis 以及 redis 常用功能, Redis 为什么如此快的原因。

[单线程模型.md](单线程模型.md) : 由于网络硬件的升级，Redis 从单线程模型演化为多线程模型。

[基本数据类型](基本数据类型.md) ：常见的 5种基本数据类型及应用场景，ZSet 的底层数据结构及运行效率

[Redis 核心类](Redis 核心类.md) : 处理不同类型的数据的 redisTemplate 类；



### Redis 进阶

[持久化机制](./持久化机制.md)： Redis主要包括 RDB 持久化和 AOF 持久化这两种持久化方式

[内存管理](内存管理.md) ：Redis 使用过期删除策略管理已过期的键值对。但是，在内存不足的时候，Redis 会触发内存淘汰策略。

[Redis 事务](Redis 事务.md) ： Redis 事务不支持回滚，无法保证原子性和持久性。

[Lua 脚本](Lua 脚本.md) ：lua 脚本可以执行批量操作，但是依然不具备原子性。



### 客户端工具

 Redis 可视化桌面终端软件：[Another Redis Desktop Manager](https://github.com/qishibo/AnotherRedisDesktopManager/releases)