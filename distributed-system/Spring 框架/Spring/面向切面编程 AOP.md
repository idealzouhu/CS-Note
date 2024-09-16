[TOC]

## 一、AOP 概述

### 1.1 什么是 AOP

AOP（Aspect-Oriented Programming，面向切面编程）是一种编程范式，旨在<font color="red">**将横切关注点（cross-cutting concerns）从业务逻辑代码中分离出来，以提高代码的模块化和可维护性**</font>。横切关注点是指那些在多个模块中都会出现的关注点，例如日志记录、安全检查、事务管理等。

<img src="images/crosscut-logic-and-businesslogic-separation      .png" alt="img" style="zoom:50%;" />



### 1.2 AOP 的领域模型

**切面（Aspect）**：切面是一个模块，包含横切关注点的定义和实现。切面可以通过注解或配置来声明。

**连接点（Join Point）**：程序执行的某个特定点，例如方法调用、异常抛出等。AOP 可以在这些点上插入横切关注点。

**切入点（Pointcut）**：切入点定义了切面要织入的连接点的集合。通过切入点表达式，可以精确地指定在哪些连接点上应用切面。

**通知（Advice）**：通知定义了切面在特定的连接点上执行的动作。通知类型包括前置通知、后置通知、返回通知、异常通知和环绕通知。

**织入（Weaving）**：织入是将切面应用到目标对象并创建代理对象的过程。织入可以在编译时、类加载时或运行时进行。

