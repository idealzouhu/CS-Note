## 分布式锁概述

### 什么是 Redis 分布式锁

由于 Redis 本身可以被多个客户端共享访问，Redis 可以用来保存分布式锁，实现分布式系统中的互斥锁机制。

![img](images/1719108636409-6ff3328a-f0fb-4028-82ee-059b4f548a8a.webp)

<font color="red">Redis 通常是通过键（key）来管理锁。</font> 

```java
RLock lock = redisson.getLock("myLock");

try {   
	lock.lock();
	// TODO: 执行业务逻辑
} finally {
	lock.unlock();
}
```



### Redis 加锁

加锁核心命令是 `SETNX`（SET if Not Exists），它保证在键不存在时设置键值，并返回 `true`，如果键已经存在，则返回 `false`。

举例如下：

```shell
SET lock_key unique_value  NX EX 10
```

- **lock_key**：锁的键名称。这个键代表分布式锁，通常是一个唯一标识锁的字符串。
- **unique_value**：锁的值，通常是一个唯一的随机值（比如 UUID 或客户端标识）。这个值的作用是标识具体哪个客户端拥有该锁。它用于保证锁的唯一性，并防止其他客户端意外释放锁。
- **NX**（Not Exist）：表示**只有在键不存在时才进行设置**，避免其他客户端抢占锁
- **EX 10**（Expire 10 seconds）：设置该键的过期时间为 10 秒。如果客户端在 10 秒内没有完成任务，锁将自动失效，其他客户端可以获取到该锁。设置过期时间的目的是为了锁一直无法释放



### Redis 释放锁

**锁释放操作可以通过 Lua 脚本原子性地进行**：先检查锁的值是否为客户端自己创建的值，如果是，则删除锁，确保锁的释放操作不会被打断。

Lua 脚本内容如下：

```lua
// 检查锁的值是否与 unique_value 匹配，如果匹配，释放锁
if redis.call("GET", KEYS[1]) == ARGV[1] then
    return redis.call("DEL", KEYS[1])
else
    return 0
end
```



使用 `EVAL` 命令来执行 Lua 脚本， 将  `lock_key` 作为键传递，并将 `unique_value` 作为参数传递：

```bash
EVAL "script" 1 lock_key unique_value
```

在 Lua 脚本中，`KEYS[1]`  对应 `lock_key`，  `ARGV[1]`  对应  `unique_value`



## Redission 具体设计

Redission 是基于 Redis 实现的 Java 解决方案，可以用于实现分布式锁。

Rediison 分布式锁特性：

- 可重入
- 自动续期



### 锁自动续期

为什么要自动续期锁？ 如果操作共享资源的线程还未执行完成的话，我们需要不断地延长锁的过期时间，进而保证锁不会因为超时而被释放。

当未指定锁超时时间时，Watch Dog 自动续期机制将会启用。



[分布式锁常见实现方案总结 | JavaGuide](https://javaguide.cn/distributed-system/distributed-lock-implementations.html#如何实现锁的优雅续期)



### 保证集群情况下分布式锁的可靠性

在 Redis Cluster 中，master 节点已经获取锁。如果此时 master 节点发生故障，slave 节点被选举为 master 节点，此时新的 master 节点依然可以获取锁。

<img src="images/redis-master-slave-distributed-lock.png" alt="img" style="zoom:80%;" />



针对这一问题，Redis 之父 antirez 设计了 [Redlock](https://redis.io/docs/latest/develop/use/patterns/distributed-locks/#the-redlock-algorithm)来解决。Redlock 算法的思想是让客户端向 Redis 集群中的多个独立的 Redis 实例依次请求申请加锁，如果客户端能够和半数以上的实例成功地完成加锁操作，那么我们就认为，客户端成功地获得分布式锁，否则加锁失败。

即使部分 Redis 节点出现问题，只要保证 Redis 集群中有半数以上的 Redis 节点可用，分布式锁服务就是正常的。

为了获取锁，客户端执行以下操作：

1. **获取当前时间（毫秒）：** 客户端首先获取当前的时间戳，单位是毫秒，用于计算锁的有效时间。
2. **依次在所有 N 个实例上尝试获取锁：** 客户端在所有 Redis 实例上依次尝试获取锁，使用相同的键名和随机值。在每个实例上设置锁时，客户端使用的超时时间应比锁的自动释放时间小。例如，如果锁的自动释放时间为 10 秒，超时时间可以设置在 5 到 50 毫秒之间。这可以避免客户端在与某个宕机的 Redis 节点通信时长时间被阻塞：如果某个实例不可用，客户端应该尽快尝试与下一个实例通信。
3. **计算获取锁所用的时间：** 客户端通过从当前时间减去步骤 1 中获得的时间戳来计算获取锁所用的时间。如果客户端能够在大多数实例（至少 N/2 + 1 个实例）上成功获取锁，并且获取锁所用的时间小于锁的有效时间，那么该锁被认为已成功获取。
4. **锁的有效时间调整：** 如果锁被成功获取，那么锁的有效时间将是初始有效时间减去获取锁的时间（即步骤 3 中计算的时间）。
5. **获取锁失败时的处理：** 如果客户端由于某些原因未能成功获取锁（例如，它未能在 N/2 + 1 个实例上获取锁，或计算出的有效时间为负数），客户端将尝试解锁所有实例（即使在它认为未能成功加锁的实例上也会尝试解锁）

其中，由于分布式锁需要在多个实例上成功获取，设置超时时间用来避免长时间阻塞，提高锁获取的效率。+



## 参考资料

[Distributed Locks with Redis | Docs](https://redis.io/docs/latest/develop/use/patterns/distributed-locks/)

[分布式锁常见实现方案总结 | JavaGuide](https://javaguide.cn/distributed-system/distributed-lock-implementations.html#redis-如何解决集群情况下分布式锁的可靠性)

