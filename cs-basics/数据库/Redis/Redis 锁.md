<font color="red">Redis 通常是通过键（key）来管理锁。</font> 

```java
RLock lock = redisson.getLock("myLock");

try {   
	lock.lock();
	// TODO: 执行业务逻辑
} finally {
	lock.unlock();
}

```

