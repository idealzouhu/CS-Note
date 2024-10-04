### 什么是 canal

[canal](https://github.com/alibaba/canal) 主要用途是<font color="red">基于 MySQL 数据库增量日志解析，提供增量数据订阅和消费</font>。

基于日志增量订阅和消费的业务包括

- 数据库镜像
- 数据库实时备份
- 索引构建和实时维护(拆分异构索引、倒排索引等)
- 业务 cache 刷新
- 带业务逻辑的增量数据处理

![img](images/68747470733a2f2f696d672d626c6f672e6373646e696d672e636e2f32303139313130343130313733353934372e706e67)



### 工作原理

canal 的工作原理主要是将自己伪装成 

#### MySQL主备复制原理

- MySQL master 将数据变更写入二进制日志( binary log, 其中记录叫做二进制日志事件binary log events，可以通过 show binlog events 进行查看)
- MySQL slave 将 master 的 binary log events 拷贝到它的中继日志(relay log)
- MySQL slave 重放 relay log 中事件，将数据变更反映它自己的数据

#### canal 工作原理

- canal 模拟 MySQL slave 的交互协议，伪装自己为 MySQL slave ，向 MySQL master 发送dump 协议
- MySQL master 收到 dump 请求，开始推送 binary log 给 slave (即 canal )
- canal 解析 binary log 对象(原始为 byte 流)







### 组件

[canal-admin](https://github.com/alibaba/canal/wiki/Canal-Admin-Guide) 设计上是为canal提供整体配置管理、节点运维等面向运维的功能，提供相对友好的WebUI操作界面，方便更多用户快速和安全的操作

canal-admin的核心模型主要有：

1. instance，对应canal-server里的instance，一个最小的订阅mysql的队列
2. server，对应canal-server，一个server里可以包含多个instance
3. 集群，对应一组canal-server，组合在一起面向高可用HA的运维