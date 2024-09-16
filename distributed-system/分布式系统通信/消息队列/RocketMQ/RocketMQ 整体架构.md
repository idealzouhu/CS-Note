### RocketMQ 集群部署模型

Broker 和 NameServer 是 Apache RocketMQ 中的两个重要组件

在部署时，作为两个容器来分开部署的

![RocketMQ部署架构](images/RocketMQ部署架构-ee0435f80da5faecf47bca69b1c831cb.png)



- `Broker` **做了集群并且还进行了主从部署**
- **单个 Broker 和所有 NameServer 保持长连接**







### Proxy 

引入 Proxy 模块后，Proxy 承担了协议适配、权限管理、消息管理等计算功能，Broker 则更加专注于存储。这样存储和计算相分离，在云原生环境下可以更好地进行资源调度。

![img](images/057972e1b2cd15490817ee2431c32ea2.png)



### 参考资料

[初识RocketMQ | RocketMQ4.x (apache.org)](https://rocketmq.apache.org/zh/docs/4.x/introduction/02whatis)

[一张图进阶 RocketMQ -整体架构 - 掘金 (juejin.cn)](https://juejin.cn/post/7109262212350345224)

[RocketMQ 5.0 时代，6 张图带你理解 Proxy！_rocketmq proxy-CSDN博客](https://blog.csdn.net/m0_73311735/article/details/130079912)