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



去加载META-INF下的spring-factories文件。在spring-factories中配置了自动装配类











[最近一场面试（Spring Boot的自动装配原理及流程）_spring自动装配的原理-CSDN博客](https://blog.csdn.net/weixin_45764765/article/details/110250531)