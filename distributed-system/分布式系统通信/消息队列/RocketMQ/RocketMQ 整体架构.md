### RocketMQ 领域模型

![领域模型](images/mainarchi-9b036e7ff5133d050950f25838367a17.png)





#### 消费者

根据消息 push 和 pull 的方式，消费者可以分为  PushConsumer 、SimpleConsumer 和 PullConsumer。 [消费者分类和不同的使用场景 | RocketMQ (apache.org)](https://rocketmq.apache.org/zh/docs/featureBehavior/06consumertype/)







#### 消息

消息分为不同的类型，。每种类型的消息都有相应的应用场景和生命周期。

若消费者只需要关注部分消息，可通过设置过滤条件在 Apache RocketMQ 服务端进行过滤，只获取到需要关注的消息子集，避免接收到大量无效的消息。[消息过滤 | RocketMQ (apache.org)](https://rocketmq.apache.org/zh/docs/featureBehavior/07messagefilter)





### RocketMQ 集群部署模型

Broker 和 NameServer 是 Apache RocketMQ 中的两个重要组件

在部署时，作为两个容器来分开部署的

![RocketMQ部署架构](images/RocketMQ部署架构-ee0435f80da5faecf47bca69b1c831cb.png)





### Proxy 

引入 Proxy 模块后，Proxy 承担了协议适配、权限管理、消息管理等计算功能，Broker 则更加专注于存储。这样存储和计算相分离，在云原生环境下可以更好地进行资源调度。

![img](images/057972e1b2cd15490817ee2431c32ea2.png)



### 参考资料

[初识RocketMQ | RocketMQ4.x (apache.org)](https://rocketmq.apache.org/zh/docs/4.x/introduction/02whatis)

[一张图进阶 RocketMQ -整体架构 - 掘金 (juejin.cn)](https://juejin.cn/post/7109262212350345224)

[RocketMQ 5.0 时代，6 张图带你理解 Proxy！_rocketmq proxy-CSDN博客](https://blog.csdn.net/m0_73311735/article/details/130079912)