## 一、HashTable 类

`java.util`  提供了线程安全的 Map —— `HashTable` 。

**`HashTable` 使用 `synchronized` 来保证线程安全**，包含 `get()/put()` 在内的所有相关需要进行同步执行的方法都加上了 `synchronized`  关键字，以锁定这个哈希表。`HashTable` 线程安全策略的代价非常大，这相当于给整个哈希表加了一把大锁。



## 二、ConcurrentHashMap( 1.7 老版本)

ConcurrentHashMap 采用分段锁（segment） 机制。

`ConcurrentHashMap` 的内部结构的层次关系为 ConcurrentHashMap→Segment→HashEntry。这样 设计的好处在于，每次访问的时候只需要将一个 `Segment` 锁定，而不需要将整个 Map 类型集合都进行锁定。

<img src="images/image-20240828192953841.png" alt="image-20240828192953841" style="zoom:80%;" />



## 三、ConcurrentHashMap( 最新版本)

### 3.1 底层数据结构

在 JDK 17， 数据结构为 <font color="red">**数组 + 链表 / 红黑树**</font>。其中，

- 数组(哈希表)： 存放 HashMap 里面的键值对。数组里面的每个元素要么是 `null`，要么是哈希桶的头节点（一个 `Node<K, V>` 对象），指向链表 / 红黑树的起始。
- 链表 / 红黑树(哈希桶)：解决哈希冲突。当链表达到一定长度时，链表将会转化为红黑树。 在 链表 / 红黑树 中，所有元素的键不相同，但键的哈希值相同。

![jdk1.8之后的内部结构-HashMap](images/jdk1.8_hashmap.png)



`table` 是 `HashMap` 用来存储键值对的数组，具体细节为：

- 每个元素是一个链表的头节点（或者在树化的情况下是一个红黑树的根节点）。
- 数组的长度总是 2 的幂，可以通过掩码操作（位运算）高效地计算索引， 这有助于哈希运算的均匀分布。
- 数组在第一次使用时被初始化，并根据需要进行扩容。扩容时，数组的长度会增加到下一个 2 的幂次方。

```java
public class HashMap<K,V> extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable{
    
    /**
     * The table, initialized on first use, and resized as
     * necessary. When allocated, length is always a power of two.
     * (We also tolerate length zero in some operations to allow
     * bootstrapping mechanics that are currently not needed.)
     */
    transient Node<K,V>[] table;
    
    static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;     // 哈希值
        final K key;        // 键
        V value;            // 值
        Node<K,V> next;     // 指向下一个节点，链表或者红黑树中的节点

        // 构造函数、setter、getter 方法
        ...
    }
    
    static final class TreeNode<K,V> extends LinkedHashMap.Entry<K,V> {
        TreeNode<K,V> parent;  // red-black tree links
        TreeNode<K,V> left;
        TreeNode<K,V> right;
        TreeNode<K,V> prev;    // needed to unlink next upon deletion
        boolean red;
      
        // 构造函数、setter、getter 方法
    	...
}

```

链式桶是一个由NODE 节点组成的链表。树状桶是一棵由TreeNode节点组成的红黑树，树的根 节点为TreeBin类型。





### 3.2 线程安全的实现策略

线程安全的实现策略为 `Node + CAS + synchronized`，具体优化如下：

- **细粒度锁与 CAS 的结合使用**：使用更细粒度的锁（`synchronized`）和 CAS (Compare-And-Swap) 算法来保证线程安全，根据锁竞争操作来判断使用 `synchronized`  或者 CAS 。
- **锁粒度进一步优化到节点级别**：锁的粒度被细化到每个节点，最大限度地降低锁的范围，从而实现更高的并发度。JDK 17 最大并发度是 Node 数组的大小。

- **链表或树节点操作中的自旋机制**：在某些操作（如链表或树节点的插入、删除）中，CAS 失败后，采用自旋的方式重试。在高并发环境下，自旋可以避免不必要的上下文切换，从而提高性能。如果自旋锁仍然无法解决冲突，`ConcurrentHashMap` 会回退到更传统的锁机制，即使用内置的 `synchronized` 来进行更粗粒度的锁保护。



### 3.2 initTable() 方法

`initTable()` 方法用于初始化 `ConcurrentHashMap` 的 `table`（存储桶数组），并保证线程安全。

关键变量有：

- `Node<K,V>[] table`：`ConcurrentHashMap` 的存储桶数组。

- `sizeCtl`：控制并发扩容或初始化的标志变量。

  - **负数**：表示有线程正在进行初始化或扩容。

  - **0**：表示尚未初始化。

  - **正数**：如果未初始化，则作为初始容量；如果已初始化，则作为下一次扩容的触发阈值。

