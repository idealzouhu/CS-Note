## HashSet 类

### HashSet 类简介

`HashSet` 不允许存储相同的元素，每个元素在集合中是唯一的。

```
public class HashSet<E>
    extends AbstractSet<E>
    implements Set<E>, Cloneable, java.io.Serializable
{

}
```





### 底层数据结构

`HashSet` 实际上是使用 `HashMap` 来存储元素。<font color="red">**存储元素的值实际上是键值对里面的 Key** </font>。

```java
public class HashSet<E>
    extends AbstractSet<E>
    implements Set<E>, Cloneable, java.io.Serializable
{	
     // 存储集合元素
	 private transient HashMap<E,Object> map;
}
```





### 如何保证每个元素唯一

`HashSet` 实际上是通过内部的 `HashMap` 来实现的。它的每一个元素被存储为 `HashMap` 的键 (`Key`)，并且**所有键的值 (`Value`) 统一为一个静态常量 `PRESENT`**。使用一个统一的常量 `PRESENT` 作为 `HashMap` 的值可以节省内存，避免为每个元素设置不同的 `Value`。

由于<font color="red">**`HashMap` 本身的特性就是不允许键重复**</font>，因此`HashSet` 能够确保集合中不出现重复元素。

```java
public class HashSet<E>
    extends AbstractSet<E>
    implements Set<E>, Cloneable, java.io.Serializable
{
	  private transient HashMap<E,Object> map;
	 
	  // Dummy value to associate with an Object in the backing Map
	  private static final Object PRESENT = new Object();
    
     public boolean add(E e) {
        return map.put(e, PRESENT)==null;
     }
}
```



**在 `HashMap` 中，每个键（`Key`）必须是唯一的**。因此，当 `HashMap.put(e, PRESENT)` 方法被调用时，`HashMap` 会计算键 `e` 的哈希值，并根据哈希值将其放入适当的位置。

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





## LinkedHashSet 类

### LinkedHashSet 类简介

LinkedHashSet 不仅保证元素的唯一性，还**保持元素的插入顺序**。