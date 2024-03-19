

`ApplicationContext` 类是 Spring 框架中的一个关键接口，它是 Spring 应用上下文的核心接口之一，用于管理 Spring 容器中的所有 bean 对象以及它们之间的依赖关系。

`ApplicationContext` 类的主要作用包括：

1. 管理 bean 对象：`ApplicationContext` 负责加载和管理 Spring 容器中的所有 bean 对象，包括创建、初始化和销毁 bean。
2. 实现依赖注入：通过 `ApplicationContext` 可以实现依赖注入，即将 bean 之间的依赖关系自动注入到相应的 bean 中，从而实现解耦和组件重用。
3. 提供配置和资源管理：`ApplicationContext` 提供了一种方便的方式来加载和管理配置文件、属性文件、国际化资源等外部资源。
4. 实现事件发布与监听：Spring 应用上下文支持事件驱动编程模型，`ApplicationContext` 可以发布事件并通知相关监听器。
5. 提供 AOP 支持：`ApplicationContext` 支持面向切面编程（AOP），可以通过配置实现横切关注点的功能，如日志记录、事务管理等。







# 应用案例



在策略者模式中，

```
// 从应用上下文中获取所有AbstractChainHandler类型的bean
        Map<String, AbstractChainHandler> chainFilterMap = ApplicationContextHolder
                .getBeansOfType(AbstractChainHandler.class);
```









