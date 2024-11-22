> 完整示例代码参考：[java-demos/spring-cloud-nacos at main · idealzouhu/java-demos (github.com)](https://github.com/idealzouhu/java-demos/tree/main/spring-cloud-nacos)

## 一、Nacos 部署

### 1.1 启动 Nacos Server

```bash
docker run ^
-d -p 8848:8848 ^
-p 9848:9848 ^
--name nacos-test ^
-e MODE=standalone ^
-e TIME_ZONE='Asia/Shanghai' ^
nacos/nacos-server:v2.1.2
```

部分指令的含义如下：

- `-e MODE=standalone`: 使用环境变量 `MODE` 设置 Nacos 的启动模式。这里设置为 `standalone` 模式，意味着单机模式运行（适合开发环境使用）。在生产环境中，通常会配置为集群模式。
- `-e TIME_ZONE='Asia/Shanghai'`: 设置容器内的时区为 `Asia/Shanghai`，即中国标准时间（CST，东八区）



### 1.2 启用 Nacos 控制台

运行成功，稍等几秒启动时间，浏览器输入  http://localhost:8848/nacos/index.html  查看控制台。



## 二、微服务设置

### 2.1 导入依赖

```xml
<dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
        </dependency>

        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>

        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-loadbalancer</artifactId>
        </dependency>
```



### 2.2 创建微服务

创建多个微服务  **User Service**、**Order Service**、**Payment Service** 和 **Notification Service**。这些微服务将通过 Nacos 进行服务注册和发现，它们之间有特定的关系和交互方式。

不同微服务之间的关系为：

- **User Service** 提供用户信息，供其他微服务使用。
- **Order Service** 处理用户的订单，并在订单创建时调用 **Payment Service** 来处理支付。
- **Payment Service** 处理支付请求，并在支付成功时调用 **Notification Service** 发送通知。
- **Notification Service** 负责发送通知，例如订单状态更新通知





### 2.3 **`application.yml`** 配置

除了 `spring.application.name` 和 `server.port`, 不同微服务之间的配置基本上差不多。

```yaml
spring:
  application:
    name: order-service
  cloud:
    nacos:
      # nacos 注册中心
      discovery:
        server-addr: 127.0.0.1:8848   # nacos 注册中心地址
        namespace: public
server:
  port: 9000
```



### 三、创建微服务

由于这些微服务的代码基本类似，本文只展示如何创建 Order Service 。

### 3.1 启用客户端

使用 `@EnableDiscoveryClient`。

注意：如果不使用 `@EnableDiscoveryClient` 注解的话，`@FeignClient` 注解将无法使用。

```java
@SpringBootApplication
@EnableFeignClients(basePackages = "com.zouhu.orderservice.remote")
public class OrderServiceApplication {

	public static void main(String[] args) {
		SpringApplication.run(OrderServiceApplication.class, args);
	}

}

```



### 3.2  创建 Feign 代理对象

使用注解 `@FeignClient` 创建代理对象。

```
@FeignClient(name = "pay-service")      // 定义一个 Feign 客户端接口，用于远程调用其他服务
public interface PayRemoteService {

    // 定义远程调用方法
    @GetMapping("/pay/process")
    String processPayment();

}
```



### 3.3 测试远程调用服务

在运行所有微服务后，运行 http://localhost:9000/order/create

```java
@RestController
@RequestMapping("/order")
public class OrderController {
    @Autowired
    private PayRemoteService payRemoteService;

    /**
     * 创建订单并调用支付服务
     * <p>
     *     调用方法链接： http://localhost:9000/order/create
     *     Nacos控制台访问链接： http://localhost:8848/nacos/index.html
     * </p>
     *
     * @return
     */
    @GetMapping("/create")
    public String createOrder() {
        String paymentStatus = payRemoteService.processPayment();
        return "Order created! Payment status: " + paymentStatus;
    }

}
```





## 参考案例

[Spring Cloud OpenFeign 中文文档 (springdoc.cn)](https://springdoc.cn/spring-cloud-openfeign/#手动创建-feign-client)

[Nacos Docker 快速开始 | Nacos 官网](https://nacos.io/docs/latest/quickstart/quick-start-docker/#_top)

[Nacos快速入门教程：从零开始搭建配置中心与服务注册发现_慕课手记 (imooc.com)](https://www.imooc.com/article/352069#:~:text=Nacos适用场景 1 微服务架构中的服务发现与配置管理。,2 多团队或部门的资源隔离与权限控制。 3 实时更新配置以快速响应业务需求的变化。)