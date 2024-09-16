

## 一、MapReduce 概述

### 1.1 什么是 MapReduce

Hadoop MapReduce是一个软件框架，可以轻松编写应用程序，以可靠、容错的方式在商用硬件的大型集群（数千个节点）上并行处理大量数据（数TB数据集）。





## 二、MapReduce 架构

### 2.1 MapReduce 组成

MapReduce框架由单个主ResourceManager、每个集群节点一个工作NodeManager和每个应用程序的MRAppMaster组成（请参阅YARN架构指南）。







通常，计算节点和存储节点是相同的，也就是说，MapReduce框架和分布式文件系统（参见HDFS架构指南）运行在同一组节点上。这种配置允许框架在已经存在数据的节点上有效地调度任务，从而在整个集群中产生非常高的聚合带宽。