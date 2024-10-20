### OOM 问题

`java.lang.OutOfMemoryError` 报错情况如下表所示。

| **OOM 问题**                           | **描述**                                                     | **可能原因**                                               |
| -------------------------------------- | ------------------------------------------------------------ | ---------------------------------------------------------- |
| **Java Heap Space**                    | 当 Java 堆内存不足以存储对象时抛出该异常。                   | 大量对象创建、内存泄漏、对象生命周期过长、过大的数组。     |
| **Metaspace**                          | 当 Metaspace 区域（存储类元数据）不足时抛出该异常。          | 类加载过多、动态生成类、ClassLoader 泄漏。                 |
| **GC Overhead Limit Exceeded**         | 当 GC 花费过多时间（超过 98% 的时间）而回收的内存少于 2% 时抛出该异常。 | 堆内存配置不足、内存泄漏或应用程序的内存需求激增。         |
| **Direct Buffer Memory**               | 使用 NIO 的直接缓冲区（Direct Buffer）时，内存不足。         | 直接缓冲区使用不当、未释放直接缓冲区。                     |
| **Unable to Create New Native Thread** | 系统无法创建新的线程（通常是因为线程限制被耗尽）。           | 应用程序创建过多线程、操作系统线程限制或 Java 虚拟机限制。 |
| **Requesting Memory**                  | 请求的本地内存不足，通常发生在使用 JNI 的情况下。            | JNI 代码内存管理不当、错误的内存释放。                     |



### 排查 OOM 问题



### 分析 dump 文件

Dump 文件是 Java 进程的内存镜像，其中主要包括 **系统信息**、**虚拟机属性**、**完整的线程 Dump**、**所有类和对象的状态** 等信息。

#### 设置 JVM 启动参数

```
-XX:+HeapDumpOnOutOfMemoryError
-XX:HeapDumpPath=./（参数为 Dump 文件生成路径）
```





### 参考资料

[应用出现OOM异常，程序员如何第一时间知道？ (yuque.com)](https://www.yuque.com/magestack/12306/cpk7ga8acmpe5nm8#HhFXE)

[线上JVM OOM问题，如何排查和解决？ (qq.com)](https://mp.weixin.qq.com/s/Bs8r1ecNzeUgfyxyH2cQ7w)