AOP 的术语具体解释查看 [AOP Concepts :: Spring Framework](https://docs.spring.io/spring-framework/reference/6.0/core/aop/introduction-defn.html)



### 1.3 AOP 的工作原理

AOP 的工作原理是通过**代理模式**，将切面逻辑动态地织入到目标对象的执行流程中。或者通过字节码操作来实现 AOP。

常见的 AOP 框架（如 Spring AOP）通常使用动态代理技术来实现这一点。



### 1.4 Spring AOP 和 AspectJ AOP 有什么区别

**Spring AOP** 和 **AspectJ AOP** 都是面向切面编程（AOP）的实现方案，但在使实现方式、使用场景以及功能特性上有显著区别。

- **实现方式**：
  - **Spring AOP**:  基于**代理**（Proxy）机制，默认使用 JDK 动态代理或 CGLIB 代理实现。
  - AspectJ AOP: 在编译时、类加载时甚至运行时进行**字节码操作**，使用独立的 AspectJ 编译器来处理切面逻辑。

- 切入点表达式：
  - **Spring AOP**: 仅支持**方法级别**的切面。它只支持切入方法调用的逻辑，无法切入构造函数、字段访问、静态方法等。
  - **AspectJ AOP**: 支持更丰富的切入点表达式，包括**方法、构造函数、字段、静态方法、异常处理块**等。它能够对代码的更多方面进行切入和增强。



## 二、Spring AOP

在 Spring 框架里面，AOP 解决方案主要有：

- 基于代理的框架（如 Spring AOP）
-  AspectJ 框架 （Spring 将 Spring AOP 和 IoC 与 AspectJ 无缝集成）



### 2.1 Spring AOP 的常用注解

| 注解                                                         | 用途                                 |
| ------------------------------------------------------------ | ------------------------------------ |
| [@Aspect](https://docs.spring.io/spring-framework/reference/6.0/core/aop/ataspectj/at-aspectj.html) | 将一个类声明为切面类                 |
| [@Pointcut](https://docs.spring.io/spring-framework/reference/6.0/core/aop/ataspectj/pointcuts.html) | 定义切入点，用于匹配连接点           |
| [@Before](https://docs.spring.io/spring-framework/reference/6.0/core/aop/ataspectj/advice.html#aop-advice-before) | 前置通知，在目标方法执行之前执行     |
| @After                                                       | 后置通知，在目标方法执行之后执行     |
| [@AfterReturning](https://docs.spring.io/spring-framework/reference/6.0/core/aop/ataspectj/advice.html#aop-advice-after-returning) | 返回通知，在目标方法成功执行之后执行 |
| [@AfterThrowing](https://docs.spring.io/spring-framework/reference/6.0/core/aop/ataspectj/advice.html#aop-advice-after-throwing) | 异常通知，在目标方法抛出异常后执行   |
| [@Around](https://docs.spring.io/spring-framework/reference/6.0/core/aop/ataspectj/advice.html#aop-ataspectj-around-advice) | 环绕通知，围绕目标方法执行           |

注意，**切面类需要被识别为bean，**以便Spring AOP框架能够管理和应用切面。





### 2.2 @Around 使用示例

[`ProceedingJoinPoint`](https://docs.spring.io/spring-framework/reference/6.0/core/aop/ataspectj/advice.html#aop-ataspectj-around-advice) 是 Spring AOP 中的一个接口，用于在环绕通知（`@Around`）中**访问和控制连接点的执行**。它提供了执行目标方法的能力，并允许获取目标方法的详细信息，例如方法参数、返回值、执行时间等。

```java
@Aspect
public class LoggingAspect {

    @Around("execution(* com.example.service.*.*(..))")
    public Object logAround(ProceedingJoinPoint joinPoint) throws Throwable {
        System.out.println("Method " + joinPoint.getSignature().getName() + " is called");
        
        // 获取方法参数
        Object[] args = joinPoint.getArgs();
        for (Object arg : args) {
            System.out.println("Arg: " + arg);
        }

        // 执行目标方法
        Object result = joinPoint.proceed();

        System.out.println("Method " + joinPoint.getSignature().getName() + " execution completed");
        return result;
    }
}
```



### 2.3 切入点表达式语法

切入点表达式用于定义在哪些连接点（ JoinPoint ）上应用通知（ Advice）。

常用的[表达式]([Declaring a Pointcut :: Spring Framework](https://docs.spring.io/spring-framework/reference/6.0/core/aop/ataspectj/pointcuts.html#aop-pointcuts-designators))有：

| 表达式        | 描述                                   |
| ------------- | -------------------------------------- |
| `execution`   | 匹配方法执行，常用的基本表达式。       |
| `within`      | 限定匹配指定类的方法执行。             |
| `this`        | 匹配当前 AOP 代理对象的类型。          |
| `target`      | 匹配当前目标对象实例的类。             |
| `args`        | 匹配当前方法参数的类。                 |
| `@within`     | 匹配特定注解所标注的类。               |
| `@target`     | 匹配当前目标对象类型具有指定注解的类。 |
| `@args`       | 匹配当前方法参数持有特定注解的执行。   |
| `@annotation` | 匹配当前执行方法持有特定注解的方法。   |
| `bean`        | 匹配特定名称的 Bean 对象。             |

常用的符号有：

| 符号 | 含义 | 用途                                                 | 示例                                                      |
| ---- | ---- | ---------------------------------------------------- | --------------------------------------------------------- |
| `.`  | 点   | 用于分隔包名、类名和方法名，表示明确的路径           | `execution(com.example.service.*)`                        |
| `..` | 双点 | 表示匹配任意数量的包或参数                           | `execution(com.example..*)`                               |
| `*`  |      | 在参数列表中，表示匹配任意数量的参数，包括零个或多个 | `execution(* com.example.service.MyService.myMethod(..))` |

另外，我们可以使用 `&&`, `||`, `!` 运算符来[组合多个切入点表达式](https://docs.spring.io/spring-framework/reference/6.0/core/aop/ataspectj/pointcuts.html#aop-pointcuts-combining)。

举个例子，

```java
@Around("execution(* com.example.service.*.*(..))")
```

这个切入点表达式分解如下：

- `execution`: 指定这是一个方法执行的切入点表达式。
- `*`: 返回类型，`*` 表示匹配任意返回类型。
- `com.example.service.*`: 类路径，`com.example.service.*` 表示 `com.example.service` 包下的任意类。
- `*`: 方法名，`*` 表示任意方法。
- `(..)`: 参数列表，`(..)` 表示匹配任意参数列表。

切入点表达式语法具体细节查看  [Declaring a Pointcut :: Spring Framework](https://docs.spring.io/spring-framework/reference/6.0/core/aop/ataspectj/pointcuts.html#aop-pointcuts-designators)



## 参考资料

[Aspect Oriented Programming with Spring :: Spring Framework](https://docs.spring.io/spring-framework/reference/6.0/core/aop.html)

[Spring AOP APIs :: Spring Framework](https://docs.spring.io/spring-framework/reference/6.0/core/aop-api.html)

[IoC & AOP详解（快速搞懂） | JavaGuide](https://javaguide.cn/system-design/framework/spring/ioc-and-aop.html#aop-解决了什么问题)

[手摸手实现日志组件库 (yuque.com)](https://www.yuque.com/magestack/12306/xbqtshu86z8hq4t2)

[第15章-Spring AOP切点表达式（Pointcut）详解-CSDN博客](https://blog.csdn.net/weixin_43793874/article/details/124753521)