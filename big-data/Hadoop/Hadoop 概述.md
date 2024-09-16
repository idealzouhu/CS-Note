## 一、Hadhoop 概述

### 1.1 什么是 Hadhoop

Apache Hadoop软件库是一个框架，该框架允许使用简单的编程模型跨计算机集群**对大型数据集进行分布式处理**。它旨在从单个服务器扩展到数千台机器，每台机器都提供本地计算和存储。库本身不用于依靠硬件来提供高可用性，而是被设计用来检测和处理应用程序层的故障，因此可以在计算机集群的顶部提供高可用性服务，每台计算机都容易出现故障。







## 二、Hadhoop 架构

### 2.2 Hadhoop 组成模块

Hadhoop 主要包括以下模块：

- Hadoop Common：支持其他Hadoop模块的通用实用程序。

- **Hadoop Distributed File System**（HDFS）：提供对应用程序数据的高吞吐量访问的分布式文件系统。

- Hadoop YARN：用于作业调度和集群资源管理的框架。

- Hadoop MapReduce：一个基于YARN的系统，用于并行处理大型数据集。
- **[Hadoop Ozone](https://hadoop.apache.org/ozone/)**：**[ Hadoop](https://hadoop.apache.org/ozone/)**的对象存储。

在Hadoop 框架中， 最核心的设计就是：MapReduce 和 HDFS。

1. MapReduce 的思想是由 Google 的一篇论文所提及而被广为流传的，简单的一句话解释 MapReduce 就是“任务的分解与结果的汇总”。
2. HDFS 是 Hadoop 分布式文件系统（Hadoop Distributed File System）的缩写，为分布式计算存储提供了底层支持。



### 2.3 Hadhoop 核心架构

Hadoop2.0时期架构图

最底层是分布式文件系统HDFS。

在分布式文件系统之上是资源管理与调度系统YARN。YARN相当于一个操作系统，基于YARN可以部署离线批处理计算框架MapReduce、交互式分布式计算框架Tez，内存计算框架Spark，流式计算框架Storm等。

上层可以运行Hive、Pig等数据库。Hive是一种类似于sql的分布式数据库，可以构建数据仓库。Pig是一种流数据库语言。

如果需要执行多个连续的分布式计算任务，那么Oozie是理想的选择，其可以方便的执行多种任务。

除此之外，Hbase可以作为分布式数据库来使用，例如构建海量图像数据的存储仓库，其查询速度很快，是面向列存储的数据库。

ZooKeeper是分布式协调服务。

Flume可以构建日志分析系统。Sqoop可以轻松将数据库中的数据导入HDFS.

![img](images/11120339-b5427a627e215313)







## 参考资料

[Apache Hadoop](https://hadoop.apache.org/)

[Hadoop 中文网](https://hadoop.org.cn/)

[Hadoop生态系统架构 - 简书 (jianshu.com)](https://www.jianshu.com/p/e883684d24e4)

[大数据学习之路05——Hadoop原理与架构解析-腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/1431491)