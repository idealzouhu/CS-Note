### 什么是 CyclicBarrier 类

`CyclicBarrier` 是用来让一组线程在某个同步点（**屏障点**）等待，直到所有线程都到达这个点后再继续执行。它的作用类似于一个屏障，只有所有参与的线程都到达屏障后，才会解除阻塞，让这些线程继续执行。



### 工作原理

1. `CyclicBarrier` 的构造函数接受一个参数，指定需要等待的线程数。

   ```java
   // 每次指定指定需要等待的线程数
   private final int parties;
   
   // 计数器，记录仍在等待的参与方数量。每次初始值为 parties
   private int count;
   
   public CyclicBarrier(int parties) {
   this(parties, null);
   }
   ```

2. 所有线程调用 `await()` 方法后，将会阻塞，直到调用 `await()` 的线程数达到设定值，屏障才会打开，所有线程继续执行。

   ```java
   public int await() throws InterruptedException, BrokenBarrierException {
           try {
               return dowait(false, 0L);
           } catch (TimeoutException toe) {
               throw new Error(toe); // cannot happen
           }
   }
   ```

3. 当 `count` 数目为 0 时， 屏障将会重置，`count` 的值重新赋值为 parties。

   ```java
   private int dowait(boolean timed, long nanos)
   throws InterruptedException, BrokenBarrierException,
   TimeoutException {
       ...
       int index = --count;
       if (index == 0) {  // tripped
       	...
   		 nextGeneration();
            return 0;
       }
       ...
   }
   
   // 通知所有等待线程上一代已结束，为下一轮重置状态
   private void nextGeneration() {
       // signal completion of last generation
       trip.signalAll();
       // set up next generation
       count = parties;
       generation = new Generation();
   }
   ```

   



### CyclicBarrier 类和 CountdownLatch 类的区别

**循环使用**：`CyclicBarrier` 是可以重复使用的，即每次所有线程都到达屏障后，它会重置，下一次还可以再次使用。而 `CountDownLatch` 只能使用一次。

**屏障与计数器**：`CyclicBarrier` 是等待 **所有线程** 到达某个点，然后一起继续执行；`CountDownLatch` 则是线程等待计数器减到 0，才继续执行。

**附加操作**：`CyclicBarrier` 提供了一个额外的 `Runnable` 参数，允许在所有线程到达屏障时执行某个操作。`CountDownLatch` 没有这个功能。





### 使用案例

```java
public class CyclicBarrierExample {
    public static void main(String[] args) {

        // 创建 CyclicBarrier，指定 3 个线程，并且提供一个 Runnable，所有线程到达屏障时执行
        CyclicBarrier barrier = new CyclicBarrier(3, () -> {
            System.out.println("所有线程都已到达屏障，开始下一轮任务");
        });

        // 创建任务
        Runnable task = () -> {
            try {
                // 模拟第一轮任务
                System.out.println(Thread.currentThread().getName() + " 执行第一轮任务");
                Thread.sleep(1000); // 模拟任务执行时间
                System.out.println(Thread.currentThread().getName() + " 到达第一轮屏障");
                barrier.await(); // 等待所有线程到达屏障

                // 模拟第二轮任务
                System.out.println(Thread.currentThread().getName() + " 执行第二轮任务");
                Thread.sleep(1000); // 模拟任务执行时间
                System.out.println(Thread.currentThread().getName() + " 到达第二轮屏障");
                barrier.await(); // 等待所有线程到达屏障

            } catch (InterruptedException | BrokenBarrierException e) {
                e.printStackTrace();
            }
        };

        // 启动三个线程
        for (int i = 0; i < 3; i++) {
            new Thread(task).start();
        }
    }
}

 
```

输出结果如下：

```
Thread-0 执行第一轮任务
Thread-1 执行第一轮任务
Thread-2 执行第一轮任务
Thread-0 到达第一轮屏障
Thread-2 到达第一轮屏障
Thread-1 到达第一轮屏障
所有线程都已到达屏障，开始下一轮任务
Thread-1 执行第二轮任务
Thread-0 执行第二轮任务
Thread-2 执行第二轮任务
Thread-0 到达第二轮屏障
Thread-1 到达第二轮屏障
Thread-2 到达第二轮屏障
所有线程都已到达屏障，开始下一轮任务
```

