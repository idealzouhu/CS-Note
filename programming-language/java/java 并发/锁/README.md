### Violate 关键字

[Java 内存模型.md](Java 内存模型.md) ：Java 内存模型解决了可见性和有序性

[volatile 关键字.md](volatile 关键字.md) 



### 锁的基础

[什么是锁](什么是锁.md) ：

[锁的分类](锁的分类.md) : 列举锁在不同角度上的分类，并重点介绍常见的锁。

[怎么实现锁](怎么实现锁.md) ：列举 Java 里面常用的 volatile、synchronized、reentranlock

[乐观锁实现机制](乐观锁实现机制.md) ：给出乐观锁的实现方式(版本号、CAS算法)



### 内置锁

[什么是 Java 内置锁.md](什么是 Java 内置锁.md) ：每个 Java 对象都可以用作一个内置锁。线程进入同步代码块或方法时会自动获得该锁，在退出同步代码块或方法时会释放该锁。

[synchronized 的使用.md](synchronized 的使用.md) ：synchronized 是 Java 内置锁的实现方式。



### 显示锁

一般锁进行并发控制的规则为 读读互斥、读写互斥、写写互斥， 读写锁进行并发控制的规则为**读读不互斥**、读写互斥、写写互斥。

[什么是 Java 显示锁.md](什么是 Java 显示锁.md) ：介绍 Java 显示锁的 Lock 接口，并给出显示锁和内置锁的区别。

[ReentranLock 显示锁.md](ReentranLock 显示锁.md) ：简单地介绍 ReentranLock 显示锁。

 [ReentranReadWriteLock 显示锁.md](ReentranReadWriteLock 显示锁.md) 





### CAS 算法

 [CAS 算法.md](CAS 算法.md) ：CAS( Compare And Swap)  是一种保障变量操作原子性的无锁算法，可以在多个线程之间实现对共享数据的安全更新。

 [Unsafe 类.md](Unsafe 类.md) ：UnSafe 类封装了 CPU 的底层原子操作，从而提供 CAS 方法。

 [Atomic 原子类.md](Atomic 原子类.md) ：原子类利用 CAS 算法和 Violate 来保证变量的原子性和可见性， 可以更新基本类型的变量而无需额外的同步



### AQS 队列

无论是单体服务应用内部的锁，还是分布式环境下多体服务应用所使用的分布式锁，为了减少由于无效争夺导致的资源浪费和性能恶化，一般都**基于队列进行排队与削峰**。







### 线程同步工具

[CountDownLatch 类.md](CountDownLatch 类.md) ：作为倒计时器来使用，让一个或多个线程等待其他线程完成某些操作后再继续执行。



### 细节补充

 [lock在代码里的书写规范](lock在代码里的书写规范.md) : lock.lock() 为什么写到try语句外，lock.unlock() 放到 finally 语句块 