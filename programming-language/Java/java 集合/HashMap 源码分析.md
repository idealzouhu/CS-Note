### HashMap 简介

`HashMap` 是 Java 集合框架中的一个实现了 `Map` 接口的类。它基于哈希表实现，提供了一个键值对的映射关系，并且允许快速的查找、插入和删除操作。

```java
public class HashMap<K,V> extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable{
    ...
}
```

HaahMap 的特点有：

- <font color="red">**每个键在 `HashMap` 中是唯一的**</font>。如果插入了具有相同哈希值的键，后插入的键值对会覆盖先插入的键值对。
- `HashMap` 使用哈希表作为底层数据结构，键通过哈希函数计算出哈希值，并存储在相应的桶中。



### 底层数据结构

在 JDK 17， 数据结构为 <font color="red">**数组 + 链表 / 红黑树**</font>。其中，

- 数组(哈希表)： 存放 HashMap 里面的键值对。数组里面的每个元素要么是 `null`，要么是哈希桶的头节点（一个 `Node<K, V>` 对象），指向链表 / 红黑树的起始。
- 链表 / 红黑树(哈希桶)：解决哈希冲突。当链表达到一定长度时，链表将会转化为红黑树。 在 链表 / 红黑树 中，所有元素的键不相同，但键的哈希值相同。

![jdk1.8之后的内部结构-HashMap](images/jdk1.8_hashmap.png)



`table` 是 `HashMap` 用来存储键值对的数组，具体细节为：

- 每个元素是一个链表的头节点（或者在树化的情况下是一个红黑树的根节点）。
- 数组的长度总是 2 的幂，可以通过掩码操作（位运算）高效地计算索引， 确保哈希值的低位参与索引计算，这有助于哈希运算的均匀分布。
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



### 扩容机制

#### 较为重要的参数

`HashMap` 中需要了解的参数有 `threshold` 和 `loadFactor`。

```java
public class HashMap<K,V> extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable {
	
     /**
     * The default initial capacity - MUST be a power of two.
     */
    static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16
    
    /**
     * HashMap 需要扩容的阈值。
     * 当 HashMap 中的元素数量达到该阈值时，会触发 HashMap 的扩容操作。
     * 阈值计算公式为：threshold = capacity * load factor
     */
    int threshold;

    /**
     * HashMap 的加载因子，在无参构造函数里面初始化为 0.75 。
     * 加载因子用于衡量 HashMap 的填充程度，影响扩容的频率。
     * 加载因子越大，HashMap的利用率越高，但可能增加碰撞的概率，从而影响性能。
     */
    final float loadFactor;
}

```

当没有指定初始容量时，哈希表的默认初始容量为 16 ， 扩容阈值为 12。



#### 扩容函数

扩容的核心函数为 `resize()` 。当链表长度超过 扩容阈值时， 执行`resize()` 函数。在 `resize()` 里面， 具体步骤为：

1. 扩展哈希表长度， `newCap = oldCap << 1`  表示扩容后的容量为原来的两倍。
2. 将旧哈希表中的数据放到新的哈希表中。

```
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
	// ....
	if (++size > threshold)
            resize();
}

final Node<K,V>[] resize() {
	// ....
	 if (oldCap > 0) {
            if (oldCap >= MAXIMUM_CAPACITY) {
                threshold = Integer.MAX_VALUE;
                return oldTab;
            }
            else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                     oldCap >= DEFAULT_INITIAL_CAPACITY)
                newThr = oldThr << 1; // double threshold
    }
    // ...
}
```

