[TOC]

## 一、创建流

### 1.1 流的类型

在 Java 8 中, 集合接口有两个方法来生成流：

- [`stream()`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Arrays.html#stream(int[])) − 为集合创建串行流。
- **parallelStream()** − 为集合创建并行流。

> stream() 方法在 Collection接口



### 1.2 创建 Stream 的常见操作

举个例子，以 `stream()` 函数为例，可以通过集合、数组、I/O 通道等方式创建流。

```java
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



## 二、 中间操作

中间操作是 map-filter-reduce 算法中 map 和 filter 实现。

中间操作会返回一个新的 `Stream`，因此可以**链式调用**。这些操作是**惰性的**，即它们不会立即执行，而是在有终端操作时才会执行。这意味着中间操作只是定义了流的转换规则，实际处理会推迟到终端操作执行时。

具体细节查看  [Reducing a Stream - Dev.java](https://dev.java/learn/api/streams/reducing/)

### 2.1 流的映射 

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





### 2.2 流的过滤

过滤是在流（Stream）上进行元素过滤。此方法适用于对象流和原始类型流。

举个例子，

```java
List<String> strings = List.of("one", "two", "three", "four");
long count = strings.stream()
                    .map(String::length)
                    .filter(length -> length == 3)
                    .count();
System.out.println("count = " + count);   // 结果为：count = 2
```

其中，[`count()`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Stream.html#count())是 Stream API的另一个终端操作，它只计算已处理元素的数量



### 2.3 其他中间操作

- [`distinct()`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Stream.html#distinct())方法  ： 使用hashCode（）和equals（）方法来发现重复项， 然后去除重复项

- [`sorted()`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Stream.html#sorted())： 有一个comparator的重载，该比较器将用于比较和排序流的元素。如果您不提供比较器，则Stream API假定您的流的元素具有可比性。如果不是，则会引发ClassCastException。

- [`skip(long n)`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Stream.html#skip(long))  方法： 用于跳过流中的前 `n` 个元素，返回一个新的流。如果流的长度小于 `n`，则返回一个空流。

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





## 三、 终端操作

终端操作（Terminal Operations）是 map-filter-reduce 算法中 reduce 实现。

终端操作（Terminal Operations）是流操作中的最后一步，它们产生最终的结果或副作用。终端操作会触发流的遍历，并生成最终的结果或执行一些操作。在执行后，流就会被消耗，不能再使用。

>  如果流不以终端操作结束，则流不会处理任何数据。



### 3.1 reduce() 的使用方法

[**`reduce()`**](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Stream.html#reduce(java.util.function.BinaryOperator))  方法： 对流中的元素进行归约操作，返回一个 Optional。（官方不建议使用这个方法）例如：

```java
Optional<String> concatenated = words.stream().reduce((s1, s2) -> s1 + ", " + s2);
```

具体细节查看：[Reducing a Stream - Dev.java](https://dev.java/learn/api/streams/reducing/#with-a-binary-operator)



### 3.2 *short-circuiting* 方法

"short-circuiting" 方法是指在处理流时，当满足某个条件后，不再继续处理剩余的元素，提前结束处理流的过程。这可以帮助提高性能，因为它避免了对整个流的遍历。

在 Java 的 Stream API 中，有一些操作是 short-circuiting 的，例如：

1. **findFirst()：** 返回第一个符合条件的元素后，立即结束流的处理。
2. **findAny()：** 返回任意符合条件的元素后，立即结束流的处理。
3. **limit(n)：** 限制流的大小为 `n`，一旦达到这个大小，就不再继续处理。
4. **anyMatch(predicate)：** 一旦有任意一个元素满足条件，就结束流的处理。
5. **allMatch(predicate)：** 一旦有任意一个元素不满足条件，就结束流的处理。
6. **noneMatch(predicate)：** 一旦有任意一个元素满足条件，就结束流的处理。



### 3.3 其他的终端操作

需要注意的是，尽量避免使用  [`reduce()`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/Stream.html#reduce(java.util.function.BinaryOperator))  方法

以下是一些常见的终端操作：

1. **forEach()：** 对流中的每个元素执行指定的操作。例如：

   ```java
   codeList<String> words = Arrays.asList("apple", "banana", "orange");
   words.stream().forEach(System.out::println);
   ```

2. **count()：** 返回流中元素的数量。例如：

   ```java
   long count = words.stream().count();
   ```
   
3. **collect()：** 将流中的元素收集到一个集合或其他数据结构中。例如：

   ```java
   List<String> collectedList = words.stream().collect(Collectors.toList());
   ```
   
4. **min() 和 max()：** 返回流中的最小或最大元素。例如：

   ```java
   Optional<String> minElement = words.stream().min(String::compareTo);
   Optional<String> maxElement = words.stream().max(String::compareTo);
   ```

5. **anyMatch()、allMatch() 和 noneMatch()：** 用于检查流中的元素是否匹配给定的条件。例如：

   ```java
   codeboolean anyMatch = words.stream().anyMatch(s -> s.startsWith("a"));
   boolean allMatch = words.stream().allMatch(s -> s.length() > 3);
   boolean noneMatch = words.stream().noneMatch(s -> s.contains("z"));
   ```

具体细节查看 [Adding a Terminal Operation on a Stream - Dev.java](https://dev.java/learn/api/streams/terminal-operations/#collecting)



## 参考资料

[Stream - Dev.java](https://dev.java/learn/api/streams/terminal-operations/)