`ReentrantLock` 实现了 `Lock` 接口，是一个可重入且独占式的锁

`ReentrantLock` 更灵活、更强大，增加了轮询、超时、中断、公平锁和非公平锁等高级功能。



### [ReentrantLock 是什么？](https://javaguide.cn/java/concurrent/java-concurrent-questions-02.html#reentrantlock-是什么)





### 注意事项

在使用多线程时，正确做法是**在类级别声明一个 ReentrantLock 实例**， 这样可以确保所有调用 add 方法的线程都能正确地获取并释放同一个锁，从而避免数据竞争。

```java
public class SafeAccumalatorOne {
    private int sum = 0;
    private Lock lock = new ReentrantLock();

    public void add(int value) {
        lock.lock();
        try {
            sum += value;
        } finally {
            lock.unlock();
        }
    }

    public int getSum() {
        return sum;
    }
}
```



如果在方法里面创建了一个新的 ReentrantLock 实例，这意味着每个调用实际上都在使用不同的锁。这导致了锁失去了它原本的作用——即保护共享资源免受多个线程同时访问。

```java
public class SafeAccumalatorOne {
    private int sum = 0;

    public void add(int value) {
        Lock lock = new ReentrantLock(); // 会出现线程安全问题
        lock.lock();
        try {
            sum += value;
        } finally {
            lock.unlock();
        }
    }

    public int getSum() {
        return sum;
    }
}
```

