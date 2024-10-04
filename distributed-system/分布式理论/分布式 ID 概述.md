### 常见方案

[9种 分布式ID生成方案，让你一次学个够 - Java进阶课 - SegmentFault 思否](https://segmentfault.com/a/1190000022717820) 介绍了常用的分布式 ID 的生成方案

- UUID。JDK 提供了现成的 API。

- ##### 基于数据库的号段模式。  开源的实现有 滴滴（Tinyid）、美团（Leaf）

- 基于 Redis的模式。 原理就是使用 `redis` 的  `incr` 命令实现ID的原子性自增

- ##### 基于雪花算法（Snowflake）模式。在 twitter 公式开源后，各大公式也分别开源了各具特色的分布式生成器。







### 方案分类

[什么是分布式ID，9种分布式ID的实现方式-电子发烧友网 (elecfans.com)](https://m.elecfans.com/article/2313649.html)

客户端模式

中心化模式

客户端 + 中心化模式