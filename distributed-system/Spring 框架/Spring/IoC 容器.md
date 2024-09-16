### IoC 容器的组件

`org.springframework.beans` 和 `org.springframework.beans` 是 Spring Framework 的 IoC 容器的基础。核心 IoC 容器的主要组件包括：

- [**BeanFactory**](https://docs.spring.io/spring-framework/docs/6.1.11/javadoc-api/org/springframework/beans/factory/BeanFactory.html): 容器的核心，负责配置、创建和管理应用对象（称为Bean）。
- [**ApplicationContext**](https://docs.spring.io/spring-framework/docs/6.1.11/javadoc-api/org/springframework/context/ApplicationContext.html):  BeanFactory的子接口，增加了对国际化、事件传播、资源加载等企业特定功能的支持。
- **WebApplicationContext**：专门为Web应用设计的IOC容器
- **Spring Expression Language** (SpEL): 一种功能强大的表达式语言，用于在运行时查询和操作对象图。

`BeanFactory` 和 `ApplicationContext` 的具体区别查看 [The BeanFactory API :: Spring Framework](https://docs.spring.io/spring-framework/reference/core/beans/beanfactory.html#context-introduction-ctx-vs-beanfactory)



### IoC 的本质

IoC 容器实际上就是一个 Map 



### ApplicationContext 接口

`ApplicationContext` 是 Spring 框架中的一个关键接口，它是 Spring 应用上下文的核心接口之一，用于管理 Spring 容器中的所有 bean 对象以及它们之间的依赖关系。

`ApplicationContext` 类的主要作用包括：

1. 管理 bean 对象：`ApplicationContext` 负责加载和管理 Spring 容器中的所有 bean 对象，包括创建、初始化和销毁 bean。
2. 实现依赖注入：通过 `ApplicationContext` 可以实现依赖注入，即将 bean 之间的依赖关系自动注入到相应的 bean 中，从而实现解耦和组件重用。
3. 提供配置和资源管理：`ApplicationContext` 提供了一种方便的方式来加载和管理配置文件、属性文件、国际化资源等外部资源。
4. 实现事件发布与监听：Spring 应用上下文支持事件驱动编程模型，`ApplicationContext` 可以发布事件并通知相关监听器。
5. 提供 AOP 支持：`ApplicationContext` 支持面向切面编程（AOP），可以通过配置实现横切关注点的功能，如日志记录、事务管理等。

在策略者模式中，

```java
// 从应用上下文中获取所有AbstractChainHandler类型的bean
        Map<String, AbstractChainHandler> chainFilterMap = ApplicationContextHolder
                .getBeansOfType(AbstractChainHandler.class);
```







### 参考资料

[Spring IOC深度解析：IOC容器的原理与高级特性详解_ioc容器的原理,深入理解如何通过反射和继承机制实例化类-CSDN博客](https://blog.csdn.net/feiying101/article/details/139043192)