### 主题 topic

使用 mqadmin 创建主题：

```shell
sh mqadmin updateTopic -n <nameserver_address> -t <topic_name> -c <cluster_name> -a +message.type=<message_type>
```

其中，

- `updateTopic`：  `mqadmin` 工具的一个子命令，用来**更新**某个 `Topic` 的配置。
- `-n <nameserver_address>`： `<nameserver_address>` 为 `NameServer` 的地址
- `-t <topic_name>`：`<topic_name>` 表示要操作的 `Topic` 的名称。
- `-c <cluster_name>`： 实际使用时需要填写 RocketMQ 集群的名称
- `-a +message_type` ：根据需要传递的附加参数。 其中， `message_type` 根据消息类型设置成 `[UNSPECIFIED, TRANSACTION, FIFO, MIXED, DELAY, NORMAL]` 中的一种。如果不设置，默认为 `Normal` 类型。











### 参考资料

[Admin Tool | RocketMQ (apache.org)](https://rocketmq.apache.org/zh/docs/deploymentOperations/02admintool)