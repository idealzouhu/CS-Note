### Redis 客户端

Jedis 和 Redisson 都是 Java 中用于操作 Redis 的客户端库，它们提供了对 Redis 数据库的访问和操作接口，但它们的设计和实现有所不同。

- **Jedis**:
  - Jedis 是 Redis 官方推荐的 Java 客户端库之一，基于 Java 实现，使用简单，性能较高。
  - Jedis 是一个直接连接 Redis 服务器的库，每个线程都会获取自己的连接，操作时需要手动管理连接的获取和释放，不支持异步操作。
  - Jedis 需要手动处理连接的管理，如连接池的管理、异常处理等。
- **Redisson**:
  - Redisson 是一个基于 Redis 完全异步和反应式的客户端库，提供了丰富的功能和高级特性，如分布式对象、分布式集合、分布式锁、分布式限流等。
  - Redisson 提供了更加简洁的 API，支持异步操作和响应式编程风格，可以更方便地进行并发编程。
  - Redisson 内置了连接池和线程池管理，可以自动处理连接的获取和释放，减轻了开发者的负担。





### 为什么 Redis 这么快

- 内存
- Redis 基于 Reactor 模式设计开发了一套高效的**事件处理模型**，主要是单线程事件循环和 **IO 多路复用**（Redis 线程模式后面会详细介绍到）；
- 优化的数据类型



### Redis 怎么实现 I/O 多路复用

这里“多路”指的是多个网络连接客户端,，“复用“指的是复用同一个线程(单进程)。  I/O 多路复用其实是使用一个线程来检査多个 Socket 的就绪状态，在单个线程中通过记录跟踪每一个socket( I/O 流)的状态来管理处理多个 I/O 流。

![img](images/1720433058791-94f03cb5-e89c-45ed-ba34-88a0dac99d98.png)





### redis 和 mysql 并发量

如果缓存命中的话，4 核心 8g 内存的配置，redis 可以支撑 10w 的 qps

如果缓存没有命中的话，4 核心 8g 内存的配置，mysql 只能支持 5000 左右的 qps







### Redis 使用依赖

如果只是需要基础的 Redis 功能（如缓存、数据存储、发布/订阅等），可以使用 **`spring-boot-starter-data-redis`**。

如果项目中需要高级的 Redis 功能，特别是分布式锁、分布式集合等场景，建议使用 **`redisson-spring-boot-starter`**，因为它提供了更加丰富的功能。

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<!-- https://mvnrepository.com/artifact/org.redisson/redisson-spring-boot-starter -->
<dependency>
    <groupId>org.redisson</groupId>
    <artifactId>redisson-spring-boot-starter</artifactId>
    <version>3.12.0</version>
</dependency>
```







### 参考资料

[Redis 为什么这么快？](https://javaguide.cn/database/redis/redis-questions-01.html#redis-为什么这么快)