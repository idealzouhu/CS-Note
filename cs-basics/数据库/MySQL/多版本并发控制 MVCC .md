[InnoDB存储引擎对MVCC的实现 | JavaGuide](https://javaguide.cn/database/mysql/innodb-implementation-of-mvcc.html#多版本并发控制-multi-version-concurrency-control)



MVCC 是一种并发控制机制，用于在多个并发事务同时读写数据库时保持数据的一致性和隔离性。它是通过在每个数据行上维护多个版本的数据来实现的。当一个事务要对数据库中的数据进行修改时，MVCC 会为该事务创建一个数据快照，而不是直接修改实际的数据行。



MVCC 通过创建数据的多个版本和使用快照读取来实现并发控制。读操作使用旧版本数据的快照，写操作创建新版本，并确保原始版本仍然可用。