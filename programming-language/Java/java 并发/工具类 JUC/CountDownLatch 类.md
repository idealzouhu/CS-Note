### 什么是 CountDownLatch 类

CountDownLatch 是 Java 并发库中的一个实用类，用于解决多个线程之间的同步问题。它可以让一个或多个线程<font color="red">**等待其他线程完成某些操作后再继续执行**</font>。

`CountDownLatch ` 实际上是共享锁的一种特殊实现。





### CountDownLatch 类的原理

具体来说，CountDownLatch 提供了以下功能：

- 初始化计数器：在创建 CountDownLatch 实例时，需要指定一个初始计数。

  ```java
   public CountDownLatch(int count) {
          if (count < 0) throw new IllegalArgumentException("count < 0");
          this.sync = new Sync(count);
   }
  ```

- 减少计数器：当其他线程完成某个任务时，它们可以调用 countDown() 方法来减少计数器的值。

  ```
  public void countDown() {
          sync.releaseShared(1);
  }
  ```

- 等待所有任务完成：一个或多个线程可以调用 await() 方法来等待计数器变为零。

  ```
  public void await() throws InterruptedException {
       sync.acquireSharedInterruptibly(1);
   }
  ```

  

###  使用场景

- 启动服务前等待资源加载完成：主服务线程可以在启动之前等待其他线程加载必要的资源。
- 多线程数据处理：多个线程处理数据，主线程等待所有子线程处理完数据后再进行后续操作。
- 测试框架：在测试环境中，多个线程模拟并发请求，测试线程等待所有请求完成后再验证结果。



### 使用案例

```java
import java.util.concurrent.CountDownLatch;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class CountDownLatchExample {

    public static void main(String[] args) throws InterruptedException {
        ExecutorService executor = Executors.newFixedThreadPool(5);
        CountDownLatch latch = new CountDownLatch(5);

        for (int i = 0; i < 5; i++) {
            executor.submit(() -> {
                // 模拟耗时操作
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
                System.out.println(Thread.currentThread().getName() + " 完成");
                latch.countDown();
            });
        }

        // 等待所有任务完成
        latch.await();

        System.out.println("所有任务完成，继续执行后续逻辑...");
        executor.shutdown();
    }
}

```

在这个例子中，我们创建了一个 CountDownLatch 实例，并将其计数器设置为 5。接着，我们创建了一个线程池，并提交了 5 个任务。每个任务执行完毕后都会调用 countDown() 方法。最后，主线程调用 latch.await() 来等待所有任务完成。

