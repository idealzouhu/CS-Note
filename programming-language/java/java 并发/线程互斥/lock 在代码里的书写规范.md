## 正确案例

lock.lock() 为什么写到try语句外，lock.unlock() 放到 finally 语句块 

```java
ReentrantLock lock = new ReentrantLock();  // 获取锁对象
lock.lock();   // 加锁
try {
    // access the resource protected by this lock
} finally {
    lock.unlock();  // 解锁
}
```



## lock.lock() 为什么写到try语句外

当lock.lock() 写到try语句内时，可能会出现<font color="red">异常堆栈丢失。</font>

```
ReentrantLock lock = new ReentrantLock();
try {
    lock.lock();
    // access the resource protected by this lock
} finally {
    lock.unlock();
}
```

在上述代码中，try中的**加锁操作出现异常，finally依然会执行解锁操作**，而此时并没有获取到锁，执行解锁操作会抛出另外一个异常。虽然都是抛出异常结束，但是此时**finally解锁抛出的异常信息会将加锁的异常信息覆盖**，导致信息丢失。因此应该将加锁操作放在try块之前执行。

为什么异常会被覆盖？这种异常覆盖的行为是为了确保程序能够处理最终的异常，并且能够得到准确的异常信息。

> 如果你非要写到 try 里面，那么 **请写到 try 语句块的第一行**，或者 lock 加锁方法前面不会存在可能出现异常的代码。





## lock.unlock() 放到 finally 语句块 

为了保证当前线程执行过程中出现异常时，锁依然能被释放掉，**避免死锁的产生。**

​    





# 参考资料

[lock.lock为什么写到try语句外？ (yuque.com)](https://www.yuque.com/magestack/12306/pu52u29i6eb1c5wh)

[Lock.lock()为什么在try之前执行？_加锁为什么放在 try 语句外-CSDN博客](https://blog.csdn.net/E_N_T_J/article/details/105943325)