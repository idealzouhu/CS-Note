### 什么是 binlog

binlog 是 Server 层生成的二进制日志，记录对数据库中表结构和表数据的所有更改操作。

binlog 的主要作用为：

- **数据恢复**：当MySQL数据库发⽣故障或者崩溃时，可以通过BinLog进⾏数据恢复。
- **主从复制**: 在主数据库上开启BinLog，主数据库把BinLog发送⾄从数据库，从数据库获取 BinLog后通过I/O线程将⽇志写到中继⽇志，也就是Relay Log中。然后，通过SQL线程将 Relay Log中的数据同步⾄从数据库，从⽽达到主从数据库数据的⼀致性。





### 能不能只用 binlog 不用 redo log

不行，binog是 server 层的日志，**没办法记录哪些脏页还没有刷盘**，redolog 是存储引擎层的日志，可以记录哪些脏页还没有刷盘，这样崩溃恢复的时候，就能恢复那些还没有被刷盘的脏页数据。





### binlog 写入机制

binlog 文件保存的是全量的日志，采用<font color="red">**顺序写**</font>的方式。





### binlog 两阶段提交过程

事务提交后，redo log 和 binlog 都要持久化到磁盘，但是这两个是独立的逻辑。

为了保证 redo log 和 binlog  的一致性，MySQL 使用 XA 事务来保证一致性。其中， **binlog 作为协调者，存储引擎作为参与者**。

![时刻 A 与时刻 B](images/两阶段提交崩溃点.drawio.png)

[MySQL面试题 | 小林coding (xiaolincoding.com)](https://xiaolincoding.com/interview/mysql.html#binlog-两阶段提交过程是怎么样的)

**两阶段提交是以 binlog 写成功为事务提交成功的标识**















### 异步主从复制



![MySQL 主从复制过程](images/主从复制过程.drawio.png)