线程安全策略为：

- **CAS**：使用 CAS 尝试将 `sizeCtl` 设置为 -1，表示当前线程开始初始化
- **`volatile` 可见性**：`table` 是一个 `volatile` 变量，保证多线程对其操作的可见性。
- **细粒度控制**：通过 `sizeCtl` 的状态（正数/负数）协调线程，避免多个线程同时初始化或扩容。

```java
private final Node<K,V>[] initTable() {
    Node<K,V>[] tab; // 存储桶数组
    int sc;          // sizeCtl 控制变量

    // 1. 循环检查 table 是否已经初始化
    while ((tab = table) == null || tab.length == 0) {
        // 如果 sizeCtl 小于 0，说明其他线程正在初始化
        if ((sc = sizeCtl) < 0)
            Thread.yield(); // 当前线程让出 CPU 时间片，避免竞争
        // 使用 CAS 尝试将 sizeCtl 设置为 -1，表示当前线程开始初始化
        else if (U.compareAndSetInt(this, SIZECTL, sc, -1)) {
            try {
                // 再次检查 table 是否仍未初始化
                if ((tab = table) == null || tab.length == 0) {
                    // 2. 计算初始容量，如果 sizeCtl > 0，则用 sizeCtl 作为容量，否则使用默认容量
                    int n = (sc > 0) ? sc : DEFAULT_CAPACITY;

                    // 创建新的存储桶数组
                    @SuppressWarnings("unchecked")
                    Node<K,V>[] nt = (Node<K,V>[])new Node<?,?>[n];
                    table = tab = nt; // 初始化存储桶数组

                    // 计算下一次扩容的阈值（容量的 75%）
                    sc = n - (n >>> 2);
                }
            } finally {
                // 3. 初始化完成后恢复 sizeCtl，避免阻塞其他线程
                sizeCtl = sc;
            }
            break; // 初始化完成，退出循环
        }
    }
    return tab; // 返回初始化后的存储桶数组
}
```



### 3.3 put() 方法

putVal 方法的线程安全策略为：

- 当桶为空时，使用 CAS 更新。
- 当桶正在扩容的情况，当前线程会协助完成扩容任务。协助完成后，当前线程会继续自己的插入操作。
- 当目标桶不为空时（存在链表或红黑树结构），操作逻辑复杂，无法通过 CAS 一步完成，需要 `synchronized` 来保护一致性。

```java
  /**
     * 将指定的值与指定的键关联到此映射中。
     * 如果映射之前已包含该键的映射关系，则旧值将被替换。
     *
     * @param key 要关联的键
     * @param value 要关联的值
     * @return 之前与指定键关联的值，如果没有映射关系则返回 {@code null}
     * @throws NullPointerException 如果指定的键或值为 null
     */
    public V put(K key, V value) {
        return putVal(key, value, false);
    }

    /**
     * put 和 putIfAbsent 方法的实现。
     * 该方法用于处理将键值对实际放置到哈希表中的逻辑。
     *
     * @param key 要关联的键
     * @param value 要关联的值
     * @param onlyIfAbsent 如果为 true，则只有当键未与任何值关联时才执行 put 操作
     * @return 之前与指定键关联的值，如果没有映射关系或操作为插入则返回 {@code null}
     * @throws NullPointerException 如果指定的键或值为 null
     */
    final V putVal(K key, V value, boolean onlyIfAbsent) {
    // 1. 空值检查
    if (key == null || value == null) throw new NullPointerException();

    // 2. 计算键的哈希值，减少冲突
    int hash = spread(key.hashCode());
    int binCount = 0;

    // 3. 初始化或遍历桶数组
    for (Node<K,V>[] tab = table;;) {
        Node<K,V> f; int n, i, fh; K fk; V fv;

        // 3.1 如果桶数组未初始化，则调用 initTable 方法初始化
        if (tab == null || (n = tab.length) == 0)
            tab = initTable();

        // 3.2 计算索引 i，目标桶为空时直接用 CAS 插入新节点
        else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {
            if (casTabAt(tab, i, null, new Node<K,V>(hash, key, value)))
                break; // 插入成功，无需加锁
        }

        // 3.3 如果当前桶正在扩容，则协助扩容
        else if ((fh = f.hash) == MOVED)
            tab = helpTransfer(tab, f);

        // 3.4 检查目标桶的第一个节点是否是目标键（不覆盖值时直接返回旧值）
        else if (onlyIfAbsent
                 && fh == hash
                 && ((fk = f.key) == key || (fk != null && key.equals(fk)))
                 && (fv = f.val) != null)
            return fv;

        // 3.5 发生哈希冲突，需要加锁处理
        else {
            V oldVal = null;
            synchronized (f) { // 加锁
                if (tabAt(tab, i) == f) {
                    // 3.5.1 如果是普通链表，遍历找到目标键或插入新节点
                    if (fh >= 0) {
                        binCount = 1;
                        for (Node<K,V> e = f;; ++binCount) {
                            K ek;
                            if (e.hash == hash &&
                                ((ek = e.key) == key || (ek != null && key.equals(ek)))) {
                                oldVal = e.val;
                                if (!onlyIfAbsent)
                                    e.val = value; // 更新值
                                break;
                            }
                            Node<K,V> pred = e;
                            if ((e = e.next) == null) {
                                pred.next = new Node<K,V>(hash, key, value); // 插入新节点
                                break;
                            }
                        }
                    }
                    // 3.5.2 如果是红黑树，调用 TreeBin.putTreeVal 进行树操作
                    else if (f instanceof TreeBin) {
                        Node<K,V> p;
                        binCount = 2;
                        if ((p = ((TreeBin<K,V>)f).putTreeVal(hash, key, value)) != null) {
                            oldVal = p.val;
                            if (!onlyIfAbsent)
                                p.val = value; // 更新值
                        }
                    }
                    // 3.5.3 如果是保留节点，抛出异常
                    else if (f instanceof ReservationNode)
                        throw new IllegalStateException("Recursive update");
                }
            }

            // 3.6 如果冲突导致链表长度超过阈值，转换为红黑树
            if (binCount != 0) {
                if (binCount >= TREEIFY_THRESHOLD)
                    treeifyBin(tab, i); // 转换为红黑树
                if (oldVal != null)
                    return oldVal; // 返回旧值
                break;
            }
        }
    }

    // 4. 更新计数，触发扩容检查
    addCount(1L, binCount);
    return null; // 新增键时返回 null
}
```



