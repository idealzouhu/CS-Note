### 什么是 OpenFeign

**OpenFeign** 是一个声明式的 HTTP 客户端，它用于在微服务架构中简化服务之间的通信。在 Spring Cloud 生态系统中，OpenFeign 通过简单的注解就可以让你调用远程的 HTTP 服务，而不需要编写大量的模板代码。

其**核心概念和功能**主要有：

- **声明式 HTTP 客户端**：OpenFeign 允许你通过定义接口和使用注解来描述 HTTP 请求，并将这些请求映射到远程服务上。与传统的 REST 客户端不同，你不需要手动编写 HTTP 请求的代码，只需声明要调用的接口和方法即可。
- **负载均衡**：在 Spring Cloud 中，OpenFeign 可以与 Ribbon（或 Spring Cloud LoadBalancer）结合使用，实现客户端负载均衡。这样，当你调用一个远程服务时，OpenFeign 可以自动选择一个合适的实例来处理请求。
- **集成 Hystrix**：OpenFeign 也可以与 Hystrix 集成，以实现服务调用的熔断、降级等容错功能。在微服务架构中，服务之间的调用链可能非常复杂，通过熔断机制，可以避免级联故障，增强系统的健壮性。





### 声明式服务调用

OpenFeign 是SpringCloud 在 Feign的基础上支持了 SpringMVC的注解，如@RequestMapping等。OpenFeign 的@FeignClient可以解析SpringMVC的@RequestMapping注解下的接口，并通过动态代理的方式产生实现类，实现类中做负载均衡并调用其他服务。

```java
@FeignClient(name = "notify-service")
public interface RemoteNotifyService {

    @GetMapping("/notify/send")
    void sendNotification(@RequestParam("message") String message);
}

```

**@OpenFeign 的本质就是创建一个动态代理对象。**







### 替换默认的 httpclient

Feign在默认情况下使用的是JDK原生的**URLConnection**发送HTTP请求，没有连接池，但是对每个地址会保持一个长连接，即利用HTTP的persistence connection。

在生产环境中，通常不使用默认的http client，通常有如下两种选择：

- 使用**ApacheHttpClient**
- 使用**OkHttp**

替换方式为：

导入依赖

```
    <dependency>
      <groupId>org.apache.httpcomponents</groupId>
      <artifactId>httpclient</artifactId>
    </dependency>
    
<!--     使用 feign-httpclient 替换Feign原生httpclient-->
    
    <dependency>
      <groupId>io.github.openfeign</groupId>
      <artifactId>feign-httpclient</artifactId>
    </dependency>
```

开启配置

```
feign:
  client:
    config:
      default:
        connectTimeout: 5000
        loggerLevel: HEADERS
        readTimeout: 5000
  httpclient:
    enabled: false
  okhttp:
    enabled: true
```

[Spring Cloud OpenFeign夺命连环9问，这谁受得了？-CSDN博客](https://blog.csdn.net/sufu1065/article/details/123013496)





### 通讯优化

openFeign支持**请求/响应**开启 GZIP 压缩。