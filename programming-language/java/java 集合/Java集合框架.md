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

















# 参考资料

[java.util (Java SE 21 & JDK 21) (oracle.com)](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/package-summary.html)

[Java集合简介 - 廖雪峰的官方网站 (liaoxuefeng.com)](https://www.liaoxuefeng.com/wiki/1252599548343744/1265109905179456)

[The Collections Framework - Dev.java](https://dev.java/learn/api/collections-framework/)

[Java 集合框架 | 菜鸟教程 (runoob.com)](https://www.runoob.com/java/java-collections.html)