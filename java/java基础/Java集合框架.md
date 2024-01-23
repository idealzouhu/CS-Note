# 一、Java集合框架

## 1.1 什么是java集合框架

Java集合框架（Java Collections Framework）是 Java 平台提供的一组用于存储、操作和处理对象集合的类和接口的集合。它提供了一套通用的接口和实现类，使得在处理对象集合时更加方便、高效和灵活。



java集合框架重要发展历程主要如下：

- Java SE 2 首次引入 java 集合框架
-  Java SE 5 引入泛型
- Java8中引入了 lambda 表达式以及接口中的默认方法



## 1.2 集合与数组

事实上，由于集合是一个对象，并且给定一个对象是可扩展的，您可以在JDK提供的大多数集合上添加您需要的任何操作。用数组做这件事是不可能的，因为数组不是Java中的对象。

**集合能为您做什么，而数组不能？**

- 集合跟踪它包含的元素的数量

- 集合的容量不受限制：您可以在集合中添加（几乎）任意数量的元素

- 集合可以控制您可以在其中存储哪些元素。例如，您可以防止添加空元素

* 可以查询集合是否存在给定元素

- 集合提供与另一个集合相交或合并等操作。





# 二、集合框架具体内容

ava的集合类定义在`java.util`包中，支持泛型，主要提供了3种集合类，包括`List`，`Set`和`Map`。

Java集合使用统一的`Iterator`遍历，尽量不要使用遗留接口。

>  [java.util (Java SE 21 & JDK 21) (oracle.com)](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/package-summary.html)
>
> [Collections Framework Overview (Java SE 21 & JDK 21) (oracle.com)](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/doc-files/coll-overview.html)

## 2.1 整体框架

![img](images/2243690-9cd9c896e0d512ed.gif)

从集合框架图可以看到，**java 集合框架主要包括两种类型的容器**：

- 集合（Collection）： 存储一组元素，元素之间没有任何关系
- 映射（map）： 存储键值映射

集合框架是一个用来代表和操纵集合的统一架构。所有的集合框架都包含如下内容：

- **接口：集合的抽象数据类型**。例如 Collection、List、Set、Map 等。之所以定义多个接口，是为了以不同的方式操作集合对象
- **实现类：集合接口的具体实现**。从本质上讲，它们是可重复使用的数据结构，例如：ArrayList、LinkedList、HashSet、HashMap。
- **算法：实现集合接口的对象里的方法执行的一些有用的计算**。例如搜索和排序，这些算法实现了多态，那是因为相同的方法可以在相似的接口上有着不同的实现。

| Interface      | Hash Table                                                   | Resizable Array                                              | Balanced Tree                                                | Linked List                                                  | Hash Table + Linked List                                     |
| :------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `Set`          | [`HashSet`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/HashSet.html) |                                                              | [`TreeSet`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/TreeSet.html) |                                                              | [`LinkedHashSet`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/LinkedHashSet.html) |
| `List`         |                                                              | [`ArrayList`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/ArrayList.html) |                                                              | [`LinkedList`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/LinkedList.html) |                                                              |
| `Queue, Deque` |                                                              | [`ArrayDeque`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/ArrayDeque.html) |                                                              | [`LinkedList`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/LinkedList.html) |                                                              |
| `Map`          | [`HashMap`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/HashMap.html) |                                                              | [`TreeMap`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/TreeMap.html) |                                                              | [`LinkedHashMap`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/LinkedHashMap.html) |

## 2.2 遗留类和遗留接口

需要注意的是，由于Java的集合设计非常久远，中间经历过大规模改进，我们要注意到有一小部分集合类是遗留类，不应该继续使用：

- `Hashtable`：一种线程安全的 `Map` 实现；在并发环境中，可以使用 [`ConcurrentHashMap`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/ConcurrentHashMap.html)  作为替代品。
- `Vector`：一种线程安全的 `List` 实现；如果您在非并发环境中使用 `Vector` ，那么您可以安全地将其替换为 `ArrayList` 。
- `Stack`：基于 `Vector` 实现的 `LIFO` 的栈。在非并发环境中应替换为 [`ArrayDeque`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/ArrayDeque.html)。

还有一小部分接口是遗留接口，也不应该继续使用：

- `Enumeration<E>`：已被`Iterator<E>`取代。



## 1.3 集合框架设计特点

java集合设计特点：

- 实现**接口和实现类相分离**。

- **支持泛型**。限制在一个集合中只能放入同一种数据类型的元素，例如：

  ```
  List<String> list = new ArrayList<>(); // 只能放入String类型
  ```

- java访问集合统一通过**迭代器**来实现。这样做的好处在于无需知道集合内部元素是按什么方式存储的





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





# 四、Map层次结构

<img src="images/03_map-hierarchy.png" alt="The Map Interface Hierarchy" style="zoom:33%;" />

## 4.1 相关资料





# 五、Java迭代器

> [Iterator (Java SE 21 & JDK 21) (oracle.com)](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Iterator.html)
>
> [Iterating over the Elements of a Collection - Dev.java](https://dev.java/learn/api/collections-framework/iterating/)



在使用迭代器遍历集合时，同时修改了集合的结构。这是不安全的操作，可能导致  `ConcurrentModificationException ` 异常。具体原因是在迭代过程中，不允许修改集合的内容，除非使用迭代器的方法进行修改。

```
Collection<String> strings = new ArrayList<>();
strings.add("one");
strings.add("two");

Iterator<String> iterator = strings.iterator();
while (iterator.hasNext()) {

    String element = iterator.next();
    strings.remove(element);   // 错误代码
    // iterator.remove();      // 正确代码； 使用迭代器的 remove 方法
}

```



# 参考资料

[java.util (Java SE 21 & JDK 21) (oracle.com)](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/package-summary.html)

[Java集合简介 - 廖雪峰的官方网站 (liaoxuefeng.com)](https://www.liaoxuefeng.com/wiki/1252599548343744/1265109905179456)

[The Collections Framework - Dev.java](https://dev.java/learn/api/collections-framework/)

[Java 集合框架 | 菜鸟教程 (runoob.com)](https://www.runoob.com/java/java-collections.html)