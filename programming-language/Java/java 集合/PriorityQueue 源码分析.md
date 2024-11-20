### PriorityQueue 类概述

**PriorityQueue** 是优先队列，可以按照比较器或者元素的自然顺序进行排序。

```java
public class PriorityQueue<E> extends AbstractQueue<E>
    implements java.io.Serializable {

}
```

<img src="images/image-20240828213637928.png" alt="image-20240828213637928" style="zoom:80%;" />



### 底层数据结构

在 `PriorityQueue` 里面，**数组 `queue` 用于存储堆中的元素**，使用平衡二叉堆（Balanced Binary Heap）来实现。具体细节如下：

数组表示堆：

1. 子节点索引：
   - 节点 `queue[n]` 的左子节点是 `queue[2*n+1]`。
   - 节点 `queue[n]` 的右子节点是 `queue[2*(n+1)]`。
2. 排序规则：
   - 如果提供了 `comparator`，则按照 `comparator` 排序。
   - 如果没有提供 `comparator`，则按照元素的自然顺序排序。
3. 堆性质：对于堆中的每个节点 n 和其所有后代节点 d，都有 n <= d。
4. 最小值位置：假设队列非空，最小值始终位于 queue[0]。

```java
transient Object[] queue; // non-private to simplify nested class access
```

当元素存储在一个数组中，并且需要支持优先级队列的功能时，**元素的优先级通常是通过元素本身或者与元素紧密关联的方式来存储的**。

1. 元素自身携带优先级：
   - 如果元素本身就是可比较的对象（比如实现了 Comparable 接口），那么元素的自然顺序就可以用来表示优先级。
   - 如果元素是一个复合对象，可以将优先级作为对象的一个属性（例如，一个 Pair<优先级, 数据> 或者自定义类中包含优先级字段）。
2. 数组索引映射优先级：
   - 数组中的每个位置存储的是一个完整的元素，包括其数据和优先级。在这种情况下，优先级不需要单独存储，因为它是元素的一部分。