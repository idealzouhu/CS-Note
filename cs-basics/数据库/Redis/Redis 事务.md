



- 非回滚事务：当 Redis 事务执行过程中发生运行时错误时，Redis 不会自动回滚事务。因为 Redis 事务采用的是乐观锁的方式来实现原子性。

