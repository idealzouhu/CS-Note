### Spring MVC 的核心组件

- **`DispatcherServlet`**  是前端控制器设计模式的实现，提供 Spring Web MVC 的集中访问点，而且负责职责的分派，而且与Spring IoC 容器无缝集成，从而可以获得 Spring 的所有好处
- **`HandlerMapping`**：用于将 HTTP 请求映射到具体的 `Handler`
- **`Handler`** 是处理 HTTP 请求的具体对象或方法， 即所熟悉的 Controller。
- **`HandlerAdapter`** 是调用 `Handler` 的适配器，它根据 `Handler` 的类型来选择合适的调用方式。





### Spring MVC的工作原理

Spring MVC的工作原理如下：

1. **请求进入**： 客户端所有的请求都提交给 `DispatcherServlet`,它会委托应用系统的其他模块负责负责对请求进行真正的处理工作。 
2. **查询 `Handler`**： `DispatcherServlet` 查询一个或多个 `HandlerMapping` , 找到处理请求的 `Handler`。
3. **匹配 `HandlerAdapter`** ：找到 `Handler` 后，`DispatcherServlet` 会根据 `Handler` 的类型，选择一个合适的 `HandlerAdapter`。不同的 `Handler` 可能需要不同的 `HandlerAdapter` 来适配，比如 `RequestMappingHandlerAdapter` 适用于基于注解的控制器方法。
4. **调用 `HandlerAdapter`**： `HandlerAdapter` 调用 `Handler` 来处理请求，并执行与之相关的业务逻辑。最后，`HandlerAdapter`  将 `Handler`  的执行结果包装成`ModelAndView` ， 并返回给 `DispatcherServlet`   。
5. **视图解析**：`DispatcherServlet` 查询一个或多个 `ViewResolver` 视图解析器, 找到 `ModelAndView` 对象指定的视图对象 。
6. **返回响应**：视图对象负责渲染返回给客户端。

![img](images/de6d2b213f112297298f3e223bf08f28.png)





### 适配器模式

`HandlerAdapter` 和 `Handler` 是解耦的，通过 `HandlerAdapter`，Spring MVC 能够灵活支持不同类型的处理器，而不需要 `DispatcherServlet` 直接依赖于特定的 `Handler` 类型。

适配器的职责是将请求委托给具体的 `Controller`（源接口），并将结果返回给客户端（目标接口）。其中，

- 目标接口：客户端的 HTTP 请求，例如对 `/example` 的 GET 请求。
- 源接口：`HandlerAdapter`，它是处理请求的接口，用于适配不同类型的 `Controller`。
- 适配器：`HandlerAdapter` 的具体实现类，它将请求适配到具体的 `Controller` 实现。



举个例子，

```java
@Controller
public class MyController {
    @RequestMapping("/hello")
    public String sayHello() {
        return "hello";
    }
}
```

在这个例子中：

- `MyController` 的 `sayHello` 方法就是一个 `Handler`。
- `RequestMappingHandlerAdapter` 就是相应的 `HandlerAdapter`，它负责调用带有 `@RequestMapping` 注解的处理方法。

当请求 `/hello` 时，`DispatcherServlet` 通过 `HandlerMapping` 找到 `MyController` 的 `sayHello` 方法（即 `Handler`），然后使用 `RequestMappingHandlerAdapter` 适配并调用该方法。









### 参考资料

[【SpringMVC】一文带你彻底搞懂SpringMVC的工作流程（最强详解！！）_简单的谈一下springmvc的工作流程-CSDN博客](https://blog.csdn.net/miles067/article/details/132571198)

[Spring常见面试题总结 | JavaGuide](https://javaguide.cn/system-design/framework/spring/spring-knowledge-and-questions-summary.html#springmvc-工作原理了解吗)

[Spring Web MVC :: Spring Framework](https://docs.spring.io/spring-framework/reference/web/webmvc.html)

