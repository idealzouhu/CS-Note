### 什么是 hotkey

hotkey 指访问频率明显高于其他 key 的 key。Hotkey  通常会导致以下问题：

- 单点性能瓶颈
- 不均衡的负载分配
- 潜在的系统宕机





### hotkey 产生原因

**业务逻辑集中**：某些业务操作集中在少数几个键上。例如，热点商品的库存、流量较大的用户数据等。

**不合理的热点数据处理**：对某些重要的数据（如热门文章、产品推荐等）没有合理的分布策略，导致单一键或少量键的数据访问过于集中。



### 怎么发现 hotkey

- **使用 Redis 自带的 `--hotkeys` 参	数来查找**
- **使用`MONITOR` 命令**实时查看所有操作，但对 Redis 的性能影响较大

- **借助开源项目来分析**



## hotkey

- 读写分离：主节点处理写请求，从节点处理读请求
- 二级缓存：将 hotkey 存放一份到 JVM 本地内存中
- **使用 Redis Cluster**：将热点数据分散存储在多个 Redis 节点上。





### 参考资料

[Redis常见面试题总结(下) | JavaGuide](https://javaguide.cn/database/redis/redis-questions-02.html#redis-bigkey-大-key)