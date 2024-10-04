



## 根据锁的大小进行划分

### 表级锁





### 行级锁

行级锁的类型主要有三类：

- Record Lock，记录锁，也就是仅仅把一条记录锁上；
- Gap Lock，间隙锁，锁定一个范围，但是不包含记录本身；
- Next-Key Lock 称为临键锁，是 Record Lock + Gap Lock 的组合，锁定一个范围，并且锁定记录本身；
- **插入意向锁**



间隙锁之间不存在冲突。

插入意向锁和间隙锁之间存在冲突。



## 根据锁的功能进行划分

### 共享锁和排他锁

共享锁（Share Lock，S 锁）允许多个事务同时读取同一条记录，但不允许修改。这种锁的名字来源于它允许多个事务**共享**锁定资源进行读取操作。

排他锁（Exclusive Lock，X 锁）不允许其他事务访问被锁定的资源（既不能读取，也不能修改）。它是为了防止其他事务对数据进行读取或写入，从而保证数据的独占性。







不论是表级锁还是行级锁，都存在共享锁和排他锁







### 参考资料

[MySQL :: MySQL 8.4 Reference Manual :: 17.7.1 InnoDB Locking](https://dev.mysql.com/doc/refman/8.4/en/innodb-locking.html)

[MySQL常见面试题总结 | JavaGuide](https://javaguide.cn/database/mysql/mysql-questions-01.html#mysql-锁)

[MySQL 有哪些锁？ | 小林coding (xiaolincoding.com)](https://xiaolincoding.com/mysql/lock/mysql_lock.html#插入意向锁)