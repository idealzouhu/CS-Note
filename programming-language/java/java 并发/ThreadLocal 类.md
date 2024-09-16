## 一、ThreadLocal 概述

### 为什么引入 ThreadLocal

ThreadLocal 是 Java 中用于解决线程间变量隔离的一个工具类，它**允许每个线程拥有自己独立的变量副本**，从而避免了线程安全问题。



### 应用场景

ThreadLocal 的应用场景主要有：

- **线程本地变量存储**：在 Web 应用中，每个请求通常在一个独立的线程中处理。可以使用 ThreadLocal 来存储请求相关的数据，如用户身份、事务 ID 或者请求上下文等，这样每个线程都可以访问自己的数据副本而不会影响其他线程。
- **线程本地日志记录器**：在日志记录中，为了提高性能，可以为每个线程分配一个本地的日志记录器实例，避免在日志记录时的锁竞争。

- **线程本地数据库连接**：在数据库操作中，为每个线程提供一个独立的数据库连接，可以避免连接共享导致的线程安全问题。
- **线程本地日期格式化**：SimpleDateFormat 不是线程安全的，因此在多线程环境中使用 ThreadLocal 来存储每个线程的日期格式化器可以避免线程安全问题。



### 使用案例

在 Web 应用中，每个请求通常在一个独立的线程中处理。可以**使用 ThreadLocal 来存储请求相关的数据**，如用户身份、事务 ID 或者请求上下文等，这样每个线程都可以访问自己的数据副本而不会影响其他线程。

```java
public class RequestContextHolder {
    private static final ThreadLocal<RequestContext> contextHolder = new ThreadLocal<>();

    public static void setRequestContext(RequestContext context) {
        contextHolder.set(context);
    }

    public static RequestContext getRequestContext() {
        return contextHolder.get();
    }

    public static void clearRequestContext() {
        contextHolder.remove();
    }

    public static void main(String[] args) {
        // 创建并设置请求上下文
        RequestContext context = new RequestContext();
        context.setUserId("user123");
        RequestContextHolder.setRequestContext(context);

        // 获取请求上下文
        RequestContext retrievedContext = RequestContextHolder.getRequestContext();
        System.out.println("Request ID: " + retrievedContext.getRequestId());
        System.out.println("User ID: " + retrievedContext.getUserId());

        // 清理线程局部变量
        RequestContextHolder.clearRequestContext();

    }
}
```



## 二、ThreadLocal 的实现原理

### 线程 Thread 的局部变量存储机制

<font color="red">**ThreadLocal  依赖于每个线程附带的独立哈希表**</font> 。这意味着每个线程都有自己的哈希表来存储与该线程相关的 ThreadLocal 变量。这些哈希表分别是 `Thread.threadLocals` 和 `Thread.inheritableThreadLocals`，分别用于普通线程局部变量和可继承的线程局部变量。`Thread` 的部分代码如下：

```java
public class Thread implements Runnable {
	/* ThreadLocal values pertaining to this thread. This map is maintained
     * by the ThreadLocal class. */
    ThreadLocal.ThreadLocalMap threadLocals = null;

    /*
     * InheritableThreadLocal values pertaining to this thread. This map is
     * maintained by the InheritableThreadLocal class.
     */
    ThreadLocal.ThreadLocalMap inheritableThreadLocals = null;
    
    ...
}
```

**每个 ThreadLocal 对象在哈希表中充当 Key 的角色**，而线程局部变量的值则作为对应的值存储。这意味着每个 ThreadLocal 对象在每个线程的哈希表中都有一个唯一的条目。

> 实际上`key`并不是`ThreadLocal`本身，而是它的一个**弱引用**



### ThreadLocalMap 数据结构

每个线程在往 `ThreadLocal` 里放值的时候，都会往自己的 `ThreadLocalMap` 里存，读也是以 `ThreadLocal` 作为引用，在自己的 `map` 里找对应的 `key` ，从而实现了**线程隔离**。

