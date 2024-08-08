### 并发编程

[并发编程三大特性.md](并发编程三大特性.md)  ：并发编程的特性主要包括原子性、有序性、可见性。

[Java 内存模型.md](Java 内存模型.md) ：Java 内存模型解决了可见性和有序性

[怎么实现锁](怎么实现锁.md) ：列举 Java 里面常用的 volatile、synchronized、reentranlock

[乐观锁实现机制](乐观锁实现机制.md) ：给出乐观锁的实现方式(版本号、CAS算法)



### 进程和线程

 [JVM 层次的进程.md](JVM 层次的进程.md) 

 [线程状态和变化.md](线程状态和变化.md) ：了解线程在 JVM 里面的状态变化流程。

 [线程创建的常见方式.md](线程创建的常见方式.md) ：介绍常见的 4 种线程创建方式，并给出相应的源码解析。

 [Thread 类的属性和方法.md](Thread 类的属性和方法.md) ：给出线程的基本属性和常用方法

 [ThreadLocal 类.md](ThreadLocal 类.md) ：分析 ThreadLocal 类的底层实现原理



### 线程池

 [JUC 的线程池架构.md](JUC 的线程池架构.md) ：给出 JUC( java.util.concurrent ) 的线程池框架的类图，并简单介绍各个类之间的联系

[线程池参数解析.md](线程池参数解析.md) ：分析线程池参数的具体含义和不同参数之间的联系，并给出一个线程池的简单使用案例 

[线程池命名.md](线程池命名.md) ：初始化线程池的时候需要显示命名（设置线程池名称前缀），有利于定位问题。



### 锁的基础

[什么是锁](什么是锁.md) ：

[锁的分类](锁的分类.md) : 列举锁在不同角度上的分类，并重点介绍常见的锁。



### 内置锁

[什么是 Java 内置锁.md](什么是 Java 内置锁.md) ：每个 Java 对象都可以用作一个内置锁。线程进入同步代码块或方法时会自动获得该锁，在退出同步代码块或方法时会释放该锁。

[synchronized 的使用.md](synchronized 的使用.md) ：synchronized 是 Java 内置锁的实现方式。



### 显示锁

[什么是 Java 显示锁.md](什么是 Java 显示锁.md) ：介绍 Java 显示锁的 Lock 接口，并给出显示锁和内置锁的区别。

[ReentranLock 显示锁.md](ReentranLock 显示锁.md) ：简单地介绍 ReentranLock 显示锁。



### CAS 算法

 [CAS 算法.md](CAS 算法.md) ：CAS( Compare And Swap)  是一种保障变量操作原子性的无锁算法，可以在多个线程之间实现对共享数据的安全更新。

 [Unsafe 类.md](Unsafe 类.md) ：UnSafe 类封装了 CPU 的底层原子操作，从而提供 CAS 方法。

 [Atomic 原子类.md](Atomic 原子类.md) ：原子类利用 CAS 算法和 Violate ，保证了变量的原子性和可见性。



### 工具类

[CountDownLatch 类.md](CountDownLatch 类.md) ：作为倒计时器来使用，让一个或多个线程等待其他线程完成某些操作后再继续执行。





### 细节补充

 [如何在项目里面使用并发技术.md](如何在项目里面使用并发技术.md) 

 [重要考点](重要考点.md) ： 列举一些比较重要的考点

 [lock在代码里的书写规范](lock在代码里的书写规范.md) : lock.lock() 为什么写到try语句外，lock.unlock() 放到 finally 语句块 