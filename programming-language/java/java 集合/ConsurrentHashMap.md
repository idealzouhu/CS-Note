









### 底层数据结构

在 JDK 17， 数据结构为 <font color="red">**数组 + 链表 / 红黑树**</font>。其中，

- 数组： 存放 HashMap 里面的键值对。
- 链表 / 红黑树：解决哈希冲突。当链表达到一定长度时，链表将会转化为红黑树。

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
   
}

```