### 3.4 get() 方法

线程安全策略：

- 只读操作无需加锁
- 基于 `volatile` 保证可见性：`ConcurrentHashMap` 内部使用了 `volatile` 修饰 `table` 和桶中的节点（如 `Node` 数组）。

```java
public V get(Object key) {
    Node<K,V>[] tab; Node<K,V> e, p; int n, eh; K ek;
    // 1. 计算 key 的哈希值并通过 spread 方法进一步优化分布
    int h = spread(key.hashCode());

    // 2. 检查 table 是否初始化且长度大于 0，同时定位到 key 对应的桶
    if ((tab = table) != null && (n = tab.length) > 0 &&
        (e = tabAt(tab, (n - 1) & h)) != null) {

        // 3. 检查桶中第一个节点的哈希值是否等于目标 key 的哈希值
        if ((eh = e.hash) == h) {
            // 3.1 如果 key 相等（引用相等或 equals 比较相等），直接返回对应的值
            if ((ek = e.key) == key || (ek != null && key.equals(ek)))
                return e.val;
        }
        // 4. 如果第一个节点的哈希值小于 0，说明这是一个特殊节点（如 TreeBin 或 ForwardingNode）
        //    调用 find 方法进一步查找 key
        else if (eh < 0)
            return (p = e.find(h, key)) != null ? p.val : null;

        // 5. 如果不是特殊节点且 key 不匹配，沿着链表或树结构继续查找目标 key
        while ((e = e.next) != null) {
            if (e.hash == h &&
                ((ek = e.key) == key || (ek != null && key.equals(ek))))
                return e.val;
        }
    }

    // 6. 如果未找到匹配的 key，返回 null
    return null;
}
```



### 3.6 key 和 value 不能为 null

null 具备二义性：

- 不存在
- 存在，但本身为 null

在多线程的环境下，`containsKey(key)` **无法完全准确地判断一个 `key` 是否存在**。因为 `containsKey` 本身并不会加锁，也没有保证在它执行时，其他线程不会修改 `Map`。举个例子，

- 线程 A 执行 `containsKey(key)`，判断 `key` 是否存在。

- 在查询过程中，线程 B 执行了 `remove(key)`，将该 `key` 从 `Map` 中移除。

- 如果线程 A 得到 `containsKey(key)` 为 `true`，而实际上 `key` 已经被线程 B 删除了，结果就不准确了。



## 参考资料

[ConcurrentHashMap 源码分析 | JavaGuide](https://javaguide.cn/java/collection/concurrent-hash-map-source-code.html)