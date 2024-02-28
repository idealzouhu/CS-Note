## Spring MVC

Spring MVC即一种基于 Java 的实现 MVC 设计模型的请求驱动类型的轻量级 Web 框架，主要由 DispatcherServlet、处理器映射、控制器、视图解析器、视图组成。其工作流程主要为：客户端请求提交到 DispatcherServlet；由 DispatcherServlet 控制器寻找一个或多个 HandlerMapping，找到处理请求的 Controller；DispatcherServlet 将请求提交到 Controller；Controller 调用业务逻辑处理后返回 ModelAndView；DispatcherServlet 寻找一个或多个 ViewResolver 视图解析器，找到 ModelAndView 指定的视图；视图负责将结果显示到客户端。

在Spring MVC的接口中，我具体的了解了DispatcherServlet。DispatcherServlet是前端控制器设计模式的实现，提供Spring Web MVC的集中访问点，而且负责职责的分派，而且与Spring IoC容器无缝集成，从而可以获得Spring的所有好处。即Spring MVC 所有的请求都经过 DispatcherServlet 来统一分发，在 DispatcherServlet 将请求分发给 Controller 之前需要借助 Spring MVC 提供的 HandlerMapping 定位到具体的 Controller。



springMVC的工作原理如下：

> - springmvc请所有的请求都提交给DispatcherServlet,它会委托应用系统的其他模块负责负责对请求进行真正的处理工作。 
> - DispatcherServlet查询一个或多个HandlerMapping,找到处理请求的Controller。
> - DispatcherServlet请请求提交到目标Controller 。
> - Controller进行业务逻辑处理后，会返回一个ModelAndView 。
> - Dispathcher查询一个或多个ViewResolver视图解析器,找到ModelAndView对象指定的视图对象 。
> - 视图对象负责渲染返回给客户端。

[C语言中文网](http://c.biancheng.net/view/4392.html)