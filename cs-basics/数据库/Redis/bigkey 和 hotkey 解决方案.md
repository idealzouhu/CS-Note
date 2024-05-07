|          | bigkey                       | hotkey             |
| -------- | ---------------------------- | ------------------ |
| 解决方案 | 切割 bigkey                  | 读写分离           |
|          | 手动清理                     | 二级缓存           |
|          | 将数据转换成更合理的数据结构 | 使用 Redis Cluster |





## 大key 





![img](images/1698769281434-ff0a9b27-98bd-4d6a-ac40-1de0163d57f4.jpeg)





## 热 key

- 读写分离

- 二级缓存

- **使用 Redis Cluster**：将热点数据分散存储在多个 Redis 节点上。







## 参考资料

[Redis常见面试题总结(下) | JavaGuide](https://javaguide.cn/database/redis/redis-questions-02.html#redis-hotkey-热-key)

[如何解决大Key问题？ | 拿个offer - 开源&项目实战 (nageoffer.com)](https://nageoffer.com/pages/54ed17/#_2-业务层面)

[如何解决热Key问题？ | 拿个offer - 开源&项目实战 (nageoffer.com)](https://nageoffer.com/pages/88edcc/#回答话术)