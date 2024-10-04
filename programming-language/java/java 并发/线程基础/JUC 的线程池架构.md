## 一、 JUC 线程池架构—— Executor 框架

### 1.1 架构设计

ThreadPoolExecutor 是 JUC 线程池的核心实现类。

<img src="images/image-20240719165532421.png" alt="image-20240719165532421" style="zoom:67%;" />



### Excutor 接口

`Executor` 是 Java 并发包 (`java.util.concurrent`) 中的一个核心接口，**提供了一个基本的任务执行框架**。

`Executor` 接口定义了一个执行任务的方法，<font color="red">**使任务的提交与任务的执行机制解耦**</font>。这种解耦允许开发者专注于任务的逻辑，而不需要关心任务是如何、何时以及在哪个线程中执行的。

```java
package java.util.concurrent;
public interface Executor {

    /**
     * Executes the given command at some time in the future.  The command
     * may execute in a new thread, in a pooled thread, or in the calling
     * thread, at the discretion of the {@code Executor} implementation.
     *
     * @param command the runnable task
     * @throws RejectedExecutionException if this task cannot be
     * accepted for execution
     * @throws NullPointerException if command is null
     */
    void execute(Runnable command);
}
```





### ExecutorService 接口

`ExecutorService` 是一个接口，继承自 `Executor`，它**定义了管理和控制任务执行的方法**。`ExecutorService` 实现类可以通过 `Executors` 工具类来创建。 

`ExecutorService` 提供了一些扩展功能，比如任务提交、任务调度、关闭线程池等。主要功能有：

- **任务提交**: 可以提交 `Runnable` 或 `Callable` 任务。
- **任务调度**: 提供方法来调度任务的执行时间和频率（如果是 `ScheduledExecutorService`）。
- **线程池管理**: 提供方法来管理线程池的生命周期，包括关闭线程池、等待线程池完成任务等。

`ExecutorService` 线程池**提交异步执行 target 目标任务的常用方法**主要为 execute() 和 submit() , 具体细节如下：

| 任务类型               | 方法                                                         | 说明                                                         |
| ---------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `Runnable` 任务        | `execute(Runnable command)`                                  | 提交一个不返回结果的任务                                     |
| `Callable` 任务        | `submit(Callable<T> task)`                                   | 提交一个有返回结果的任务，返回一个 `Future` 对象             |
| 定时 `Runnable` 任务   | `schedule(Runnable command, long delay, TimeUnit unit)`      | 提交一个在指定延迟后执行的任务                               |
| 定时 `Callable` 任务   | `schedule(Callable<V> callable, long delay, TimeUnit unit)`  | 提交一个在指定延迟后执行的任务，并返回一个 `Future` 对象     |
| 周期性 `Runnable` 任务 | `scheduleAtFixedRate(Runnable command, long initialDelay, long period, TimeUnit unit)` | 提交一个在初始延迟后开始执行，并以固定频率重复执行的任务     |
| 周期性 `Runnable` 任务 | `scheduleWithFixedDelay(Runnable command, long initialDelay, long delay, TimeUnit unit)` | 提交一个在初始延迟后开始执行，并在每次执行完后等待指定时间再执行 |



### AbstractExecutorService 抽象类

`AbstractExecutorService` 存在的目的是为 `ExecutorService` 中的接口提供默认实现。





### ThreadPoolExecutor 类

ThreadPoolExecutor 是 JUC 线程池的核心实现类。线程的创建和终止需要很大的开销，线程池中预先提供了指定数量的可重用线程，所以使用线程池会节省系统资源，并且每个线程池都维护了 一些基础的数据统计，方便线程的管理和监控。





### ScheduledExecutorService 接口

`ScheduledExecutorService` 是一个接口，它继承于 `ExecutorService`。它是一个可以完成“延时” 和“周期性”任务的调度线程池接口，其功能和 Timer/TimerTask 类似。





### ScheduledThreadPoolExecutor 实现类

ScheduledThreadPoolExecutor 继承于 ThreadPoolExecutor，它提供了ScheduledExecutorService线程池接口中“延时执行”和“周期执行”等抽象调度方法的具体实现。

ScheduledThreadPoolExecutor 类似于 Timer，但是在高并发程序中，ScheduledThreadPoolExecutor 的性能要优于 Timer。



### Executors 静态工厂类

`Executors` 是一个工具类，包含了静态工厂方法，<font color="red">**用于创建 `ExecutorService` 、`ScheduledExecutorService` 等线程池实例对象**</font>。它**简化了线程池的创建过程**，使得创建线程池变得更加方便和直观。

| 线程池类型     | 方法                                                 | 说明                                                      |
| -------------- | ---------------------------------------------------- | --------------------------------------------------------- |
| 固定大小线程池 | `Executors.newFixedThreadPool(int nThreads)`         | 创建一个固定大小的线程池，线程池中的线程数量为 `nThreads` |
| 缓存线程池     | `Executors.newCachedThreadPool()`                    | 创建一个缓存线程池，根据需要创建新线程，但会复用空闲线程  |
| 单线程池       | `Executors.newSingleThreadExecutor()`                | 创建一个单线程池，所有任务按顺序执行                      |
| 调度线程池     | `Executors.newScheduledThreadPool(int corePoolSize)` | 创建一个调度线程池，可以执行定时或周期性任务              |

