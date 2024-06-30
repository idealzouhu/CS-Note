# 五、Stream API

## 5.1  map-filter-reduce 算法

`map-filter-reduce` 是处理数据的非常经典的算法（也是一种常用于集合处理的编程范式），它主要包含三个步骤：映射（`map`）、过滤（`filter`）、归约（`reduce`）。这一组合的操作可以在函数式编程中高效地对集合进行转换和处理。

首先介绍一下 [Java 官网](https://dev.java/learn/api/streams/map-filter-reduce/#intermediate-operations)提供的案例，让我们举个例子，假设您有一组Sale对象，它具有三个属性：日期、产品引用和金额。

```java
public class Sale {
    private String product;
    private LocalDate date;
    private int amount;

    // constructors, getters, setters
    // equals, hashCode, toString
}
```

假设您需要计算3月份的销售总额。您可能会编写以下代码：

```java
List<Sale> sales = ...; // this is the list of all the sales
int amountSoldInMarch = 0;
for (Sale sale: sales) {
    if (sale.getDate().getMonth() == Month.MARCH) {
        amountSoldInMarch += sale.getAmount();
    }
}
System.out.println("Amount sold in March: " + amountSoldInMarch);
```

上述代码的操作可以分为以下三步:

1. **映射（Map）：**使用 `map` 操作将销售记录映射为其销售金额。
2. **过滤（Filter）：**使用 `filter` 操作筛选出满足条件的销售记录，即在三月份的销售记录。
3. **归约（Reduce）：**使用 `sum` 操作对销售金额进行归约，计算出在三月份的销售金额总和。类似于SQL语言里面的 aggregation 操作

SQL语言在以可读的方式表达这种处理方面做得非常好

```sql
select sum(amount)
from Sales
where extract(month from date) = 3;
```

可以看到，在SQL中，您正在编写的是对您需要的结果的描述：3月份所有销售额的总和。您的数据库服务器有责任弄清楚如何有效地计算该结果。然而,   计算销售总额的上述 Java 代码片段是对如何计算销售总额的逐步描述。它以命令式的方式进行了精确描述。它几乎没有为Java运行时优化此计算留下空间。

因此,  我们需要引入 Stream API 。   Stream API的两个目标是

- 能够**创建更具可读性和表现力的代码**
- 为Java运行时提供一些回旋余地来**优化计算**。

> 另外, [Processing Data in Memory Using the Stream API - Dev.java](https://dev.java/learn/api/streams/map-filter-reduce/#intermediate-operations)网站里面介绍了为什么不直接利用Collction 接口实现map-filter-reduce算法的原因:  创建不必要的中间结构，对内存和CPU都有很高的开销。
>
> Stream 接口避免创建中间结构来存储映射或过滤的对象。



## 5.2 什么是Stream API

Stream API 是 Java 8 引入的一个用于**处理集合数据**的新抽象。

它提供了一种更便利、更灵活的方式来操作集合，支持函数式编程的风格。Stream API 的目标是通过更简洁的语法和更高效的执行方式，使得对集合的操作更加方便和优雅。简而言之，Stream API是关于为JDK **提供众所周知的 map-filter-reduce 算法的实现。**

这种风格将要处理的元素集合看作一种流， 流在管道中传输， 并且可以在管道的节点上进行处理， 比如筛选， 排序，聚合等。**元素流在管道中经过中间操作（intermediate operation）的处理，最后由终端操作(terminal operation)得到前面处理的结果**。其中，中间操作返回的是新的 stream，可以对流进行链式调用，如筛选、映射、排序等；终端操作触发流的遍历，执行中间操作链，并产生一个非 stream的 最终结果(包括void），如收集到集合、聚合操作等。

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



## 5.3 Stream的性质

性质1:   流是不存储任何数据的对象。

Stream API的设计方式是，只要您没有在流模式中创建任何非流对象，就不会对您的数据进行计算。

> 该性质的具体解析查看  [Stream教程官网](https://dev.java/learn/api/streams/map-filter-reduce/#intermediate-operations)的Optimizing the Map-Filter-Reduce Algorithm这部分内容
>
> 另外， [`distinct()`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Stream.html#distinct()) and [`sorted()`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Stream.html#sorted())不适合这个规则，具体解析查看[Adding Intermediate Operations on a Stream - Dev.java](https://dev.java/learn/api/streams/intermediate-operation/#flatmap)的Removing Duplicates and Sorting a Stream这部分内容



### 5.3.1 流的特征

[Finding the Characteristics of a Stream - Dev.java](https://dev.java/learn/api/streams/characteristics/#sorted)

| Characteristic                                               | Comment                                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [*ORDERED*](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Spliterator.html#ORDERED) | The order in which the elements of the stream are processed matters. |
| [*DISTINCT*](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Spliterator.html#DISTINCT) | There are no doubles in the elements processed by that stream. |
| [*NONNULL*](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Spliterator.html#NONNULL) | There are no null elements in that stream.                   |
| [*SORTED*](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Spliterator.html#SORTED) | The elements of that stream are sorted.                      |
| [*SIZED*](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Spliterator.html#SIZED) | The number of elements this stream processes is known.       |
| [*SUBSIZED*](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Spliterator.html#SUBSIZED) | Splitting this stream produces two [*SIZED*](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Spliterator.html#SIZED) streams. |







## 5.4 创建流

### 5.4.1 创建流

在 Java 8 中, 集合接口有两个方法来生成流：

- [`stream()`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Arrays.html#stream(int[])) − 为集合创建串行流。
- **parallelStream()** − 为集合创建并行流。

> stream() 方法在 Collection接口

举个例子，以 `stream()` 函数为例，可以通过集合、数组、I/O 通道等方式创建流。

```
//  从 List 创建流
List<String> list = Arrays.asList("apple", "banana", "orange");
Stream<String> streamFromList = list.stream();

// 从数组创建 IntStream：
int[] array = {1, 2, 3, 4, 5};
IntStream streamFromArray = Arrays.stream(array);

// 使用 Stream.of 创建流：
Stream<String> streamFromValues = Stream.of("apple", "banana", "orange");
```



事实上，创建流还有很多方式，更多细节查看 [Creating Streams - Dev.java](https://dev.java/learn/api/streams/creating/#intro)



## 5.5 中间操作

中间操作是 map-filter-reduce 算法中 map 和 filter 实现

### 5.5.1  流的映射

**(1) map()**

映射流包括使用[`Function`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/function/Function.html)转换其元素。

您可以使用 [`map()`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Stream.html#map(java.util.function.Function)) 方法将一个流映射到另一个流，该方法将[`Function`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/function/Function.html)作为参数。映射流意味着**该流处理的所有元素都将使用[`Function`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/function/Function.html)进行转换**。举个例子，示例代码如下：

```java
List<String> strings = List.of("one", "two", "three", "four");
Function<String, Integer> toLength = String::length;
Stream<Integer> ints = strings.stream()
                              .map(toLength);
```

需要注意的是，这段代码里面没有终端操作，因此这段代码没有做任何事



另外，有三种方法可以从 [`Stream`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Stream.html) 转到原始类型的流：[`mapToInt()`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Stream.html#mapToInt(java.util.function.ToIntFunction)), [`mapToLong()`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Stream.html#mapToLong(java.util.function.ToLongFunction)) and [`mapToDouble()`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Stream.html#mapToDouble(java.util.function.ToDoubleFunction))。举个例子，示例代码如下：

```java
List<String> strings = List.of("one", "two", "three", "four");
IntSummaryStatistics stats = strings.stream()
                                    .mapToInt(String::length)
                                    .summaryStatistics();
System.out.println("stats = " + stats);
```

 输出结果为

```
 stats = IntSummaryStatistics{count=4, sum=15, min=3, average=3,750000, max=5}
```



**(2)flatmap()**

在 Java 中，[`flatMap()`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Stream.html#flatMap(java.util.function.Function)) 是流（Stream）类的一个中间操作，用于处理包含多个元素的流。它的作用是**将每个流元素映射为一个流，然后将这些流合并成一个新的流**。

举个例子，假设有一个列表，其中包含多个单词，我们希望将每个单词拆分为字符，并将所有字符合并到一个新的流中：

```
List<String> words = Arrays.asList("Hello", "World");

// 使用 flatMap 将每个单词拆分为字符，并合并为一个新流
List<String> characters = words.stream()
                              .flatMap(word -> Arrays.stream(word.split("")))
                              .collect(Collectors.toList());

System.out.println(characters);
```

在这个例子中，`flatMap` 将每个单词映射为一个字符流，然后将这些字符流合并成一个新的流，最终得到一个包含所有字符的列表。输出结果为：

```
[H, e, l, l, o, W, o, r, l, d]
```



**(3)mapMulti()**

[`mapMulti()`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Stream.html#mapMulti(java.util.function.BiConsumer)) 





### 5.5.2 流的过滤

过滤是在流（Stream）上进行元素过滤。此方法适用于对象流和原始类型流。

举个例子，

```
List<String> strings = List.of("one", "two", "three", "four");
long count = strings.stream()
                    .map(String::length)
                    .filter(length -> length == 3)
                    .count();
System.out.println("count = " + count);
```

其中，[`count()`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Stream.html#count())是 Stream API的另一个终端操作，它只计算已处理元素的数量

结果如下：

```
count = 2
```





### 5.5.3 其他操作

- [`distinct()`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Stream.html#distinct())方法  ： 使用hashCode（）和equals（）方法来发现重复项， 然后去除重复项

- [`sorted()`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Stream.html#sorted())： 有一个comparator的重载，该比较器将用于比较和排序流的元素。如果您不提供比较器，则Stream API假定您的流的元素具有可比性。如果不是，则会引发ClassCastException。

* [`skip(long n)`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Stream.html#skip(long))  方法： 用于跳过流中的前 `n` 个元素，返回一个新的流。如果流的长度小于 `n`，则返回一个空流。

- [`limit(long maxSize)`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Stream.html#limit(long)) 方法： 用于限制流的大小，返回一个包含不超过 `maxSize` 个元素的新流。

- [`concat()`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Stream.html#concat(java.util.stream.Stream,java.util.stream.Stream))  ： 用于连接两个流，将它们合并成一个新的流。  建议用 flatMap()  来替代这个函数，举个例子：

```java
List<Integer> list0 = List.of(1, 2, 3);
List<Integer> list1 = List.of(4, 5, 6);
List<Integer> list2 = List.of(7, 8, 9);

// 1st pattern: concat
List<Integer> concat = 
    Stream.concat(list0.stream(), list1.stream())
          .collect(Collectors.toList());

// 2nd pattern: flatMap
List<Integer> flatMap =
    Stream.of(list0.stream(), list1.stream(), list2.stream())
          .flatMap(Function.identity())
          .collect(Collectors.toList());

System.out.println("concat  = " + concat);
System.out.println("flatMap = " + flatMap);
```

- [`peek()`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Stream.html#peek(java.util.function.Consumer))： ，用于对流中的每个元素执行操作，同时保持流的原始结构。它返回的流与原始流相同，但可以在执行 `peek()` 时观察到中间的操作。这个**可以用来调试流**。

  ```java
  List<String> strings = List.of("one", "two", "three", "four");
  List<String> result =
          strings.stream()
                  .peek(s -> System.out.println("Starting with = " + s))
                  .filter(s -> s.startsWith("t"))
                  .peek(s -> System.out.println("Filtered = " + s))
                  .map(String::toUpperCase)
                  .peek(s -> System.out.println("Mapped = " + s))
                  .collect(Collectors.toList());
  System.out.println("result = " + result);
  ```





## 5.6 终端操作

终端操作（Terminal Operations）是 map-filter-reduce 算法中 reduce 实现。终端操作（Terminal Operations）是流操作中的最后一步，它们产生最终的结果或副作用。终端操作会触发流的遍历，并生成最终的结果或执行一些操作。

>  如果流不以终端操作结束，则流不会处理任何数据。



以下是一些常见的终端操作：

1. **forEach()：** 对流中的每个元素执行指定的操作。例如：

   ```
   javaCopy codeList<String> words = Arrays.asList("apple", "banana", "orange");
   words.stream().forEach(System.out::println);
   ```

2. **count()：** 返回流中元素的数量。例如：

   ```
   javaCopy code
   long count = words.stream().count();
   ```

3. **collect()：** 将流中的元素收集到一个集合或其他数据结构中。例如：

   ```
   javaCopy code
   List<String> collectedList = words.stream().collect(Collectors.toList());
   ```

4. **reduce()：** 对流中的元素进行归约操作，返回一个 Optional。（官方不建议使用这个方法）例如：

   ```
   javaCopy code
   Optional<String> concatenated = words.stream().reduce((s1, s2) -> s1 + ", " + s2);
   ```

5. **min() 和 max()：** 返回流中的最小或最大元素。例如：

   ```
   javaCopy codeOptional<String> minElement = words.stream().min(String::compareTo);
   Optional<String> maxElement = words.stream().max(String::compareTo);
   ```

6. **anyMatch()、allMatch() 和 noneMatch()：** 用于检查流中的元素是否匹配给定的条件。例如：

   ```
   javaCopy codeboolean anyMatch = words.stream().anyMatch(s -> s.startsWith("a"));
   boolean allMatch = words.stream().allMatch(s -> s.length() > 3);
   boolean noneMatch = words.stream().noneMatch(s -> s.contains("z"));
   ```

需要注意的是，尽量避免使用  [`reduce()`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Stream.html#reduce(java.util.function.BinaryOperator))  方法



### 5.6.1 在集合或数组中收集流元素

[Adding a Terminal Operation on a Stream - Dev.java](https://dev.java/learn/api/streams/terminal-operations/#collecting)



### 5.6.2 *short-circuiting* 方法

"short-circuiting" 方法是指在处理流时，当满足某个条件后，不再继续处理剩余的元素，提前结束处理流的过程。这可以帮助提高性能，因为它避免了对整个流的遍历。

在 Java 的 Stream API 中，有一些操作是 short-circuiting 的，例如：

1. **findFirst()：** 返回第一个符合条件的元素后，立即结束流的处理。
2. **findAny()：** 返回任意符合条件的元素后，立即结束流的处理。
3. **limit(n)：** 限制流的大小为 `n`，一旦达到这个大小，就不再继续处理。
4. **anyMatch(predicate)：** 一旦有任意一个元素满足条件，就结束流的处理。
5. **allMatch(predicate)：** 一旦有任意一个元素不满足条件，就结束流的处理。
6. **noneMatch(predicate)：** 一旦有任意一个元素满足条件，就结束流的处理。



## 5.7 Collector

- `Collector` 是 Java Stream API 中的一个接口，定义了将流中元素累积到一个可变结果容器的操作。`Collector` 接口中定义了一系列方法，包括创建结果容器、将元素添加到容器、合并两个部分结果等。

- `Collectors` 是 `Collector` 接口的一个实用工具类，它提供了一些静态方法，用于创建常见的收集器实例。这些静态方法返回的收集器已经预先定义了一些常见的行为，例如将流中的元素收集到列表、集合、映射等中。

在简单的情况下，您可以直接使用 `Collectors` 中的静态方法，而不必手动实现 `Collector` 接口。但是，如果您需要自定义收集逻辑，或者需要处理特殊情况，可以实现 `Collector` 接口来创建自定义的收集器。



### 5.7.1 使用 Collector



### 5.7.2 使用 Collectors 工厂类创建收集器



### 5.7.3 通过实现 Collector 接口创建收集器





## 5.8 遵循最佳实践

- **当场创建和使用流**:  如您所见，您只允许调用流上的一个方法，即使这个方法是中间的。因此，将流存储在字段或局部变量中是无用的，有时甚至是危险的。编写将流作为参数的方法也可能是危险的，因为您不能确定您收到的流还没有被操作过。

- **流不应该有任何副作用**。编写一个修改流本身之外的一些变量或字段的操作是一个可以避免的坏主意。