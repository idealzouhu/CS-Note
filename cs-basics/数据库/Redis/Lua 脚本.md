## 一、Lua 脚本概述

### 什么是 Lua 脚本

Lua（发音为"loo-ah"，意为"月亮"）是一种轻量级的脚本语言，用**标准C语言编写**并以源代码形式开放， 其设计目的是为了嵌入应用程序中，从而为应用程序提供灵活的扩展和定制功能。

Lua 由巴西里约热内卢天主教大学（Pontifical Catholic University of Rio de Janeiro）的一个研究小组于1993年创建，现已成为一种通用的脚本语言，被广泛应用于游戏开发、嵌入式系统、服务器端编程等领域。

[Lua 变量 | 菜鸟教程 (runoob.com)](https://www.runoob.com/lua/lua-variables.html)



### Lua 脚本可能存在的问题

- 在 Redis 集群中，Lua 脚本无法保证所有的 key 都在同一个 hash lot(哈希槽)中
- 如果 Lua 脚本运行时出错并中途结束，之后的操作不会进行，但是之前已经发生的写操作不会撤销，所以即使使用了 Lua 脚本，也不能实现类似数据库回滚的原子性。



### Lua 脚本和 pipeline 的区别

pipeline **不适用于执行顺序有依赖关系**的一批命令。就比如说，你需要将前一个命令的结果给后续的命令使用，pipeline 就没办法满足你的需求了。对于这种需求，我们可以使用 **Lua 脚本** 





## 二、Lua 脚本在 Redis 中的使用

从 Redis 2.6.0 版本开始，Lua 脚本被用于实现复杂的原子操作和事务，以及对多个命令的批量执行。Redis 提供了 `EVAL` 和 `EVALSHA` 命令来执行 Lua 脚本。

在 Redis 中，Lua 脚本本身是**不支持回滚**的。因此，如果需要类似于事务回滚的功能，需要在 Lua 脚本内部进行处理。例如，在 Lua 脚本中可以使用条件判断、错误处理等机制来控制执行流程，并在发生错误时手动撤销之前的操作。



### 2.1 利用 Spring Data Redis 使用脚本

（1）  `DefaultRedisScript` 是 Spring Data Redis 提供的一个类，用于**表示 Redis 中的 Lua 脚本**。它封装了 Lua 脚本的内容以及执行结果的类型，并提供了一些方法来设置脚本和获取结果。

```java
// 使用单例模式获取到 Redis 执行的 Lua 脚本
DefaultRedisScript<Long> actual = Singleton.get(LUA_TICKET_AVAILABILITY_TOKEN_BUCKET_PATH, () -> {
    DefaultRedisScript<Long> redisScript = new DefaultRedisScript<>();
    redisScript.setScriptSource(new ResourceScriptSource(new ClassPathResource(LUA_TICKET_AVAILABILITY_TOKEN_BUCKET_PATH)));
    redisScript.setResultType(Long.class);
    return redisScript;
});
```

（2）然后，使用 `stringRedisTemplate` 执行了一个 Redis Lua 脚本，并获取了执行结果 `result`。

```java
// 执行 Redis Lua 脚本
Long result = stringRedisTemplate.execute(actual, Lists.newArrayList(actualHashKey, luaScriptKey), JSON.toJSONString(seatTypeCountArray), JSON.toJSONString(takeoutRouteDTOList));

// 如果执行结果不为 null 且等于 0，则返回 true，表示执行成功；
return result != null && Objects.equals(result, 0L);
```





### Lua 脚本实践案例

当你在 Redis 中执行 Lua 脚本时，可以通过 `EVAL` 命令来调用脚本，并且需要明确地传递 `KEYS` 和 `ARGV` 参数。比如：

```bash
EVAL "redis.call('HMSET', KEYS[1], unpack(ARGV, 1, #ARGV - 1)); redis.call('EXPIREAT', KEYS[1], ARGV[#ARGV])" 1 myhash field1 value1 field2 value2 1690992000
```

在这个例子中：

- `1` 表示传入了 1 个键。
- `KEYS[1] = "myhash"`
- `ARGV = ["field1", "value1", "field2", "value2", 1690992000]` （其中 `1690992000` 是 Unix 时间戳）
- `#ARGV`：表示 `ARGV` 数组的元素个数
- `redis.call()`：在 Lua 脚本中调用 Redis 命令，这里调用的是 `HMSET` 命令。





对应的 Java 代码为：

```java
  String luaScript = "redis.call('HMSET', KEYS[1], unpack(ARGV, 1, #ARGV - 1)) " +
                "redis.call('EXPIREAT', KEYS[1], ARGV[#ARGV])";

        List<String> keys = Collections.singletonList(couponTemplateCacheKey);
        List<String> args = new ArrayList<>(actualCacheTargetMap.size() * 2 + 1);
        actualCacheTargetMap.forEach((key, value) -> {
            args.add(key);
            args.add(value);
        });

        // 优惠券活动过期时间转换为秒级别的 Unix 时间戳
        args.add(String.valueOf(couponTemplateDO.getValidEndTime().getTime() / 1000));

        // 执行 LUA 脚本
        stringRedisTemplate.execute(
                new DefaultRedisScript<>(luaScript, Long.class),
                keys,
                args.toArray()
        );
```





## 细节补充

### Lua 脚本执行失败会怎样（有点疑惑）

[【Java面试】为什么Lua脚本可以保证原子性？_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1nA4m1A7Gn/?spm_id_from=333.788&vd_source=52cd9a9deff2e511c87ff028e3bb01d2)

如果 Lua 脚本执行期间，发生运行时错误，会出现以下情况：

- **错误信息返回：** Redis 会将 Lua 脚本执行过程中的错误信息返回给客户端。这样客户端就可以根据返回的错误信息进行相应的处理。
- **影响后续操作：** 如果 Lua 脚本执行失败，后续的操作将不会执行。Redis 不会执行脚本中发生错误之后的命令。









### 使用过程中存在的问题

错误案例：

```bash
$ GET test

$ DEL test

$ EVAL "local key = KEYS[1]; local value = ARGV[1]; local expire_time_ms = ARGV[2]; local result = redis.call('SET', key, value, 'NX', 'PX', expire_time_ms); if result == 'OK' then return 'ok1' else return 'ok2' end" 1 test testValue 60000
"ok2"
```

理论上来说， 应该返回 ‘ok1’



正确案例

```bash
$ GET test

$ DEL test

$ EVAL "local key = KEYS[1]; local value = ARGV[1]; local expire_time_ms = ARGV[2]; if redis.call('SET', key, value, 'NX', 'PX', expire_time_ms) then return 'ok1' else return 'ok2' end" 1 test testValue 60000
"ok1"

$ EVAL "local key = KEYS[1]; local value = ARGV[1]; local expire_time_ms = ARGV[2]; if redis.call('SET', key, value, 'NX', 'PX', expire_time_ms) then return 'ok1' else return 'ok2' end" 1 test testValue 60000
"ok2"
```

