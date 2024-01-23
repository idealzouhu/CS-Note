



# 一、Servlet概述

## 1.1 与Web服务器及程序协助的CGI

CGI（Common Gateway Interface，通用网关接口）是指 **Web 服务器在接收到客户端发送过来的请求后转发给程序的一组机制**。在 CGI 的 作用下，程序会对请求内容做出相应的动作，比如创建 HTML等动态内容。

使用 CGI 的程序叫做 CGI 程序，通常是用 Perl、PHP、Ruby 和 C 等 编程语言编写而成。

![image-20231202225406802](images/image-20231202225406802.png)



## 1.2 为什么引入servlet

之前提及的 CGI，由于每次接到请求，程序都要跟着启动一次。因此一旦访问量过大，Web 服务器要承担相当大的负载。而 Servlet 常驻内存，运行在与 Web 服务器相同的进程中。在每次请求时，可启动相对进程级别更为轻量的Servlet，程序的执行效率从而变得更高。因此，采用Servlet的Web服务器 受到的负载较小。**Servlet 作为解决 CGI 问题的对抗技术 ，随 Java 一起得到了普及**。

![image-20231202230251186](images/image-20231202230251186.png)

 **Servlet是一种能在服务器上生成动态Web内容的程序。**Servlet 是用 Java 语言实现的一个接口，属于面向企业级 Java（JavaEE，Java Enterprise Edition）的一部分。

>  Servlet全称Java Servlet，未有中文译文。其中，Servlet=Server +  Applet，表示 轻量服务程序

狭义的Servlet是指Java语言实现的一个接口(可以理解为规范或者协议)， 广义的Servlet是指任何实现了这个Servlet接口的类。一般情况下，人们将Servlet理解为后者。



## 1.3 Servlet 容器和 Servlet 规范

Servlet 的运行环境叫做  **Servlet 容器**（也称为Web 容器或者Servlet引擎）。由于Servlet没有main方法，不能独立运行，它必须被部署到Servlet容器中，由容器来实例化和调用 Servlet的方法，Servlet容器在Servlet的生命周期内包容和管理Servlet。

在java web应用中，常见的Servlet 容器主要有Tomcat、[Jetty](https://eclipse.dev/jetty/)、[GlassFish](https://javaee.github.io/glassfish/)、[Undertow](https://undertow.io/)。同时，Tomcat也是web服务器。Tomcat 提供了一个运行环境，用于加载、初始化和管理 Servlet，同时处理客户端的请求并将结果返回给客户端。它是 Java Web 应用程序的部署平台，负责管理 Servlet 的生命周期、处理并发请求、维护会话状态等。



一个Java Web App通常打包为`.war`文件，并且可以部署到Tomcat、Jetty等多种Web服务器上。**为什么一个Java Web App基本上可以无修改地部署到多种Web服务器上呢？原因就在于Servlet规范。**

Servlet规范是Java Servlet API的规范，用于定义Web服务器如何处理HTTP请求和响应。Servlet规范有一组接口，对于Web App来说，操作的是接口，而真正对应的实现类，则由各个Web Server实现，这样一来，Java Web App实际上编译的时候仅用到了Servlet规范定义的接口，只要每个Web服务器在实现Servlet接口时严格按照规范实现，就可以保证一个Web App可以正常运行在多种Web服务器上：

> 换个角度理解，servlet是jvm上j2ee定义的web容器协议，tomcat是jvm上servlet 协议的实现，

![image-20231202231822685](images/image-20231202231822685.png)







# 二、Java Servlet代码解析

Java Servlet是运行在带有支持 Java Servlet 规范的解释器的 web 服务器上的 Java 类， 其实现代码包含在java企业版里面。

> 我们平常使用的java SE并不包含Java Servlet实现代码

本文将以Java(TM) EE 7 Specification APIs为例进行阐述，其官方API文档路径为[Overview (Java(TM) EE 7 Specification APIs) (oracle.com)](https://docs.oracle.com/javaee/7/api/toc.htm)



## 1.2 Servlet API概述

Servlet API 包含以下4个Java包：

1.javax.servlet   其中包含定义servlet和servlet容器之间契约的类和接口。

2.javax.servlet.http   其中包含定义HTTP Servlet 和Servlet容器之间的关系。

3.javax.servlet.annotation   其中包含标注servlet，Filter,Listener的标注。它还为被标注元件定义元数据。

4.javax.servlet.descriptor，其中包含提供程序化登录Web应用程序的配置信息的类型。







# 参考资料

[Servlet 简介 | 菜鸟教程 (runoob.com)](https://www.runoob.com/servlet/servlet-intro.html)

[什么是Servlet？有什么作用？ - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/625617649)

[tomcat与servlet - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/465936851)

[Servlet规范 - 廖雪峰的官方网站 (liaoxuefeng.com)](https://www.liaoxuefeng.com/wiki/1545956031987744/1545956220731425)