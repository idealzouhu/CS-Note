# 一、Lua 脚本概述

Lua（发音为"loo-ah"，意为"月亮"）是一种轻量级的脚本语言，用**标准C语言编写**并以源代码形式开放， 其设计目的是为了嵌入应用程序中，从而为应用程序提供灵活的扩展和定制功能。

Lua 由巴西里约热内卢天主教大学（Pontifical Catholic University of Rio de Janeiro）的一个研究小组于1993年创建，现已成为一种通用的脚本语言，被广泛应用于游戏开发、嵌入式系统、服务器端编程等领域。

[Lua 变量 | 菜鸟教程 (runoob.com)](https://www.runoob.com/lua/lua-variables.html)



# 二、Lua 脚本在 Redis 中的使用

从 Redis 2.6.0 版本开始，Lua 脚本被用于实现复杂的原子操作和事务，以及对多个命令的批量执行。Redis 提供了 `EVAL` 和 `EVALSHA` 命令来执行 Lua 脚本。

在 Redis 中，Lua 脚本本身是**不支持回滚**的。因此，如果需要类似于事务回滚的功能，需要在 Lua 脚本内部进行处理。例如，在 Lua 脚本中可以使用条件判断、错误处理等机制来控制执行流程，并在发生错误时手动撤销之前的操作。



## 2.1 利用 Spring Data Redis 使用脚本

（1）  `DefaultRedisScript` 是 Spring Data Redis 提供的一个类，用于**表示 Redis 中的 Lua 脚本**。它封装了 Lua 脚本的内容以及执行结果的类型，并提供了一些方法来设置脚本和获取结果。

```
// 使用单例模式获取到 Redis 执行的 Lua 脚本
DefaultRedisScript<Long> actual = Singleton.get(LUA_TICKET_AVAILABILITY_TOKEN_BUCKET_PATH, () -> {
    DefaultRedisScript<Long> redisScript = new DefaultRedisScript<>();
    redisScript.setScriptSource(new ResourceScriptSource(new ClassPathResource(LUA_TICKET_AVAILABILITY_TOKEN_BUCKET_PATH)));
    redisScript.setResultType(Long.class);
    return redisScript;
});
```

（2）然后，使用 `stringRedisTemplate` 执行了一个 Redis Lua 脚本，并获取了执行结果 `result`。

```
// 执行 Redis Lua 脚本
Long result = stringRedisTemplate.execute(actual, Lists.newArrayList(actualHashKey, luaScriptKey), JSON.toJSONString(seatTypeCountArray), JSON.toJSONString(takeoutRouteDTOList));

// 如果执行结果不为 null 且等于 0，则返回 true，表示执行成功；
return result != null && Objects.equals(result, 0L);
```







# 细节补充

## 为什么 Lua 脚本可以保证原子性？

主要原因为：

- Redis 命令的执行采用的是单线程模型。
- Redis 会把 Lua 脚本作为一个整体单独执行。





## Lua 脚本执行失败会怎样（有点疑惑）

[【Java面试】为什么Lua脚本可以保证原子性？_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1nA4m1A7Gn/?spm_id_from=333.788&vd_source=52cd9a9deff2e511c87ff028e3bb01d2)

如果 Lua 脚本执行期间，发生运行时错误，会出现以下情况：

- **错误信息返回：** Redis 会将 Lua 脚本执行过程中的错误信息返回给客户端。这样客户端就可以根据返回的错误信息进行相应的处理。
- **影响后续操作：** 如果 Lua 脚本执行失败，后续的操作将不会执行。Redis 不会执行脚本中发生错误之后的命令。





