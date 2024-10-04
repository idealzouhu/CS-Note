### 锁的基础

[什么是锁](什么是锁.md) ：

[锁的分类](锁的分类.md) : 列举锁在不同角度上的分类，并重点介绍常见的锁。



### 内置锁

[synchronized 的使用.md](synchronized 的使用.md) ：synchronized 是 Java 内置锁的实现方式。

[Java 内置锁的底层实现原理.md](Java 内置锁的底层实现原理.md) ：每个 Java 对象都可以用作一个内置锁。线程进入同步代码块或方法时会自动获得该锁，在退出同步代码块或方法时会释放该锁。



### 显示锁

一般锁进行并发控制的规则为 读读互斥、读写互斥、写写互斥， 读写锁进行并发控制的规则为**读读不互斥**、读写互斥、写写互斥。

[什么是 Java 显示锁.md](什么是 Java 显示锁.md) ：介绍 Java 显示锁的 Lock 接口，并给出显示锁和内置锁的区别。

[ReentranLock 显示锁.md](ReentranLock 显示锁.md) ：简单地介绍 ReentranLock 显示锁。

[ReentranReadWriteLock 显示锁.md](ReentranReadWriteLock 显示锁.md) 



### 乐观锁

[乐观锁实现机制](乐观锁实现机制.md) ：给出乐观锁的实现方式(版本号、CAS算法)

[CAS 算法.md](CAS 算法.md) ：CAS( Compare And Swap)  是一种保障变量操作原子性的无锁算法，可以在多个线程之间实现对共享数据的安全更新。

[Unsafe 类.md](Unsafe 类.md) ：UnSafe 类封装了 CPU 的底层原子操作，从而提供 CAS 方法。

[Atomic 原子类.md](Atomic 原子类.md) ：原子类利用 CAS 算法和 Violate 来保证变量的原子性和可见性， 可以更新基本类型的变量而无需额外的同步



### AQS 队列

无论是单体服务应用内部的锁，还是分布式环境下多体服务应用所使用的分布式锁，为了减少由于无效争夺导致的资源浪费和性能恶化，一般都**基于队列进行排队与削峰**。

 [AQS 抽象队列同步器.md](AQS 抽象队列同步器.md) ：构建锁和同步容器的基础类，用于减少由于无效争夺导致的资源浪费和性能恶化。



### 细节补充

 [lock在代码里的书写规范](lock在代码里的书写规范.md) : lock.lock() 为什么写到try语句外，lock.unlock() 放到 finally 语句块 