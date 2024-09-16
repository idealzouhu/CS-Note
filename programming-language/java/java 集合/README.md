### Java 集合基础

给出集合使用的常规操作，以及不同类之间的关系。

 [Java集合框架.md](Java集合框架.md) 

 [Collection 层次结构.md](Collection 层次结构.md) 

 [List 层次结构.md](List 层次结构.md) 

 [Set 层次结构.md](Set 层次结构.md) 

 [Queue 层次结构.md](Queue 层次结构.md) 

 [Map 层次结构.md](Map 层次结构.md) 

 [Compator 接口.md](Compator 接口.md) 

 [迭代器的使用.md](迭代器的使用.md) 





### Java 集合源代码 （线程不安全）

 [ArrayList 源码分析.md](ArrayList 源码分析.md) ：分析 `ArrayList` 的底层数据结构和扩容机制。

 [LinkedList 源码分析](LinkedList 源码分析.md) ：分析 `LinkedList` 的底层数据结构和时间复杂度。

 [HashMap 源码分析](HashMap 源码分析.md) ：分析 `HashMap` 的底层数据结构、哈希桶中链表如何转换为红黑树。

 [TreeMap 源码分析](TreeMap 源码分析.md) ：

 [HashSet 源码分析](HashSet 源码分析.md) ：分析 HashSet、LinkedHashSet 类的源码。

 [PriorityQueue 源码分析](PriorityQueue 源码分析.md) ：



### Java 集合源代码 （线程安全）

JDK 提供的这些并发容器大部分位于 `java.util.concurrent` 包中，一般基于 CAS + Volatile 来实现，对数据的增删改做出限制。另外，`Collections.synchronized` 包装方法可以将普通的容器包装成线程安全的容器，即在需要同步访问的方法上加上关键字 `synchronized` ，性能开销较大。

[CopyOnWriteArrayList 源码分析](CopyOnWriteArrayList 源码分析.md) ：使用写时复制( Copy-On-Write)思想保证读写不互斥。

[BlockingQueue 源码分析](BlockingQueue 源码分析.md) : 阻塞队列与普通队列之间的最大不同点在于阻塞队列提供了阻塞式的添加和删除方法，并分析 BlockingQueue 的不同实现类。

