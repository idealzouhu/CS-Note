### Java 程序的进程

什么是 Java 程序的进程呢?

Java编写的程序都运行在Java虚拟机(JVM)中，每当使用 Java 命令启动一个 Java 应用程序时,就会启动一个 JVM 进程。在这个JVM进程内部，所有 Java 程序代码都是以线程来运行的。IVM找到程序的入口点main()方法，然后运行main()方法，这样就产生了一个线程，这个线程称为主线程。当main()方法结束后，主线程运行完成，JVM进程也随即退出。





### Java 程序的线程

在Java中，执行程序流程的重要单位是“方法”，而栈内存的分配的单位是“栈帧”(或者叫“方法帧”)。

方法的每一次执行都需要为其分配一个栈帧(方法帧)，栈帧主要保存该方法中的局部变量、方法的返回地址以及其他方法的相关信息。当线程的执行流程进入方法时，JVM就会为方法分配一个对应的栈帧压入栈内存:当线程的执行流程跳出方法时，JVM就从栈内存弹出该方法的栈帧，此时方法栈帧的内存空间就会被回收，栈帧中的变量就会被销毁。





### JVM 线程状态和 OS 线程状态

JVM的幕后工作和操作系统的线程调度有关。Java中的线程管理是通过JNI本地调用的方式， 委托操作系统的线程管理API完成的。当Java线程的Thread实例的start()方法被调用后，操作系统中 的对应线程进入的并不是运行状态，而是就绪状态，而Java线程并没有这个就绪状态。

JVM的线程状态与其幕后的操作系统线程状态之间的转换关系为：

![image-20240705203355053](images/image-20240705203355053.png)

**为什么 JVM 没有区分就绪状态和运行状态？**一个线程一次最多只能在 CPU 上运行比如 10-20ms 的时间（此时处于运行 状态），也即大概只有 0.01 秒这一量级，时间片用后就要被切换下来放入调度队列的末尾等待再次调度。（也即回到就绪状态）。线程切换的如此之快，区分这两种状态就没什么意义了。



### Java 线程状态转换

Java 线程状态变迁图(图源：[挑错 |《Java 并发编程的艺术》中关于线程状态的三处错误open in new window](https://mp.weixin.qq.com/s/UOrXql_LhOD8dhTq_EPI0w))：

![Java 线程状态变迁图](images/640.png)





### 参考资料

[挑错 |《Java 并发编程的艺术》中关于线程状态的三处错误 (qq.com)](https://mp.weixin.qq.com/s/UOrXql_LhOD8dhTq_EPI0w)