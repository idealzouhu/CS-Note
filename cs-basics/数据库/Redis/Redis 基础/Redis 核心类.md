## RedisTemplate

**RedisTemplate：** RedisTemplate 是  Spring Data Redis 提供的 Redis 客户端，用于在 Spring 应用程序中与 Redis 进行交互。

它提供了一系列的方法来执行 Redis 的各种操作，如存储、获取、删除、执行 Lua 脚本等。RedisTemplate 支持泛型，可以方便地处理不同类型的数据。

RedisTemplate 是基于模板模式设计的，更贴近于底层的 Redis 命令，适合于与 Spring 框架集成的应用程序。

[RedisTemplate常用方法总结_redistemplate range-CSDN博客](https://blog.csdn.net/sinat_22797429/article/details/89196933)



## RedissonClient

**RedissonClient** 是 Redisson 客户端的核心接口，提供了丰富的功能和特性，例如分布式对象、分布式锁、分布式集合等。Redisson 客户端的设计更加注重面向对象的编程风格，提供了更高层次的抽象和更便捷的 API，适合于构建分布式应用程序。





## RedissonClient  和 RedisTemplate 区别

`RedissonClient` 更适合于构建分布式应用程序，提供了丰富的高级功能和面向对象的编程风格；

 `RedisTemplate` 更适合于与 Spring 框架集成的应用程序，提供了基本的 Redis 操作方法和一些高级功能的支持。选择使用哪种客户端取决于具体的项目需求和技术栈。





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