### CopyOnWriteArrayList 类简介

```java
public class CopyOnWriteArrayList<E>
    implements List<E>, RandomAccess, Cloneable, java.io.Serializable {
    
}
```



### 写时复制( Copy-On-Write)思想

**基本原理**: 写时复制的基本原理是延迟实际的数据复制，直到需要修改数据时才进行复制。初始时，多个对象可以共享相同的数据副本。当其中一个对象需要修改数据时，它会创建数据的一个副本，并对副本进行修改。其他对象仍然使用未修改的数据副本。写完之后，更新数据。

写时复制的优点：

- 适合读多写少的并发场景，避免对读操作加锁导致的资源浪费。

缺点：

- 写操作开销
- 数据一致性问题



### 如何采用 Copy-On-Write 策略

Copy-On-Write 策略是 `CopyOnWriteArrayList` 线程安全的核心，使读取操作完全无需加锁，即**读读不互斥、<font color="red">读写不互斥</font>、写写互斥**。当需要修改（ `add`，`set`、`remove` 等操作） `CopyOnWriteArrayList` 的内容时，不会直接修改原数组，而是会先创建底层数组的副本，对副本数组进行修改，修改完之后再将修改后的数组赋值回去，这样就可以保证写操作不会影响读操作了。

在 `CopyOnWriteArrayList`  的写入操作 `add()` 里面，加了独占锁以**确保只能有一个线程进行写入操作**，避免多线程写的时候会复制出多个副本。同时，在执行替换数组地址操作之前，读取的是老数组的数据，数据是有效数据。

```java
public class CopyOnWriteArrayList<E>
    implements List<E>, RandomAccess, Cloneable, java.io.Serializable {
    
    /** 内部对象数组，通过 getArray/setArray 方法访问 */
    private transient volatile Object[] array;
     
    final transient Object lock = new Object();

    public boolean add(E e) {
        synchronized (lock) {
            Object[] es = getArray();
            int len = es.length;
            es = Arrays.copyOf(es, len + 1);
            es[len] = e;
            setArray(es);	// 替换数组
            return true;
        }
    }
}  
```

但是，每次添加元素的时候都会重新复制一份新的数组，开销比较大。

