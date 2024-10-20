###  什么是Stream API

Stream API 是 Java 8 引入的一个用于**处理集合数据**的新抽象。

它提供了一种更便利、更灵活的方式来操作集合，支持函数式编程的风格。Stream API 的目标是通过更简洁的语法和更高效的执行方式，使得对集合的操作更加方便和优雅。简而言之，Stream API是关于为JDK **提供众所周知的 map-filter-reduce 算法的实现。**



### Stream API 的结构

这种风格将要处理的元素集合看作一种流， 流在管道中传输， 并且可以在管道的节点上进行处理， 比如筛选， 排序，聚合等。**元素流在管道中经过中间操作（intermediate operation）的处理，最后由终端操作(terminal operation)得到前面处理的结果**。其中，

- 中间操作返回的是新的 stream，可以对流进行链式调用，如筛选、映射、排序等。这些操作是惰性的，即它们不会立即执行，而是在有终端操作时才会执行。
- 终端操作触发流的遍历，执行中间操作链，并产生一个非 stream的 最终结果(包括void），如收集到集合、聚合操作等。

```
+--------------------+       +------+   +------+   +---+   +-------+
| stream of elements +-----> |filter+-> |sorted+-> |map+-> |collect|
+--------------------+       +------+   +------+   +---+   +-------+
```

举个例子， 以上的流程转换为 Java 代码为：

```
List<String> fruits = List.of("apple", "banana", "orange", "kiwi", "grape");

// 使用 stream() 方法将集合转换为流
List<String> result = fruits.stream()
                            .filter(fruit -> fruit.length() > 5)  // 过滤长度大于5的水果
                            .map(String::toUpperCase)            // 将水果名称转换为大写
                            .sorted()                            // 排序
                            .collect(Collectors.toList());       // 将流中的元素收集为列表
```



### Stream 不存储任何数据

性质1:   流是不存储任何数据的对象。

Stream API的设计方式是，只要您没有在流模式中创建任何非流对象，就不会对您的数据进行计算。

> 该性质的具体解析查看  [Stream教程官网](https://dev.java/learn/api/streams/map-filter-reduce/#intermediate-operations)的 Optimizing the Map-Filter-Reduce Algorithm这部分内容
>
> 另外， [`distinct()`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Stream.html#distinct()) and [`sorted()`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Stream.html#sorted())不适合这个规则，具体解析查看[Adding Intermediate Operations on a Stream - Dev.java](https://dev.java/learn/api/streams/intermediate-operation/#flatmap)的Removing Duplicates and Sorting a Stream这部分内容



### Stream 的特征

[Finding the Characteristics of a Stream - Dev.java](https://dev.java/learn/api/streams/characteristics/#sorted)

| Characteristic                                               | Comment                                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [*ORDERED*](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Spliterator.html#ORDERED) | The order in which the elements of the stream are processed matters. |
| [*DISTINCT*](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Spliterator.html#DISTINCT) | There are no doubles in the elements processed by that stream. |
| [*NONNULL*](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Spliterator.html#NONNULL) | There are no null elements in that stream.                   |
| [*SORTED*](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Spliterator.html#SORTED) | The elements of that stream are sorted.                      |
| [*SIZED*](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Spliterator.html#SIZED) | The number of elements this stream processes is known.       |
| [*SUBSIZED*](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Spliterator.html#SUBSIZED) | Splitting this stream produces two [*SIZED*](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Spliterator.html#SIZED) streams. |





###  遵循最佳实践

- **当场创建和使用流**:  如您所见，您只允许调用流上的一个方法，即使这个方法是中间的。因此，将流存储在字段或局部变量中是无用的，有时甚至是危险的。编写将流作为参数的方法也可能是危险的，因为您不能确定您收到的流还没有被操作过。

- **流不应该有任何副作用**。编写一个修改流本身之外的一些变量或字段的操作是一个可以避免的坏主意。





### 参考资料

[Processing Data in Memory Using the Stream API - Dev.java](https://dev.java/learn/api/streams/map-filter-reduce/#good-practices)