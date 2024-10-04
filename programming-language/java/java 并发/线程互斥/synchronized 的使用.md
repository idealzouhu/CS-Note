### synchronized 使用方法

Synchronized 的使用大概分三种情况：

- **修饰实例方法（锁当前对象实例）**：也就是没有 static 修饰的方法，这时我们的锁对象就是当前执行该方法的对象；
- **修饰静态方法（锁当前类）**：就是 static 修饰的方法，这时我们的锁对象是我们的当前类的 Class 对象；
- **修饰代码块（锁指定对象/类）**：锁对象就是代码块锁的对象；

```java
public class TestSynchronized {
    private Object obj = new Object();

    // 修饰非 static 方法，锁对象为当前对象即 this
    public synchronized void test() {
        // 线程安全的代码
    }

    // 修饰 static 方法，锁对象为当前类 Class
    public synchronized static void say() {
        // 线程安全的代码
    }

    // 非静态方法中的代码块，锁对象为 obj
    public void add(int num) {
        synchronized (obj) {
            // 线程安全的代码
        }
    }
}

```



### synchronized 底层原理

[synchronized 底层原理了解吗？](https://javaguide.cn/java/concurrent/java-concurrent-questions-02.html#synchronized-底层原理了解吗)

[【Java 多线程】5 - 7 深入了解监视器锁 （讲解java 对象头、Monitor 对象）](https://www.cnblogs.com/zebt/articles/17737727.html)



### synchronized 最佳实践

- 尽量不要使用 `synchronized(String a)` 。因为 JVM 中，字符串常量池具有缓存功能。



### 参考资料

[【Java 多线程】5 - 7 深入了解监视器锁 （讲解java 对象头、Monitor 对象）](https://www.cnblogs.com/zebt/articles/17737727.html)

[Java并发常见面试题总结（中） | JavaGuide](https://javaguide.cn/java/concurrent/java-concurrent-questions-02.html#synchronized-关键字)

