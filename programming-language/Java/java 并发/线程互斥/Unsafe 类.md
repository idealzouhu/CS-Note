[TOC]

## 一、Unsafe 类概述

### 1.1 什么是 Unsafe 类

`Unsafe` 是一个由 `sun.misc` 包提供的类，属于 Java 的内部 API。

`Unsafe` 类提供了一些**低级别的、不安全的底层操作**，如直接访问系统内存资源内存、自主管理内存资源等。`Unsafe` 大量的方法都是原生（native）方法，基于 C++语言实现，可以使用指针操作内存空间。这些方法在提升Java运行效率、增强Java语言底层资源操作能力方面起到了很大的作用。但是，这增加了指针相关问题、内存泄露问题的出现概率，导致 Unsafe 变得 “危险” 。

`Unsafe` 类是特定于 JVM 实现的，不属于标准 Java API。因此，不保证在所有 JVM 实现中都可用。

 

### 1.2 Unsafe 类中 CAS 无锁方法的底层原理

操作系统层面的 CAS 是一条 CPU 的原子指令（ cmpxchg 指令），正是由于该指令具备了原子性，因此使用 CAS 操作数据时不会造成数据不一致的问题。

Unsafe 提供的 CAS 方法<font color="red">**直接通过 native方式 （封装 C++ 代码）调用了底层的 CPU 指令 cmpxchg**</font> 。



## 二、如何使用 CAS 操作

要在 Java 应用层完成 CAS（Compare-And-Swap）操作，主要涉及以下 `Unsafe` 方法调用：

1. 获取 `Unsafe` 实例。
2. 调用 `Unsafe` 提供的字段偏移量方法，以获取对象中的字段偏移量。此字段偏移量值将会作为 CAS 方法的参数。
3. 调用 `Unsafe` 提供的 CAS 方法，进行原子操作。



### 2.1 获取 Unsafe 实例

`Unsafe` 类是一个 final 修饰的不允许继承的最终类，而且其构造函数是 private 类型的方法。

```java
public final class Unsafe {
	private static final Unsafe theUnsafe = new Unsafe();
	...
}
```

因此，我们无法在外部对 `Unsafe` 实例化，**只能通过反射获取 `Unsafe`  实例**。

```java
 Field field = Unsafe.class.getDeclaredField("theUnsafe");
 field.setAccessible(true);
 unsafe = (Unsafe) field.get(null);
```



### 2.2 获取字段偏移量

Unsafe提供的获取字段（属性）偏移量的相关操作主要如下：

```java
/**
 * 获取静态字段的偏移量
 * @param field 静态字段的反射对象
 * @return 字段的偏移量
 */
public long getStaticFieldOffset(Field field) {
    return unsafe.staticFieldOffset(field);
}

/**
     * 获取实例字段的偏移量
     * @param field 实例字段的反射对象
     * @return 字段的偏移量
     */
public long getObjectFieldOffset(Field field) {
    return unsafe.objectFieldOffset(field);
}
```



### 2.3 调用 CAS 方法

`Unsafe` 类提供了 CAS 方法主要有  `compareAndSwapObject` 、 `compareAndSwapInt` 、 `compareAndSwapLong` 。其中，`compareAndSwapInt` 方法用于对指定对象的指定偏移量处的整数值进行原子更新， 具体实现如下：

```java
/**
 * Atomically updates Java variable to {@code x} if it is currently
 * holding {@code expected}.
 *
 * <p>This operation has memory semantics of a {@code volatile} read
 * and write. Corresponds to C11 atomic_compare_exchange_strong.
 *
 * @param o the object containing the integer field
 * @param offset the offset of the integer field within the object
 * @param expected the expected value of the integer field
 * @param x the new value to set if the current value equals the expected value
 * @return {@code true} if the update was successful, {@code false} otherwise
 */
@ForceInline
public final boolean compareAndSwapInt(Object o, long offset, int expected, int x) {
    return theInternalUnsafe.compareAndSetInt(o, offset, expected, x);
}

```

此方法实现原子更新：

- 检查对象 `o` 在偏移量 `offset` 处的整数值是否为 `expected`
- 若是，则更新该值为 `x` 并返回 true ；否则返回 false。
- 方法具有 volatile 读写内存语义，确保了可见性和有序性。对应C11标准中的atomic_compare_exchange_strong。



### 2.4 完整使用案例

```java
import sun.misc.Unsafe;
import java.util.concurrent.CountDownLatch;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.lang.reflect.Field;

/**
 * 利用CAS实现线程安全累加
 * <p>
 *     使用CAS无锁编程算法实现一个轻量级的安全自增实现版本. 每条线程通过 CAS 自旋对共享数据进行自增操作.
 * </p>
 *
 * @author zouhu
 * @data 2024-08-07 13:11
 */
public class SafeAccumalatorTwo {
    private volatile int sum = 0;
    private static final Unsafe unsafe;
    private static final long sumOffset;

    /**
     * 获取Unsafe实例
     * <p>
     *     静态初始化块（static initializer block）, 在类加载的时候执行，只执行一次。
     *     主要用于初始化类的静态成员变量，比如静态变量、静态方法等。
     * <p
     *     Unsafe是Java虚拟机提供的一个类，它提供了一些底层的、非Java语言的接口，允许Java程序直接调用操作系统的底层服务。
     * <p>
     *     通过反射获取Unsafe实例，以获取CAS操作所需的偏移量.
     *     获取偏移量是为了能够通过Unsafe的compareAndSwapInt方法来更新共享变量的值.
     *     通过Unsafe的objectFieldOffset方法获取共享变量sum的偏移量.
     *
     */
    static {
        try {
            // 获取反射的 Unsafe实例
            Field field = Unsafe.class.getDeclaredField("theUnsafe");
            field.setAccessible(true);
            unsafe = (Unsafe) field.get(null);
            // 获取共享变量sum的偏移量
            sumOffset = unsafe.objectFieldOffset
                    (SafeAccumalatorTwo.class.getDeclaredField("sum"));
        } catch (Exception e) {
            throw new Error(e);
        }
    }

    /**
     * 使用 CAS 自增
     * <p>
     *     add方法使用了while循环来尝试更新sum的值，直到成功为止.
     *     如果更新失败，则继续尝试，直到成功为止.
     *     其中，通过compareAndSwapInt方法实现了CAS操作，保证了累加操作的线程安全性。
     * </p>
     *
     * @param value
     */
    public void add(int value) {
        while (true) {
            int current = sum;
            if (unsafe.compareAndSwapInt(this, sumOffset, current, current + value)) {
                break;
            }
        }
    }

    public int getSum() {
        return sum;
    }

    public static void main(String[] args) throws InterruptedException {
        SafeAccumalatorTwo accumulator = new SafeAccumalatorTwo();
        final int threadCount = 100;
        final int incrementValue = 1;

        ExecutorService executor = Executors.newFixedThreadPool(threadCount);
        CountDownLatch latch = new CountDownLatch(threadCount);

        for (int i = 0; i < threadCount; i++) {
            executor.submit(() -> {
                for (int j = 0; j < 100; j++) {
                    accumulator.add(incrementValue);
                }
                latch.countDown();
            });
        }

        latch.await(); // 等待所有线程完成
        executor.shutdown();

        // 预期结果应该是 100 * 100 * incrementValue
        System.out.println("Expected: " + (100 * 100 * incrementValue));
        System.out.println("Actual: " + accumulator.getSum());
    }
}
```





## 参考资料

《 极致经典（卷2）：Java高并发核心编程(卷2 加强版) -特供v21-release》

