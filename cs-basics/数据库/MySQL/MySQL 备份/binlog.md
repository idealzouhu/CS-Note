### 什么是 binlog

binlog 是 Server 层生成的二进制日志，记录对数据库中表结构和表数据的所有更改操作。

binlog 的主要作用为：

- **数据恢复**：当MySQL数据库发⽣故障或者崩溃时，可以通过BinLog进⾏数据恢复。
- **主从复制**: 在主数据库上开启BinLog，主数据库把BinLog发送⾄从数据库，从数据库获取 BinLog后通过I/O线程将⽇志写到中继⽇志，也就是Relay Log中。然后，通过SQL线程将 Relay Log中的数据同步⾄从数据库，从⽽达到主从数据库数据的⼀致性。





### binlog 写入机制

binlog 文件保存的是全量的日志，采用<font color="red">**顺序写**</font>的方式。

