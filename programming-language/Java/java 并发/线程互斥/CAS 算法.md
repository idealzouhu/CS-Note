[TOC]

## 一、CAS 算法概述

### 1.1 什么是 CAS 算法 

<font color="blue">**CAS( Compare And Swap)  是一种保障变量操作原子性的无锁算法，可以在多个线程之间实现对共享数据的安全更新** </font>，从而避免了锁带来的上下文切换开销和可能的死锁问题。

**Java 语言并没有直接实现 CAS**。CAS 相关的实现是直接通过 native方式 （封装 C++ 代码）调用了底层的 CPU 指令 cmpxchg。底层 CPU 利用原子操作 cmpxchg 判断内存原值与期望值是否相等，如果相等就给内存地址赋新值，否则不做任何操作。

另外，CAS 算法可能存在一些弊端：

- ABA 问题：无法确认变量是否修改过，通常使用版本号和标记来解决。
- 只能保证一个共享变量之间的原子性操作：一个比较简单的规避方法是把多个共享变量合并成一个共享变量来操作， 例如 AtomicReference 类。
- 无效 CAS 自旋会带来开销问题。



### 1.2 CAS 算法工作原理

CAS 操作涉及三个参数：

- **内存位置（V）**：需要被操作的变量的位置。

- **预期值（A）**：线程期望内存位置 V 中包含的值。

- **新值（B）**：需要写入内存位置 V 的值。

操作步骤如下：

1. 比较内存位置 V 当前的值是否等于预期值 A。
2. 如果相等，则将内存位置 V 的值更新为新值 B，并返回 `true`。
3. 如果不相等，则不做任何操作，返回 `false`。

使用 CAS 算法进行无锁编程的伪代码如下：

```pseudo
// CAS 操作函数
function CAS(pointer, expectedValue, newValue):
    // 原子地比较 pointer 处的值是否为 expectedValue
    if *pointer == expectedValue
        *pointer = newValue
        return true
    else:
        return false

function main():
    // 循环尝试进行 CAS 操作直到成功
    do:
        expectedValue = 获得当前 pointer 处的值
        newValue = 计算或获取新的值
    while not CAS(pointer, expectedValue, newValue)
```

其中，如果 CAS 失败的话，会重复进行 CAS 操作直到 CAS 操作成功，这种重复叫做 <font color="blue">**CAS 自旋**</font>





### 1.3 CAS 算法在 Java 中的应用

- `sun.misc`包 下的 Unsafe 类提供的 CAS 方法。
- Java 内置锁的轻量级锁状态。
- JDK 原子类基于CAS轻量级原子操作实现
- 无锁数据结构 `ConcurrentHashMap` 和 `ConcurrentLinkedQueue`
- 乐观锁

 



## 二、CAS 算法可能存在问题

如果使用得不合理，CAS 原子操作会存在ABA问题。



### 2.1 CAS 算法可能引发 ABA 问题

ABA 问题是一种在使用 CAS（Compare-And-Swap）操作时可能遇到的经典并发问题。它发生在以下情况下：

1. **线程 A** 读取一个共享变量的值为 **V1**。
2. **线程 B** 在此期间将该值从 **V1** 改为 **B**，然后又将其改回 **V1**。
3. 当 **线程 A** 再次尝试使用 CAS 更新该值时，它发现变量的值仍然是 **V1**，于是 CAS 操作成功执行，但实际上在这个过程中，值已经被 **线程 B** 修改过。

这种情况下，尽管 CAS 操作成功，但在此过程中发生了不一致性，因为<font color="red">**线程 A 在不知情的情况下认为没有其他线程对该值进行了修改**</font> 。





### 2.2 ABA 问题解决思路

ABA 问题解决思路如下：

- **版本号**：在共享数据结构中添加版本号，每次修改时更新版本号。这样，当线程 A 进行 CAS 操作时，不仅比较值，还比较版本号。

- **标记**：使用标记或状态字段来指示值的状态，当值被更新时，也更新标记信息。

版本号彻底解决 ABA 问题，能够区分数据的更新次数。而标记解决有限状态的 ABA 问题，无法知道数据的更新次数。



### 2.3 Java 中的解决方案

参考乐观锁的版本号，JDK 提供了一个类似 `AtomicStampedReference` 类来解决 ABA 问题。 AtomicStampReference **在 CAS 的基础上增加了一个 Stamp（印戳或版本号）**，使用这个印戳可以用来觉察数据是否发生变化，给数据带上了一种实效性的检验。

> `AtomicMarkableReference` 则是 `AtomicStampReference`  的简化版，使用 boolean 类型的标记 mark， 仅记录值是否修改过

`AtomicStampReference` 的 `compareAndSet()` 方法首先检查当前的对象引用值是否等于预期引用值， 并且当前印戳标志是否等于预期标志，如果全部相等，就以原子方式将引用值和印戳标志的值更新 为给定的更新值。

```java
public boolean compareAndSet( V expectedReference, 		//预期引用值
                              V newReference, 			//更新后的引用值
                              int expectedStamp, 		//预期印戳标志值
                              int newStamp) 		    //更新后的印戳标志值
```





## 三、CAS 算法性能提升

使用 LongAdder 类解决无效 CAS 自旋。





## 参考资料

[Java并发常见面试题总结（中） | JavaGuide](https://javaguide.cn/java/concurrent/java-concurrent-questions-02.html#如何实现乐观锁)

《 极致经典（卷2）：Java高并发核心编程(卷2 加强版) -特供v21-release》

