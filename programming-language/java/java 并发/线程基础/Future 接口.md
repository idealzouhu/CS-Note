### 什么是 Future 接口

Future 接口通常**用于实现异步编程和回调机制**， 至少提供了三大功能：

- 能够取消异步执行中的任务。
- 判断异步任务是否执行完成。 
- 获取异步任务完成后的执行结果

源代码如下：

```java
package java.util.concurrent;
public interface Future<V> {
    // 判断计算是否已经完成。
    boolean isDone();

    // 取消异步执行
    boolean cancel(boolean mayInterruptIfRunning);
    
    // 判断异步执行是否被取消。
    boolean isCancelled();
 
    // 获取异步任务完成后的执行结果
    V get() throws InterruptedException, ExecutionException;

    //  获取异步任务完成后的执行结果， 等待时间不超过指定的超时时间。
    V get(long timeout, TimeUnit unit) throws InterruptedException, ExecutionException, TimeoutException;
}

```

注意：`Future` 接口提供了 `get()` 方法来获取任务的结果。**这个方法的行为是同步的**，具体表现为：

- **阻塞行为**：当你调用 `future.get()` 时，如果任务尚未完成，当前线程将被阻塞，直到任务完成并返回结果。
- **获取结果**：在任务完成后，`get()` 方法将返回任务的结果，或者抛出异常（如果任务执行过程中发生了异常）。

虽然任务的执行是异步的，但通过 `Future` 的 `get()` 方法获取结果的过程是同步的，因为调用 `get()` 方法的线程必须等待任务完成。





### 回调和回调地狱

**回调**（Callback）是编程中的一种技术，指的是将一个函数作为参数传递给另一个函数，并在某个时间点调用该函数。回调函数通常用于处理异步操作，**当异步操作完成后调用回调函数以处理结果**。回调可以帮助实现非阻塞的代码，使得程序在执行异步操作时不会被阻塞，能够继续执行其他任务。

> **回调**在中文中通常表示“在特定事件或条件满足时调用的函数或方法。这个概念在异步编程中尤为常见，可以理解为一种“回过头来调用”的方式。





**回调地狱**（Callback Hell）是指在处理复杂的异步逻辑时，嵌套的回调函数层层嵌套，导致代码变得难以阅读和维护的现象。回调函数的过度嵌套不仅增加了代码的复杂性，还使得错误处理变得更加困难。