![img](images/2-CFHd4NU8.png)



ThreadLocal 的部分源代码如下：

```java
public class ThreadLocal<T> {
	static class ThreadLocalMap {
	
		static class Entry extends WeakReference<ThreadLocal<?>> {
            /** The value associated with this ThreadLocal. */
            Object value;

            Entry(ThreadLocal<?> k, Object v) {
                super(k);
                value = v;
            }
        }
        ...
	}
	...
}
```





### ThreadLocal 如何将值存放到线程

在 ThreadlLocal 的 `set()` 和 `get()` 方法中,  使用 `Thread.currentThread()` 获取当前正在运行的值，存放和获取线程中的值，从而保证不会被其他的线程所读取。

```java
 public void set(T value) {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null) {
            map.set(this, value);
        } else {
            createMap(t, value);
        }
    }
```









## 三、ThreadLocal 的局限性



### 可能存在的内存泄露问题

当在线程池里面使用 `ThreadLocal` 时，内存可能会存在泄露，原因来自以下几个方面：

- **强引用问题**： `ThreadLocalMap` 的键是 `ThreadLocal` 的弱引用（`WeakReference`），而值是强引用。如果 `ThreadLocal` 对象被垃圾回收，而<font color="red">**线程还在运行，那么相应的值无法被垃圾回收**</font>，因为值是强引用，导致内存泄露。

- **线程池和长生命周期的线程**： 通常情况下，当线程结束时，它的所有局部变量都会被回收。但是在使用线程池时，线程不会结束，而是被重用。如果我们没有手动清理 `ThreadLocal`，其存储的值可能长期存在，导致内存泄露。

解决方案有：

- 使用弱引用。

- **手动清理**，显式调用 `remove()` 函数。

  ```java
  threadLocal.remove(); // 在任务结束后手动清理
  ```

  





### 线程池中存在变量污染问题

在线程池中使用 `ThreadLocal` 变量可能导致**数据混乱**，因为多个任务在同一个线程中执行，它们共享了同一个 `ThreadLocal`。当线程池中的线程执行任务后，如果不清理 `ThreadLocal`，后续任务可能会继承之前任务中的数据，导致上下文污染。





### 子线程无法继承父线程的变量

ThreadLocal 的不可继承性是指在子线程中无法直接继承父线程的 ThreadLocal 变量。这意味着，如果在父线程中设置了 ThreadLocal 变量的值，这个值在子线程中默认是不可见的。这种不可继承性可能会在某些情况下带来困扰，特别是在使用线程池、异步任务或者通过Thread.join()等方式创建子线程的情况下。


> InheritableThreadLocal 可以解决 ThreadLocal 的不可继承性，但是无法避免内存泄露和变量污染





## 四、InheritableThreadLocal 概述

### 4.1 InheritableThreadLocal 和 ThreadLocal 的区别

每个线程都会有一个独立的副本，线程无法访问其他线程的 `ThreadLocal` 值。父线程设置的 `ThreadLocal` 值不会被子线程继承。

然而， InheritableThreadLocal 解决了不可继承性这一问题。



### 4.2 InheritableThreadLocal 的实现原理

`InheritableThreadLocal` 继承自 `ThreadLocal`，其核心不同之处在于子线程创建时，它会将父线程的 `ThreadLocal` 值拷贝一份到子线程的 `ThreadLocalMap` 中。

在创建子线程时， `Thread` 构造方法会调用父线程的 `inheritableThreadLocals`，将其拷贝给子线程。具体代码细节如下：

