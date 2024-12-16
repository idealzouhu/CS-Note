## 一、循环依赖概述

### 什么是循环依赖

在依赖注入框架（如 Spring IoC）中，循环依赖通常是指两个或多个 Bean 相互依赖，导致无法顺利创建 Bean 实例。

例如，假设有两个类 `ClassA` 和 `ClassB`，并且它们相互依赖。具体代码如下：

```java
class ClassA {
    private ClassB classB;

    public ClassA(ClassB classB) {
        this.classB = classB;
    }
}

class ClassB {
    private ClassA classA;

    public ClassB(ClassA classA) {
        this.classA = classA;
    }
}
```

在这种情况下，`ClassA` 和 `ClassB` 无法同时正确实例化，因为：

- 当容器试图创建 `ClassA` 时，它需要先创建 `ClassB`。
- 而在创建 `ClassB` 时，容器又需要先创建 `ClassA`。





### 循环依赖问题类型

在 Spring IoC 中， 循环依赖问题主要有以下三种情况：

* **构造器注入**：通过构造方法进行依赖注入时产生的循环依赖问题。
* **setter 注入原型模式**: 通过setter方法进行依赖注入且是在多例(原型)模式下产生的循环依赖问题。
* **setter 注入单例模式**: 通过setter方法进行依赖注入且是在单例模式下产生的循环依赖问题。

其中， 只有 **setter 注入单例模式**的循环依赖问题可以被解决，其他两种方式下的循环依赖问题无法解决。

在 **setter 注入单例模式**下， 循环依赖问题的解决方案主要有：

- 三级缓存机制：在属性注入中，容器首先创建 Bean 的实例（无需立即填充依赖），然后在实例化之后处理其依赖项，这样可以打破循环。
- 使用懒加载 + 代理对象



### 为什么部分情况下的循环依赖问题无解

使用构造器注入时，循环依赖是不可解决的，因为依赖项必须在对象创建之前完成注入。如果有循环依赖，构造器注入会导致 `StackOverflowError` 或抛出异常。



原型 Bean 的生命周期不受 Spring 容器管理，它**每次请求时都会创建一个新的实例。**因此，Spring 容器无法像管理单例 Bean 那样，在初始化阶段解决依赖关系（包括循环依赖）。

**setter 注入原型模式**时， 原型 Bean 每次请求时都会生成新实例，<font color="red">Spring 无法提前将实例缓存起来进行注入</font>。







### 懒加载

`@Lazy` 解决循环依赖的关键点在于代理对象的使用。

**使用 `@Lazy` 的情况下**：Spring 不会立即创建 `B`，而是会注入一个 `B` 的代理对象。由于此时 `B` 仍未被真正初始化，`A` 的初始化可以顺利完成。等到 `A` 实例实际调用 `B` 的方法时，代理对象才会触发 `B` 的真正初始化。



但是，懒加载会让问题延迟暴露。不推荐使用 `@Lazy`。



## 二、三级缓存机制

Spring 的三级缓存机制主要用于解决**setter 注入单例模式下的循环依赖**问题，并保证在复杂依赖关系中，能够安全、高效地创建和注入 Spring Bean。三级缓存机制的核心思想是通过分层缓存，来支持在对象创建过程中，部分对象的提前暴露，从而打破循环依赖。以下是三级缓存的具体说明：



### 2.1 三级缓存的结构

三级缓存都是 Map 类型的缓存， 分别对应 `DefaultSingletonBeanRegistry`类 中的属性。

1. **一级缓存**（`singletonObjects`）：
   一级缓存是一个存放已经完成初始化的单例对象的缓存。当 Bean 完成所有初始化步骤后，它会被放入一级缓存中。在 Bean 的生命周期中，如果一级缓存中已经存在对应的 Bean，那么直接返回即可。
   
2. **二级缓存**（`earlySingletonObjects`）：
   二级缓存是一个<font color="red">**存放提前暴露的单例对象**</font>的缓存，即已经实例化但未被初始化的 Bean 。在某些情况下，一个对象的创建未完全完成（例如，未进行属性注入和后置处理器的处理），但为了打破循环依赖，需要提前暴露该对象。在这种情况下，该对象会被放入二级缓存中，**允许其他对象提前引用**。

3. **三级缓存**（`singletonFactories`）：

   三级缓存是一个存放创建 Bean 的工厂方法（`ObjectFactory`）的缓存。通过这个工厂方法，可以创建出尚未完全初始化的对象。**三级缓存的存在目的是允许在创建过程中，Bean 被代理（如通过 AOP 生成代理对象），并且可以按需获取对象**。

   ```java
   public interface ObjectFactory<T> {
       T getObject() throws BeansException;
   }
   ```
   
   



### 2.3 三级缓存工作流程

当 Spring 容器创建单例 Bean 时，会依次检查三级缓存，确保 Bean 能够顺利地完成创建：

1. **查找一级缓存**：
   容器首先检查一级缓存 `singletonObjects`，如果目标 Bean 已经存在并完成了初始化，直接返回。
2. **查找二级缓存**：
   如果一级缓存没有找到，容器会检查二级缓存 `earlySingletonObjects`。如果该 Bean 已经被提前暴露（但尚未完全初始化），从二级缓存中获取并返回。
3. **查找三级缓存**：
   如果二级缓存也没有找到，容器会检查三级缓存 `singletonFactories`。三级缓存中存放的是创建该 Bean 的工厂方法。如果存在工厂方法，容器会通过该工厂方法生成一个早期引用，并将生成的对象放入二级缓存 `earlySingletonObjects`，然后返回。
4. **完成 Bean 的初始化**：
   如果三级缓存中找到并生成了早期引用，Spring 继续完成该 Bean 的依赖注入和初始化操作。当 Bean 完成初始化后，将其移入一级缓存 `singletonObjects`，并从二级缓存和三级缓存中移除。



### 2.4 为什么不能二级缓存

`objectFactory` 是 Spring 用来管理 Bean 延迟实例化的一个工具，通常结合缓存机制来确保 Bean 只在需要时才被创建，避免重复创建，并在 AOP 环境下管理代理对象。



在没有 AOP 的情况下，确实可以只使用一级和三级缓存来解决循环依赖问题。

但是，当涉及到 AOP 时，如果只有一级缓存和三级缓存，那么 Spring 将无法保存 三级缓存中存储的`ObjectFactory`（即生成代理对象的工厂）调用 `getObject` 方法生成前期暴露对象，这就意味着**每次访问 Bean 时都可能会重新生成代理对象**。这不仅会增加计算开销，还会导致不同的代理实例，而这与 AOP 的设计初衷不符（AOP 代理应该是对同一个 Bean 实例的装饰）。





## 参考资料

[Spring常见面试题总结 | JavaGuide](https://javaguide.cn/system-design/framework/spring/spring-knowledge-and-questions-summary.html#spring-的循环依赖)

