## 一、JVM 参数

### 基本参数





## 二、工具

### 2.1 JDK 工具



#### 2.1.1 调试工具

| 工具名称 | 用途                                                         |
| -------- | ------------------------------------------------------------ |
| `jdb`    | Java 调试器，用于调试 Java 应用程序。支持设置断点、检查变量等功能。 |
| `jinfo`  | 显示运行中的 Java 进程的配置信息，例如 JVM 参数。            |
| `jstack` | 打印 Java 线程堆栈，用于分析线程状态、定位死锁问题。         |
| `jmap`   | 生成堆转储（Heap Dump）文件，或查看堆内存的使用情况。        |
| `jhat`   | 分析堆转储文件，通常配合 `jmap` 使用。                       |

`jstack` 工具是 jdk 自带的线程堆栈分析工具， 可以用来排查 Java 程序是否死锁。[5.4 怎么避免死锁？ | 小林coding (xiaolincoding.com)](https://xiaolincoding.com/os/4_process/deadlock.html#关注作者)





### 2.2 Linux 命令行工具





## 参考资料

[【JVM进阶之路】十：JVM调优总结 - 三分恶 - 博客园 (cnblogs.com)](https://www.cnblogs.com/three-fighter/p/14644152.html)