```java
public class ThreadLocal<T> {
	  public void set(T value) {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null) {
            map.set(this, value);
        } else {
            createMap(t, value);
        }
    }
}

public class InheritableThreadLocal<T> extends ThreadLocal<T> {
	// 返回存储 key 为 InheritableThreadLocal 类型的 map
     ThreadLocalMap getMap(Thread t) {
       return t.inheritableThreadLocals;
    }
    
    // ThreadLocalMap 创建方法
	 void createMap(Thread t, T firstValue) {
        t.inheritableThreadLocals = new ThreadLocalMap(this, firstValue);
    }
}




public class Thread implements Runnable {
	ThreadLocal.ThreadLocalMap inheritableThreadLocals = null;
    
    // 线程的构造方法
     private Thread(ThreadGroup g, Runnable target, String name,
                   long stackSize, AccessControlContext acc,
                   boolean inheritThreadLocals) {
         
         ...
          // 继承父线程的 inheritableThreadLocals 到当前线程
          if (inheritThreadLocals && parent.inheritableThreadLocals != null)
            this.inheritableThreadLocals =
                ThreadLocal.createInheritedMap(parent.inheritableThreadLocals);
     }
}

```





### 4.3 InheritableThreadLocal 存在问题

**线程复用问题**：线程池中的线程是被复用的。当线程池中的线程执行任务后，如果不清理 `ThreadLocal`，后续任务可能会继承之前任务中的数据，导致上下文污染。

**动态上下文传递问题**： `InheritableThreadLocal` 的传递机制是在子线程创建时完成的，这意味着如果父线程的 `ThreadLocal` 值在子线程创建后发生变化，这个变化不会传递给已经创建的子线程。





## 五、TransmittableThreadLocal 概述

[`TransmittableThreadLocal`](https://github.com/alibaba/transmittable-thread-local/blob/master/ttl-core/src/main/java/com/alibaba/ttl3/TransmittableThreadLocal.java) 保存线程的值，并跨线程池传递

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>transmittable-thread-local</artifactId>
    <version>2.14.4</version>
</dependency>
```



### 5.1 什么是 TransmittableThreadLocal 

`TransmittableThreadLocal`(`TTL`)：在使用线程池等会池化复用线程的执行组件情况下，提供`ThreadLocal`值的传递功能，**解决异步执行时上下文传递的问题**。

`TransmittableThreadLocal`继承`InheritableThreadLocal`，使用方式也类似。相比`InheritableThreadLocal`，添加了 `protected` 的`transmitteeValue()`方法，用于<font color="red">定制 **任务提交给线程池时** 的`ThreadLocal`值传递到 **任务执行时** 的传递方式</font>，缺省是简单的赋值传递。





### 5.1 TTL 实现原理

从实现线程变量传递的原理上来看，TTL 做的实际上就是将原本与 Thread 绑定的线程变量，缓存一份到 TtlRunnable 对象中，在执行子线程任务前，将对象中缓存的变量值设置到子线程的 ThreadLocal 中以供 run() 方法的代码使用，然后执行完后，又恢复现场，保证不会对复用线程产生影响。

[TransmittableThreadLocal（TTL）实现线程变量传递的原理分析_ttlthreadlocal-CSDN博客](https://blog.csdn.net/wk52525/article/details/107859685)



![时序图](images/233595980-ef7f1f8b-36cd-45b3-b55b-45f7b3d1c94f.png)





### 使用案例

为了保证线程池中传递值， 我们有以下方式：

- 修饰`Runnable`和`Callable`
- 修饰线程池









## 参考资料

[解锁ThreadLocal的问题集：如何规避多线程中的坑_threadlocal多线程的坑-CSDN博客](https://blog.csdn.net/Mrxiao_bo/article/details/136208633)

[面试官：小伙子，听说你看过ThreadLocal源码？（万字图文深度解析ThreadLocal） - 掘金 (juejin.cn)](https://juejin.cn/post/6844904151567040519)

[alibaba/transmittable-thread-local: 📌 a missing Java std lib(simple & 0-dependency) for framework/middleware, provide an enhanced InheritableThreadLocal that transmits values between threads even using thread pooling components. (github.com)](https://github.com/alibaba/transmittable-thread-local)