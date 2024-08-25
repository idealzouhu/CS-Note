###  Queue 和 Deque

[`Queue`](https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/util/Queue.html) 接口： 建模队列。

[`Deque`](https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/util/Deque.html)接口：建模双端队列。<font color="red">**可以用来作为数据结构的队列或者栈来使用**</font>。

<img src="images/02_queue-hierarchy.png" alt="The Queue Interface Hierarchy" style="zoom:33%;" />

> 该图是 Stack 和 Queue的 接口层次结构， 仅描述了重要的接口



### 栈和队列

栈（Stack）和队列（Queue）是计算机中经典的数据结构。

- 栈：后进先出， 即LIFO（Last In, First Out）。
- 队列：先进先出， 即 FIFO（First In First Out）。

这些数据结构非常简单，提供了三个主要操作：

- *push(element)*: 向队列或栈添加一个元素。
- *pop()*:  从栈中移除一个元素，即最后添加的元素。//  *poll()*: 从队列中移除一个元素，即最早添加的元素。
- *peek()*: 允许查看通过pop()或poll()获取的元素，但不从队列或栈中移除。



### 使用 Queue 接口建模队列

| Operation | Method                                                       | Behavior when the queue is full or empty                     |
| --------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| push      | [`add(element)`](https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/util/Queue.html#add(E)) | throws an [`IllegalStateException`](https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/lang/IllegalStateException.html) |
|           | [`offer(element)`](https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/util/Queue.html#offer(E)) | returns `false`                                              |
| poll      | [`remove()`](https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/util/Queue.html#remove()) | throws a [`NoSuchElementException`](https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/util/NoSuchElementException.html) |
|           | [`poll()`](https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/util/Queue.html#poll()) | returns `false`                                              |
| peek      | [`element()`](https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/util/Queue.html#element()) | throws a [`NoSuchElementException`](https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/util/NoSuchElementException.html) |
|           | [`peek()`](https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/util/Queue.html#peek()) | returns `null`                                               |



### 使用 Deque 对栈进行建模





### 使用 Deque 对队列进行建模

| FIFO Operation | Method                                                       | Behavior when the queue is full or empty                     |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| push           | [`addLast(element)`](https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/util/Deque.html#addLast(E)) | throws an [`IllegalStateException`](https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/lang/IllegalStateException.html) |
|                | [`offerLast(element)`](https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/util/Deque.html#offerLast(E)) | returns `false`                                              |
| poll           | [`removeFirst()`](https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/util/Deque.html#removeFirst()) | throws a [`NoSuchElementException`](https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/util/NoSuchElementException.html) |
|                | [`pollFirst()`](https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/util/Deque.html#pollFirst()) | returns `null`                                               |
| peek           | [`getFirst()`](https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/util/Deque.html#getFirst()) | throws a [`NoSuchElementException`](https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/util/NoSuchElementException.html) |
|                | [`peekFirst()`](https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/util/Deque.html#peekFirst()) | returns `null`                                               |





###  Queue 和 Deque 的实现类

实现了 Queue 和 Deque

- [`ArrayDeque`](https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/util/ArrayDeque.html) 
- [`LinkedList`](https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/util/LinkedList.html)

实现了  Queue 接口

- [`PriorityQueue`](https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/util/PriorityQueue.html)



### 参考资料

[Storing Elements in Stacks and Queues - Dev.java](https://dev.java/learn/api/collections-framework/stacks-queues/)