同时，在扩容 HashMap 的时候，不需要重新计算 hash，需要看看原来的hash值新增的那个bit是1还是0就好了。具体解析查看  [Java集合面试题 | 小林coding (xiaolincoding.com)](https://xiaolincoding.com/interview/collections.html#hashmap的扩容机制介绍一下)





### 什么时候链表转换为红黑树

在 `HashMap` 中，链表会在特定条件下转换为红黑树，以提高性能。具体流程为：

1. **检查链表长度是否超过阈值**：当一个哈希桶中的链表长度超过 `TREEIFY_THRESHOLD`，调用`treeifyBin()` 函数，链表会被转换为红黑树。相关源码为：

   ```java
   // 当某个链表的长度超过此阈值时，该链表可能会转换为红黑树以提高查找效率
   static final int TREEIFY_THRESHOLD = 8;
     
   final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                      boolean evict) {
   	...
   	if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
       	treeifyBin(tab, hash);
       ...                   
   }
   ```
   
2. **链表转化为红黑树**：首先，创建一个红黑树，开始时将链表中的节点插入到红黑树中。每个链表节点会被转换为红黑树节点（`TreeNode`）。然后，检查哈希表是否为空或其长度是否小于红黑树所需的最小容量。相关源码为：

   ```java
   // 当给定索引的桶中的链表转换为红黑树时，哈希表要求的最小容量
   static final int MIN_TREEIFY_CAPACITY = 64;
   
   /**
    * 将给定索引的桶中的链表转换为红黑树，前提是哈希表的容量足够。
    * 如果哈希表的容量不足，则会先进行扩容。
    *
    * @param tab 哈希表（桶数组）。
    * @param hash 键的哈希值。
    */
   final void treeifyBin(Node<K,V>[] tab, int hash) {
       int n, index;
       Node<K,V> e;
   
       // 检查哈希表是否为空或其长度是否小于红黑树所需的最小容量
       if (tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY) {
           // 如果哈希表的容量不足，则进行扩容
           resize();
       } else if ((e = tab[index = (n - 1) & hash]) != null) {
           // 如果指定索引的桶不为空，则开始将链表转换为红黑树
           TreeNode<K,V> hd = null, tl = null; // 红黑树的头节点和尾节点
   
           // 遍历桶中的链表
           do {
               // 将链表中的节点转换为 TreeNode
               TreeNode<K,V> p = replacementTreeNode(e, null);
   
               // 将节点链接成一个双向链表，以构建红黑树
               if (tl == null) {
                   // 如果是链表中的第一个节点，则将其设置为红黑树的头节点
                   hd = p;
               } else {
                   // 否则，将当前节点链接到前一个节点
                   p.prev = tl;
                   tl.next = p;
               }
               // 更新尾节点为当前节点
               tl = p;
           } while ((e = e.next) != null); // 继续遍历直到链表末尾
   
           // 将转换后的红黑树放回哈希表的对应桶中
           if ((tab[index] = hd) != null) {
               // 调用 treeify 方法将链表转换为红黑树，调整树的结构以保持平衡
               hd.treeify(tab);
           }
       }
   }
   
   ```

   



### 链表转换为红黑树的优劣势

**优势**：

- 当某个哈桶中的元素数量超过一定阈值（`TREEIFY_THRESHOLD`），链表会被转换成一个平衡树（红黑树），以提高性能。树化使得在桶中元素数量过多时，**查找时间从 O(n) 降低到 O(log n)**。

**劣势**：

- `TreeNode`  占用的空间是普通节点的两倍，因此只有在桶中元素数量足够多时才会使用树化。树化可以减少性能下降，但在  `hashCode` 分布良好的情况下，树化通常不会被使用。





### 哈希值用于确定数组索引

`HashMap` 的底层结构是一个数组（称为哈希表），每个键值对的存储位置由 **哈希值** 决定。为了将键的哈希值映射到数组的索引位置，`HashMap` 会通过以下步骤计算出合适的数组索引：

```java
// 计算哈希值
int hash = key.hashCode();

// 计算索引位置，通常通过对数组长度进行取模运算或与操作
int index = (n - 1) & hash; // n 是数组的长度
```

其中，**`n `**表示  `HashMap`  数组的长度，通常是 2 的幂次方（如 16、32、64 等）。因为 `HashMap` 默认的初始容量是 16，并且在扩容时容量总是保持 2 的幂次方。

当 `n` 是 2 的幂次方时，<font color="red">**`& (n - 1)` 可以确保哈希值的低位参与索引计算**</font>，这在大多数情况下能**让键值对分布更均匀，减少碰撞**。举个例子，

```yaml
hash:   101101110100
n - 1:        1111
&:       00000001100
```





### put 方法

在 `HashMap` 中，<font color="red">**每个键（`Key`）必须是唯一的**</font>。因此，当 `HashMap.put(e, PRESENT)` 方法被调用时，`HashMap` 会计算键 `e` 的哈希值，并根据哈希值将其放入适当的位置。

- 如果键 `key` 在 `HashMap` 中已经存在，`put()` 方法不会插入新的键值对，而是替换掉旧的值，并返回旧的值。
- 如果键 `key` 在 `HashMap` 中不存在，`put()` 会插入新的键值对，并返回 `null`。

```java
public class HashMap<K,V> extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable {
    
    public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }
}
```



### 为什么 HashMap 需要 equals 方法

hashCode相等的两个对象，equals 不一定相等（比如散列冲突的情况）





### HashMap 提供的 clone() 方法为浅克隆

在 `HashMap` 的 `clone()` 方法中，只是复制了内部的结构（例如桶数组、链表头节点等），但并没有递归复制这些结构所引用的对象。所以 `HashMap` 是浅克隆。

```
HashMap<String, String> originalMap = new HashMap<>();
originalMap.put("key1", "value1");

HashMap<String, String> shallowClonedMap = (HashMap<String, String>) originalMap.clone();

// 浅克隆，两个 HashMap 中的元素指向相同的对象
System.out.println(originalMap.get("key1") == shallowClonedMap.get("key1"));  // 输出 true
```

深克隆的代码为：

```java
public class HashMapDeepClone {

    public static void main(String[] args) {
        // 创建原始 HashMap
        HashMap<String, MyObject> originalMap = new HashMap<>();
        originalMap.put("key1", new MyObject("value1"));
        originalMap.put("key2", new MyObject("value2"));

        // 使用 Stream 进行深克隆
        HashMap<String, MyObject> deepClonedMap = originalMap.entrySet()
                .stream()
                .collect(Collectors.toMap(
                        Map.Entry::getKey,                        // 克隆键
                        entry -> entry.getValue().deepClone(),    // 深克隆值
                        (e1, e2) -> e1,                           // 处理键冲突（这里不会发生）
                        HashMap::new                              // 创建新的 HashMap
                ));

        // 验证深克隆
        System.out.println(originalMap.get("key1") == deepClonedMap.get("key1"));  // 输出 false，表明是不同的对象
        System.out.println(originalMap.get("key1").getData().equals(deepClonedMap.get("key1").getData()));  // 输出 true，表明数据一致
    }
}
```







### 参考资料

[HashMap 源码分析 | JavaGuide](https://javaguide.cn/java/collection/hashmap-source-code.html)