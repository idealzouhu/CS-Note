## 一、什么是 Spring Boot 自动配置

**Spring Boot 自动配置（Auto-Configuration）**是 Spring Boot 的核心功能之一，旨在简化应用程序的配置和开发流程。通常，在传统的 Spring 应用中，开发者需要手动编写大量的 XML 或 Java 配置来定义 Bean、组件扫描、数据源、事务管理等各种组件。<font color="blue">**Spring Boot 的自动配置机制可以根据项目的依赖（starter）和环境（application.yaml）自动配置这些组件，极大地减少了开发者手动配置的工作**</font>。

例如，如果你引入了数据库依赖，比如 `spring-boot-starter-data-jpa`，Spring Boot 会自动配置 `DataSource`、`EntityManagerFactory` 等 Bean。



## 二、自动配置工作原理

自动配置的工作步骤为：

1. **启动自动配置**：Spring Boot通过`@EnableAutoConfiguration` 注解开启自动装配。
2. **扫描类路径**： 扫描所有依赖的 jar 包中的 `META-INF\spring.factories` 文件，加载文件中的所有配置类  `XXXAutoConfiguration`。
3. **条件判断**：每每个自动配置类通过 `@Conditional` 系列注解检查是否符合加载条件。



### 2.1 启动自动配置

在 Spring Boot 应用程序中，`@SpringBootApplication` 是表示标识一个类为 Spring Boot 的启动类的注解，整合了多个功能性注解：

-  `@EnableAutoConfiguration`：启用 Spring Boot 的自动配置机制，根据类路径中的依赖和配置文件自动加载适合的 Spring Bean。
- `@SpringBootConfiguration`：是一个特殊的 `@Configuration`，用于标记当前类为 Spring 的配置类。
- `@ComponentScan`：启用组件扫描，自动发现和注册被注解（如 `@Component`、`@Service`、`@Repository` 等）标注的 Bean，同时支持排除特定类型的组件。

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
}
```

<font color="red">**`@EnableAutoConfiguration` 注解是启动自动配置的核心**</font>，它会引导 Spring 根据应用的依赖和配置自动配置合适的 Bean。其中，

- `@AutoConfigurationPackage`：将main包下的所有组件注册到容器中

- `@Import(AutoConfigurationImportSelector.class)` ： 加载所有符合条件的自动配置类。

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import(AutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {
}
```





### 2.2 扫描类路径

`AutoConfigurationImportSelector` 会扫描依赖的 jar 包中的 `META-INF/spring.factories` 文件，并加载其中定义的自动配置类：

> 从 Spring Boot 3.x 开始，自动配置包的路径从`META-INF/spring.factories` 修改为 `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`

```java
# 示例：某个 spring.factories 文件的部分内容
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.DispatcherServletAutoConfiguration,\
org.springframework.boot.autoconfigure.orm.jpa.HibernateJpaAutoConfiguration
```

`AutoConfigurationImportSelector` 类的核心代码如下所示。

```java
public class AutoConfigurationImportSelector implements DeferredImportSelector, BeanClassLoaderAware, ResourceLoaderAware, BeanFactoryAware, EnvironmentAware, Ordered {
    
     /**
     * 根据注解元数据选择需要导入的自动配置类
     * 
     * @param annotationMetadata 注解元数据，用于检查是否启用了自动配置
     * @return 返回需要导入的自动配置类的全限定名数组如果未启用自动配置，则返回空数组
     */
    public String[] selectImports(AnnotationMetadata annotationMetadata) {
        // 检查自动配置是否已启用
        if (!this.isEnabled(annotationMetadata)) {
            // 如果未启用，返回空数组，表示不需要导入任何自动配置类
            return NO_IMPORTS;
        } else {
            // 获取自动配置类
            AutoConfigurationEntry autoConfigurationEntry = this.getAutoConfigurationEntry(annotationMetadata);
            // 将自动配置类的全限定名转换为字符串数组并返回
            return StringUtils.toStringArray(autoConfigurationEntry.getConfigurations());
        }
    }
    
     /**
     * 获取自动配置类
     * 根据给定的注解元数据，决定哪些配置类应该被自动配置
     * 
     * @param annotationMetadata 注解元数据，用于检查是否启用自动配置以及获取配置属性
     * @return 返回一个自动配置项对象，包含要应用的配置类列表和排除的配置类列表
     */
    protected AutoConfigurationEntry getAutoConfigurationEntry(AnnotationMetadata annotationMetadata) {
        // 检查自动配置是否已启用
        if (!this.isEnabled(annotationMetadata)) {
            // 如果未启用，返回空的自动配置项
            return EMPTY_ENTRY;
        } else {
            // 获取自动配置的属性
            AnnotationAttributes attributes = this.getAttributes(annotationMetadata);
            // 获取候选的自动配置类列表
            List<String> configurations = this.getCandidateConfigurations(annotationMetadata, attributes);
            // 去除重复的配置类
            configurations = this.removeDuplicates(configurations);
            // 获取需要排除的配置类集合
            Set<String> exclusions = this.getExclusions(annotationMetadata, attributes);
            // 检查并记录被排除的配置类
            this.checkExcludedClasses(configurations, exclusions);
            // 从配置列表中移除被排除的配置类
            configurations.removeAll(exclusions);
            // 过滤配置类，确保只包含符合条件的配置
            configurations = this.getConfigurationClassFilter().filter(configurations);
            // 触发自动配置导入事件，通知其他组件
            this.fireAutoConfigurationImportEvents(configurations, exclusions);
            // 返回最终的自动配置项，包含要应用的配置类列表和排除的配置类列表
            return new AutoConfigurationEntry(configurations, exclusions);
        }
    }

}
```

