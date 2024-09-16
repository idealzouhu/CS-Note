### 为什么引入 Java 显示锁

Java 内置锁使用起来比较方便。Java 内置锁使用 `synchronized` 关键字进行简单的加锁和解锁，开发者无需手动管理锁的获取和释放，降低了编程复杂度。这些工作由 JVM 底层完成，而且任何一个 Java 对象都能作为一个内置锁使用。

但是，**Java 内置锁的功能相对单一**，不具备一些比较高级的锁功能，比如： 

- **限时抢锁**：Java 内置锁不支持设置超时机制，线程在竞争锁时可能会无限等待。

- **可中断抢锁**：内置锁不支持中断功能，无法在等待锁时被中断，可能导致线程无法及时释放。

- **多个等待队列**：内置锁没有内建多个等待队列的机制，导致在特定情况下（如生产者-消费者模式）效率较低。

**Java 内置锁还存在性能问题**。在高竞争环境下，Java 对象锁会转变为重量级锁，这依赖于操作系统的 Mutex Lock 实现。重量级锁的线程阻塞和唤醒操作需要频繁在用户态和内核态之间切换，导致性能下降。

为了克服 Java 内置锁的限制，显示锁应运而生。





### 什么是 Java 显示锁

相较于 Java 内置锁使用已有对象，Java 显示锁直接创建一个对象（锁）。

Java 显示锁( Explicit Lock）抽象成 [Lock](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/concurrent/locks/Lock.html) 接口，位于 `java.util.concurrent` 并发包。

| 方法                                           | 描述                                                         |
| ---------------------------------------------- | ------------------------------------------------------------ |
| `void lock()`                                  | 获取锁。如果锁已被其他线程持有，当前线程将阻塞，直到锁变为可用。此方法不支持中断。 |
| `void lockInterruptibly()`                     | 获取锁，当前线程可以在等待过程中被中断。如果在等待锁时被中断，将抛出 `InterruptedException`。 |
| `boolean tryLock()`                            | 尝试获取锁。如果锁当前可用，获取锁并返回 `true`；如果锁已被其他线程持有，则返回 `false`。 |
| `boolean tryLock(long timeout, TimeUnit unit)` | 尝试获取锁，如果在指定的超时时间内未能获取到锁，则返回 `false`。可以被中断。 |
| `void unlock()`                                | 释放锁。如果当前线程未持有该锁，则抛出 `IllegalMonitorStateException`。 |
| `Condition newCondition()`                     | 创建一个 `Condition` 对象，用于在持有锁时进行线程间的通信。  |



### Java 内置锁和 Java 显示锁的区别

内置锁是基于已有的 Java 对象进行的。任何对象都可以作为内置锁。

显示锁通常是通过创建一个锁对象来实现的。例如，使用 `ReentrantLock` 类需要显式创建一个 `ReentrantLock` 对象。这个锁对象管理其自身的锁状态，而不是依赖于任何已有的对象。