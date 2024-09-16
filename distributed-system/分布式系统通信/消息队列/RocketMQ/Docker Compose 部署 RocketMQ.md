### 1. 编写docker-compose

为了快速启动并运行 RockerMQ 集群，创建 `docker-compose.yml` 文件。

```yaml
# 定义该文件的格式版本
version: '3.8'

# 定义一组服务，这些服务构成一个RocketMQ集群
services:
  # NameServer服务，负责管理和发现RocketMQ集群中的其他组件
  namesrv:
    # 指定NameServer服务所使用的镜像
    image: apache/rocketmq:5.3.0
    # 定义容器的名称
    container_name: rmqnamesrv
    # 将容器的9876端口映射到主机的9876端口
    ports:
      - 9876:9876
    # 将NameServer服务加入到名为rocketmq的网络中
    networks:
      - rocketmq
    # 启动时执行的命令
    command: sh mqnamesrv

  # Broker服务，负责存储消息和提供消息的收发功能
  broker:
    # 指定Broker服务所使用的镜像
    image: apache/rocketmq:5.3.0
    # 定义容器的名称
    container_name: rmqbroker
    # 将容器的多个端口映射到主机的对应端口
    ports:
      - 10909:10909
      - 10911:10911
      - 10912:10912
    # 设置环境变量，指定NameServer的地址
    environment:
      - NAMESRV_ADDR=rmqnamesrv:9876
    # 确保在启动Broker之前，NameServer已经启动
    depends_on:
      - namesrv
    # 将Broker服务加入到名为rocketmq的网络中
    networks:
      - rocketmq
    # 启动时执行的命令
    command: sh mqbroker

  # Proxy服务，提供HTTP协议的访问接口
  proxy:
    # 指定Proxy服务所使用的镜像
    image: apache/rocketmq:5.3.0
    # 定义容器的名称
    container_name: rmqproxy
    # 将Proxy服务加入到名为rocketmq的网络中
    networks:
      - rocketmq
    # 确保在启动Proxy之前，Broker和NameServer已经启动
    depends_on:
      - broker
      - namesrv
    # 将容器的两个端口映射到主机的对应端口
    ports:
      - 8080:8080
      - 8081:8081
    # 设置在服务失败时自动重启
    restart: on-failure
    # 设置环境变量，指定NameServer的地址
    environment:
      - NAMESRV_ADDR=rmqnamesrv:9876
    # 启动时执行的命令
    command: sh mqproxy

# 定义网络，用于这些服务之间的通信
networks:
  rocketmq:
    # 指定网络的驱动类型为bridge
    driver: bridge

```

注意：以上服务都来自于镜像 `apache/rocketmq:5.3.0`， 根据启动时执行的命令来执行不同的服务。





### 2. 启动 RocketMQ集群

根据 `docker-compose.yml` 文件启动所有定义的服务。在cmd 中运行如下命令

```shell
docker-compose -p rockermq_project up -d
```

其中，不同子命令的含义如下：

- `-p rockermq_project`： 指定项目名称为 `rockermq_project`
- `up`： 启动所有在 `docker-compose.yml` 文件中定义的服务。它会根据该文件拉取镜像、创建和启动容器，并处理所有依赖关系。
- `-d`：指定容器在后台运行。



运行结果为：

```shell
[+] Running 3/3
 ✔ Container rmqnamesrv  Started                                                                           0.2s
 ✔ Container rmqbroker   Started                                                                           0.1s
 ✔ Container rmqproxy    Started                                                                           0.1s
```



### 3. 启动可视化工具 RocketMQ Dashboard（可选）