在 `getAutoConfigurationEntry()` 函数中，`configurations = this.getConfigurationClassFilter().filter(configurations);` 会**筛选符合条件的配置类**，这也是自动配置中的关键步骤。这个筛选条件主要是通过自动配置类中的 **`@Conditional`** 系列注解来实现的。



### 2.3 条件配置

在自动配置类里面，Spring Boot 使用 **`@Conditional`** 系列注解来决定配置类是否生效。常用的注解为：

| 注解                              | 条件描述                                                     |
| --------------------------------- | ------------------------------------------------------------ |
| `@ConditionalOnBean`              | 当指定的 Bean 存在时，条件匹配。                             |
| `@ConditionalOnMissingBean`       | 当指定的 Bean 不存在时，条件匹配。                           |
| `@ConditionalOnClass`             | 当指定的类在类路径中存在时，条件匹配。                       |
| `@ConditionalOnMissingClass`      | 当指定的类在类路径中不存在时，条件匹配。                     |
| `@ConditionalOnProperty`          | 当指定的配置属性（`application.properties` 或 `application.yml`）满足条件时，条件匹配。 |
| `@ConditionalOnExpression`        | 当给定的 SpEL 表达式（Spring Expression Language）计算结果为 `true` 时，条件匹配。 |
| `@ConditionalOnJava`              | 当运行的 Java 版本满足条件时，条件匹配。                     |
| `@ConditionalOnResource`          | 当指定的资源文件存在时，条件匹配。                           |
| `@ConditionalOnWebApplication`    | 当当前应用是 Web 应用时，条件匹配。                          |
| `@ConditionalOnNotWebApplication` | 当当前应用不是 Web 应用时，条件匹配。                        |

根据 `XXXAutoConfiguration` 自动配置类中的注解属性来决定哪些自动配置类生效、哪些不生效，生效之后再根据某些注解属性条件来决定向容器中添加哪些组件（@Bean），同时	将自动配置类与属性类进行绑定，这样我们就可以通过 service 完成对`XxxProeprties` 属性类的注入和使用。举个例子，

```java
// 定义属性类
@Data
@ConfigurationProperties(prefix = "mail")  // 自动获取配置文件application.yaml中前缀为mail的配置
public class MailProperties {
    // 如果配置文件中配置了host属性，则该默认属性会被覆盖
    private String host = "xxx";
    private int port = 465;
    private String username = "xxx";
    private String password = "xxx";
    private String from = "xxx";

}

// 自动配置类
@AutoConfiguration
@EnableConfigurationProperties(MailProperties.class)  // 启用并注册配置绑定功能
public class MailAutoConfiguration {

    @Bean
    @ConditionalOnMissingBean   // 当容器中未存在指定类型的Bean时，才会创建当前Bean
    public MailService mailService(MailProperties properties) {
        return new MailService(properties);
    }
}
```







## 三、细节补充

因为RocketMQ 最新版本 2.2.3 没有适配 SpringBoot3，所以需要手动搞定自动装配。





## 参考资料

[Creating Your Own Auto-configuration :: Spring Boot](https://docs.spring.io/spring-boot/reference/features/developing-auto-configuration.html#features.developing-auto-configuration.understanding-auto-configured-beans)

[SpringBoot 自动装配原理详解 | JavaGuide](https://javaguide.cn/system-design/framework/spring/spring-boot-auto-assembly-principles.html#如何实现一个-starter)

[最近一场面试（Spring Boot的自动装配原理及流程）_spring自动装配的原理-CSDN博客](https://blog.csdn.net/weixin_45764765/article/details/110250531)