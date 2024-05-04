## Java 并发基础

[并发编程特性](https://javaguide.cn/java/concurrent/jmm.html#再看并发编程三个重要特性)：原子性，可见性，有序性

[什么是锁](什么是锁.md) ：

[锁的分类](锁的分类.md) : 列举锁在不同角度上的分类，并介绍一些常见的分类。

[怎么实现锁](怎么实现锁.md) ：列举 Java 里面常用的 volatile、synchronized、reentranlock

[乐观锁实现机制](乐观锁实现机制.md) ：给出乐观锁的实现方式(版本号、CAS算法)



## Java 线程池

 [线程池](线程池.md) ：给出线程池的实现模板和常见参数，以及如何动态选择线程池的核心参数





## Java 并发观点

 [实际项目中使用并发技术.md](实际项目中使用并发技术.md) 

[(1 封私信) 如何学习Java“高并发”，并在项目中实际应用？ - 知乎 (zhihu.com)](https://www.zhihu.com/question/64948142)： 这个观点还是挺有意思的





## 细节补充

 [lock在代码里的书写规范](lock在代码里的书写规范.md) : lock.lock() 为什么写到try语句外，lock.unlock() 放到 finally 语句块 