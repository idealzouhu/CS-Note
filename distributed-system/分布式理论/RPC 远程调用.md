[RPC基础知识总结 | JavaGuide](https://javaguide.cn/distributed-system/rpc/rpc-intro.html#rpc-的原理是什么)



## RPC 框架

gRPC 是实现 RPC  的一款协议， **底层是用 HTTP 2.0 来实现的**





## HTTP 协议和 RPC 的区别

[有了 HTTP 协议，为什么还要有 RPC ？ | JavaGuide](https://javaguide.cn/distributed-system/rpc/http_rpc.html)



- 从发展历史来说，**HTTP 主要用于 B/S 架构，而 RPC 更多用于 C/S 架构。但现在其实已经没分那么清了，B/S 和 C/S 在慢慢融合。** 很多软件同时支持多端，所以**对外一般用 HTTP 协议，而内部集群的微服务之间则采用 RPC 协议进行通讯**。

- RPC 不是协议，而是调用方式。gPRC 才是协议

  