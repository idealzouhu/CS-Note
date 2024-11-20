### 什么是 Spring Boot 自动装配（Auto-Configuration）

**Spring Boot 自动装配**是 Spring Boot 的核心功能之一，旨在简化应用程序的配置和开发流程。通常，在传统的 Spring 应用中，开发者需要手动编写大量的 XML 或 Java 配置来定义 Bean、组件扫描、数据源、事务管理等各种组件。**Spring Boot 的自动装配机制可以根据项目的依赖（starter）和环境（application.yaml）自动配置这些组件，极大地减少了开发者手动配置的工作**。



例如，如果你引入了数据库依赖，比如 `spring-boot-starter-data-jpa`，Spring Boot 会自动配置 `DataSource`、`EntityManagerFactory` 等 Bean。



### 自动装配工作原理

**扫描类路径**： 读取 META-INF\spring.factories 文件，加载这其中的所有组件，

**条件判断**： 根据 XXXAutoConfiguration 自动配置类中的注解属性来决定哪些自动配置类生效、哪些不生效，生效之后再根据某些注解属性条件来决定向容器中添加哪些组件（@Bean），同时将 自动配置类与属性类 进行绑定，这样我们就可以通过service完成对XxxProeprties属性类的注入和使用。



### 自动装配底层原理

```
@SpringBootApplication
@MapperScan("org.opengoofy.index12306.biz.orderservice.dao.mapper")
@EnableFeignClients("org.opengoofy.index12306.biz.orderservice.remote")
@EnableCrane4j(enumPackages = "org.opengoofy.index12306.biz.orderservice.common.enums")
public class OrderServiceApplication {

    public static void main(String[] args) {
        SpringApplication.run(OrderServiceApplication.class, args);
    }
}
```



## @EnableAutoConfiguration

```
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage //作用：将main包下的所有组件注册到容器中
@Import({AutoConfigurationImportSelector.class}) //加载自动装配类 xxxAutoconfiguration
public @interface EnableAutoConfiguration {
    String ENABLED_OVERRIDE_PROPERTY = "spring.boot.enableautoconfiguration";

    Class<?>[] exclude() default {};

    String[] excludeName() default {};
}
```



去加载META-INF下的 spring-factories 文件。在spring-factories中配置了自动装配类





# 细节补充

因为RocketMQ 最新版本 2.2.3 没有适配 SpringBoot3，所以需要手动搞定自动装配。





### 参考资料

[Creating Your Own Auto-configuration :: Spring Boot](https://docs.spring.io/spring-boot/reference/features/developing-auto-configuration.html#features.developing-auto-configuration.understanding-auto-configured-beans)

[SpringBoot 自动装配原理详解 | JavaGuide](https://javaguide.cn/system-design/framework/spring/spring-boot-auto-assembly-principles.html#如何实现一个-starter)

[最近一场面试（Spring Boot的自动装配原理及流程）_spring自动装配的原理-CSDN博客](https://blog.csdn.net/weixin_45764765/article/details/110250531)