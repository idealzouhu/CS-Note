Spring的AOP模块提供了丰富的面向切面的编程实现，允许定义方法拦截和切点以实现诸如事务管理、安全性、日志记录等横切逻辑。AOP功能的关键组成包括：

Advice: 定义了在方法执行的某个阶段执行的动作。
Pointcut: 定义了在哪些方法上执行Advice的规则。
Advisor: 将Pointcut与Advice结合。
Aspect: 模块化的横切关注点的集合。



## AOP 术语

[IoC & AOP详解（快速搞懂） | JavaGuide](https://javaguide.cn/system-design/framework/spring/ioc-and-aop.html#aop-解决了什么问题)



## AOP 案例

```
package org.opengoofy.index12306.framework.starter.log.annotation;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

/**
 * Log 注解打印，可以标记在类或者方法上
 * 标记在类上，类下所有方法都会打印；标记在方法上，仅打印标记方法；如果类或者方法上都有标记，以方法上注解为准
 *
 * @公众号：马丁玩编程，回复：加群，添加马哥微信（备注：12306）获取项目资料
 */
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
public @interface ILog {

    /**
     * 入参打印
     *
     * @return 打印结果中是否包含入参，{@link Boolean#TRUE} 打印，{@link Boolean#FALSE} 不打印
     */
    boolean input() default true;

    /**
     * 出参打印
     *
     * @return 打印结果中是否包含出参，{@link Boolean#TRUE} 打印，{@link Boolean#FALSE} 不打印
     */
    boolean output() default true;
}
```



```
 @Aspect
public class ILogPrintAspect {
    @Around("@within(org.opengoofy.index12306.framework.starter.log.annotation.ILog) ||  @annotation(org.opengoofy.index12306.framework.starter.log.annotation.ILog)")
        public Object printMLog(ProceedingJoinPoint joinPoint) throws Throwable {
        ..........
    }

}
```

   这个Java函数使用了AOP（面向切面编程）的@Around注解，表示它是一个环绕通知，会在匹配到特定切面的情况下执行。 `"@within(org.opengoofy.index12306.framework.starter.log.annotation.ILog) || @annotation(org.opengoofy.index12306.framework.starter.log.annotation.ILog)"`: 这是一个切点表达式，用于指定哪些方法需要被拦截。其中 `@within` 表示匹配带有 `@ILog` 注解的类，`@annotation` 表示匹配带有 `@ILog` 注解的方法。这样，无论是带有 `@ILog` 注解的类还是方法，都会被拦截。