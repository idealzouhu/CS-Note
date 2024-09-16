### 区别

- redo log 记录了此次事务**完成后**的数据状态，记录的是更新**之后**的值
- undo log 记录了此次事务**开始前**的数据状态，记录的是更新**之前**的值

![img](images/1717920935536-45ceca35-c79c-48eb-a240-96d580e399b5.png)

redo log 是物理日志，undo log 是逻辑日志





### 联系

因为MySQL事务执⾏过程中产⽣的Undo Log也需要进⾏持久化操作，所以 **Undo Log也会产⽣Redo Log**。由于Undo Log的完整性和可靠性需要Redo Log来保证，因此数据库崩溃时需要先做Redo Log数据恢复，然后做Undo Log回滚。