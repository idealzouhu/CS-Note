## Spring MVC 的核心组件

DispatcherServlet是前端控制器设计模式的实现，提供Spring Web MVC的集中访问点，而且负责职责的分派，而且与Spring IoC容器无缝集成，从而可以获得Spring的所有好处



### Spring MVC的工作原理

Spring MVC的工作原理如下：

1. 客户端所有的请求都提交给 DispatcherServlet,它会委托应用系统的其他模块负责负责对请求进行真正的处理工作。 
2. DispatcherServlet 查询一个或多个 HandlerMapping , 找到处理请求的 Controller 。
3. DispatcherServlet 调用 HandlerAdapter 执行 Handler ， 实际上就是将请求提交到目标 Controller 。
4. Controller 进行业务逻辑处理后，会返回一个 ModelAndView 。
5. DispatcherServlet 查询一个或多个 ViewResolver 视图解析器, 找到ModelAndView对象指定的视图对象 。
6. 视图对象负责渲染返回给客户端。

![img](images/de6d2b213f112297298f3e223bf08f28.png)





### 参考资料

[【SpringMVC】一文带你彻底搞懂SpringMVC的工作流程（最强详解！！）_简单的谈一下springmvc的工作流程-CSDN博客](https://blog.csdn.net/miles067/article/details/132571198)

[Spring常见面试题总结 | JavaGuide](https://javaguide.cn/system-design/framework/spring/spring-knowledge-and-questions-summary.html#springmvc-工作原理了解吗)

[Spring Web MVC :: Spring Framework](https://docs.spring.io/spring-framework/reference/web/webmvc.html)

