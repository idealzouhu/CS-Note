## MySQL 事务

 [MySQL 事务](MySQL 事务.md) ：事务的ACID，事务并发引起的问题，以及相应的事务隔离级别

 [事务隔离级别](事务隔离级别.md) ：





### 事务日志

 [redo log.md](redo log.md) ：redo log 是 InnoDB 存储引擎层生成的物理日志，实现事务的持久性，主要用于掉电等故障恢复。

 [undo log.md](undo log.md) ：undo log 是 InnoDB 存储引擎层生成的逻辑日志，实现事务的原子性，主要用于事务回滚和MVCC。





### 并发控制

[多版本并发控制 MVCC ](多版本并发控制 MVCC .md) : MVCC 是一种并发控制机制，用于在多个并发事务同时读写数据库时保持数据的一致性和隔离性。







