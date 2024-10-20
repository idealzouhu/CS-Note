### 什么是 Semaphore

`Semaphore`（信号量）是一种用于控制访问共享资源的同步机制。它通常用于**限制同时访问特定资源的线程数量**，从而防止资源竞争和冲突。

`Semaphore` 实际上是共享锁的一种特殊实现。



### Semaphore 的工作原理

`Semaphore` 维护了一个许可（permits）计数器，**表示可以访问资源的“许可证”数量**。

当一个线程想要访问资源时，它必须从信号量中获取一个许可。如果信号量的计数大于0，线程可以成功获取许可，并且信号量的计数减1。如果信号量的计数为0，线程将阻塞，直到有其他线程释放许可，使计数增加。



```java
public class Semaphore implements java.io.Serializable {
	
	// 同步状态，等于许可证数量
	private volatile int state;
    
    // 获取许可证
      public void acquire(int permits) throws InterruptedException {
        if (permits < 0) throw new IllegalArgumentException();
        sync.acquireSharedInterruptibly(permits);
    }
    
    // 释放许可证
    public void release(int permits) {
        if (permits < 0) throw new IllegalArgumentException();
        sync.releaseShared(permits);
    }

}
```





### Semaphore 的工作模式

Semaphore 支持公平锁和非公平锁， 通过构造方法中的参数来设置。

```java
public class Semaphore implements java.io.Serializable {
	
	private final Sync sync;
	
	// fair 字段决定使用公平锁或者非公平锁
	public Semaphore(int permits, boolean fair) {
        sync = fair ? new FairSync(permits) : new NonfairSync(permits);
    }
}
```