`RocketMQ Dashboard` 是 RocketMQ 的管控利器，为用户提供客户端和应用程序的各种事件、性能的统计信息，支持以可视化工具代替 Topic 配置、Broker 管理等命令行操作。`RocketMQ Dashboard`具体细节查看 [RocketMQ Dashboard | RocketMQ (apache.org)](https://rocketmq.apache.org/zh/docs/deploymentOperations/04Dashboard#功能概览)

```shell
# 拉取 rocketmq-dashboard 镜像
$ docker pull apacherocketmq/rocketmq-dashboard:latest

# 运行 Dashboard ，连接到运行在 127.0.0.1:9876 的 NameServer
$ docker run -d --name rocketmq-dashboard -e "JAVA_OPTS=-Drocketmq.namesrv.addr=127.0.0.1:9876" -p 8082:8080 -t apacherocketmq/rocketmq-dashboard:latest
```

在运行 Dashboard 容器后，输入网址 `localhost:8082` 打开 `RocketMQ Dashboard`





### 4. 测试消息发送

#### 4.1 导入依赖

创建 Java 项目，导入依赖 `rocketmq-client-java` ，具体版本信息查看[Maven Central: org.apache.rocketmq:rocketmq-client-java (sonatype.com)](https://central.sonatype.com/artifact/org.apache.rocketmq/rocketmq-client-java/versions)

```
<dependency>
	<groupId>org.apache.rocketmq</groupId>
	<artifactId>rocketmq-client-java</artifactId>
	<version>${rocketmq-client-java-version}</version>
</dependency>
```



#### 4.2 创建 Topic

进入 broker 容器，通过 mqadmin 创建 Topic。

```sh
# 进入名为rmqbroker的容器，并启动一个交互式的Bash shell
$ docker exec -it rmqbroker bash

# 在容器内部使用mqadmin工具更新名为TestTopic的主题配置，使其属于DefaultCluster集群
$ sh mqadmin updatetopic -t TestTopic -c DefaultCluster

# 向指定的 NameServer 请求所有注册的 Topic 列表（无需执行）
$ sh mqadmin topicList -n rmqnamesrv:9876
```

具体运行过程为：

```shell
PS C:\Users\zouhu\Desktop> docker exec -it rmqbroker bash
rocketmq@15cc0e42b818:~/rocketmq-5.3.0/bin$ sh mqadmin updatetopic -t TestTopic -c DefaultCluster
create topic to 172.18.0.3:10911 success.
TopicConfig [topicName=TestTopic, readQueueNums=8, writeQueueNums=8, perm=RW-, topicFilterType=SINGLE_TAG, topicSysFlag=0, order=false, attributes={}]
rocketmq@15cc0e42b818:~/rocketmq-5.3.0/bin$
```



#### 4.3 生产者发送普通消息

```java
@Slf4j
public class ProducerExample {
    public static void main(String[] args) throws ClientException, IOException {
        // 接入点地址，需要设置成Proxy的地址和端口列表，一般是xxx:8080;xxx:8081
        String endpoint = "localhost:8080";
        // 消息发送的目标Topic名称，需要提前创建。
        String topic = "TestTopic";
        ClientServiceProvider provider = ClientServiceProvider.loadService();
        ClientConfigurationBuilder builder = ClientConfiguration.newBuilder().setEndpoints(endpoint);
        ClientConfiguration configuration = builder.build();
        // 初始化Producer时需要设置通信配置以及预绑定的Topic。
        Producer producer = provider.newProducerBuilder()
                .setTopics(topic)
                .setClientConfiguration(configuration)
                .build();
        // 普通消息发送。
        Message message = provider.newMessageBuilder()
                .setTopic(topic)
                // 设置消息索引键，可根据关键字精确查找某条消息。
                .setKeys("messageKey")
                // 设置消息Tag，用于消费端根据指定Tag过滤消息。
                .setTag("messageTag")
                // 消息体。
                .setBody("messageBody5".getBytes())
                .build();
        try {
            // 发送消息，需要关注发送结果，并捕获失败等异常。
            SendReceipt sendReceipt = producer.send(message);
            log.info("Send message successfully, messageId={}", sendReceipt.getMessageId());
        } catch (ClientException e) {
            log.error("Failed to send message", e);
        }
        producer.close();
    }
}
```





#### 4.3 消费者订阅普通消息

```java
@Slf4j
public class ConsumerExample {
    public static void main(String[] args) throws ClientException, IOException, InterruptedException {
        final ClientServiceProvider provider = ClientServiceProvider.loadService();
        // 接入点地址，需要设置成Proxy的地址和端口列表，一般是xxx:8080;xxx:8081
        // 此处为示例，实际使用时请替换为真实的 Proxy 地址和端口
        String endpoints = "localhost:8080";
        ClientConfiguration clientConfiguration = ClientConfiguration.newBuilder()
                .setEndpoints(endpoints)
                .build();
        // 订阅消息的过滤规则，表示订阅所有Tag的消息
        String tag = "*";
        FilterExpression filterExpression = new FilterExpression(tag, FilterExpressionType.TAG);
        // 为消费者指定所属的消费者分组，Group需要提前创建
        String consumerGroup = "YourConsumerGroup";
        // 指定需要订阅哪个目标Topic，Topic需要提前创建。
        String topic = "TestTopic";
        // 初始化PushConsumer，需要绑定消费者分组ConsumerGroup、通信参数以及订阅关系
        PushConsumer pushConsumer = provider.newPushConsumerBuilder()
                .setClientConfiguration(clientConfiguration)
                // 设置消费者分组
                .setConsumerGroup(consumerGroup)
                // 设置预绑定的订阅关系
                .setSubscriptionExpressions(Collections.singletonMap(topic, filterExpression))
                // 设置消费监听器
                .setMessageListener(messageView -> {
                    // 获取消息内容
                    ByteBuffer bodyBuffer = messageView.getBody();
                    byte[] bodyBytes = new byte[bodyBuffer.remaining()];
                    bodyBuffer.get(bodyBytes); // 读取缓冲区中的字节
                    String messageBody = new String(bodyBytes);

                    // 处理消息并返回消费结果
                    log.info("Consume message successfully, messageId={}, messageBody={},", messageView.getMessageId(), messageBody);
                    return ConsumeResult.SUCCESS;
                })
                .build();
        Thread.sleep(Long.MAX_VALUE);
        // 如果不需要再使用 PushConsumer，可关闭该实例
        pushConsumer.close();
    }
}

```



### 5. 停止所有服务

```
docker-compose down
```



### 参考资料

[Docker Compose 部署 RocketMQ | RocketMQ (apache.org)](https://rocketmq.apache.org/zh/docs/quickStart/03quickstartWithDockercompose#1编写docker-compose)

[Admin Tool 使用方法 | RocketMQ (apache.org)](https://rocketmq.apache.org/zh/docs/deploymentOperations/02admintool)