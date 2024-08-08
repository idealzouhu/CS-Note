## ThreadLocal 概述

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



## ThreadLocal 的具体细节

### 线程局部变量存储机制

<font color="red">**ThreadLocal  依赖于每个线程附带的独立哈希表**</font> 。这意味着每个线程都有自己的哈希表来存储与该线程相关的 ThreadLocal 变量。这些哈希表分别是 `Thread.threadLocals` 和 `Thread.inheritableThreadLocals`，分别用于普通线程局部变量和可继承的线程局部变量。Thread 的部分代码如下：

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



### 数据结构

每个线程在往 `ThreadLocal` 里放值的时候，都会往自己的 `ThreadLocalMap` 里存，读也是以 `ThreadLocal` 作为引用，在自己的 `map` 里找对应的 `key` ，从而实现了**线程隔离**。

![img](images/2-CFHd4NU8.png)



ThreadLocal 的部分源代码如下：

```
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



## ThreadLocalMap 的具体细节











### 参考资料

[面试官：小伙子，听说你看过ThreadLocal源码？（万字图文深度解析ThreadLocal） - 掘金 (juejin.cn)](https://juejin.cn/post/6844904151567040519)