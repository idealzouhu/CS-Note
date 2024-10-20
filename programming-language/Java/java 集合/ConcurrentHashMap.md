## 一、HashTable 类

`java.util`  提供了线程安全的 Map —— `HashTable` 。

**`HashTable` 使用 `synchronized` 来保证线程安全**，包含 `get()/put()` 在内的所有相关需要进行同步 执行的方法都加上了 `synchronized`  关键字，以锁定这个哈希表。`HashTable` 线程安全策略的代价非常大，这相当于给整个哈希表加了一把大锁。



## 二、ConcurrentHashMap( 1.7 老版本)

ConcurrentHashMap 采用分段锁（segment） 机制。

`ConcurrentHashMap` 的内部结构的层次关系为 ConcurrentHashMap→Segment→HashEntry。这样 设计的好处在于，每次访问的时候只需要将一个 `Segment` 锁定，而不需要将整个 Map 类型集合都进 行锁定。

<img src="images/image-20240828192953841.png" alt="image-20240828192953841" style="zoom:80%;" />



## 三、ConcurrentHashMap( 最新版本)

### 底层数据结构

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





### 线程安全的实现

使用更细粒度的锁（Synchronized）和 **CAS (Compare-And-Swap)** 算法来保证线程安全，将锁的粒度进一步降低到每个节点或操作层面， 并减少锁的使用。

**根据锁竞争操作来判断使用 Synchronized 或者 CAS** 

在某些操作（比如链表或树节点的插入、删除）中，如果 CAS 失败，则会尝试采用自旋锁机制。在高并发环境下，自旋可以避免不必要的上下文切换，从而提高性能。

如果自旋锁仍然无法解决冲突，`ConcurrentHashMap` 会回退到更传统的锁机制，即使用内置的 `synchronized` 来进行更粗粒度的锁保护。



