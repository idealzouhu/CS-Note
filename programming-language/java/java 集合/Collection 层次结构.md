# 三、Collection 层次结构

## 3.1 Collection接口

[Collection存储元素的常见方法教程](https://dev.java/learn/api/collections-framework/collection-interface/#to-array)



## 3.2 List 和 set

 `List` 接口： 表示有序的集合，即元素按照插入顺序排列。通过索引可以访问列表中的元素。允许存储相同的元素，同一个元素可以出现多次。

`Set` 接口： 表示无序的集合，元素之间没有明确定义的顺序。不能通过索引访问元素。不允许存储相同的元素，每个元素在集合中是唯一的。

![The Collection Interface Hierarchy](images/01_interfaces-hierarchy.png)

> 该图是 Collection 接口层次结构， 仅描述了重要的接口



### 3.1.1 相关资料

[Extending Collection with List - Dev.java](https://dev.java/learn/api/collections-framework/lists/)

[Extending Collection with Set, SortedSet and NavigableSet - Dev.java](https://dev.java/learn/api/collections-framework/sets/)



## 3.3 Stack 和 Queue

<img src="images/02_queue-hierarchy.png" alt="The Queue Interface Hierarchy" style="zoom:33%;" />

> 该图是 Stack 和 Queue的 接口层次结构， 仅描述了重要的接口

### 3.3.1 主要操作

栈（Stack）和队列（Queue）是计算机中经典的数据结构。

- 栈：后进先出， 即LIFO（Last In, First Out）。
- 队列：先进先出， 即 FIFO（First In First Out）。

这些数据结构非常简单，提供了三个主要操作：

- *push(element)*: 向队列或栈添加一个元素。
- *pop()*:  从栈中移除一个元素，即最后添加的元素。//  *poll()*: 从队列中移除一个元素，即最早添加的元素。
- *peek()*: 允许查看通过pop()或poll()获取的元素，但不从队列或栈中移除。





### 3.3.1 相关资料

[Storing Elements in Stacks and Queues - Dev.java](https://dev.java/learn/api/collections-framework/stacks-queues/)