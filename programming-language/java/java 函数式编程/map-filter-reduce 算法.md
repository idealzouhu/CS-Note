[TOC]

###  map-filter-reduce 算法

`map-filter-reduce` 是处理数据的非常经典的算法（也是一种常用于集合处理的编程范式），可以在函数式编程中高效地对集合进行转换和处理。它主要包含三个步骤：

- 映射（`map`）：将流中的每个元素应用一个函数，转换为新的元素
- 过滤（`filter`）：根据条件筛选流中的元素，保留满足条件的元素
- 归约（`reduce`）：通过累加器将流中的元素合并为一个单一的结果



### map-filter-reduce 算法的简单案例

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

1. **映射（Map）:**使用 `map` 操作将销售记录映射为其销售金额。
2. **过滤（Filter）：**使用 `filter` 操作筛选出满足条件的销售记录，即在三月份的销售记录。
3. **归约（Reduce）：**使用 `sum` 操作对销售金额进行归约，计算出在三月份的销售金额总和。类似于SQL语言里面的 aggregation 操作

SQL语言在以可读的方式表达这种处理方面做得非常好

```sql
select sum(amount)
from Sales
where extract(month from date) = 3;
```

可以看到，在SQL中，您正在编写的是对您需要的结果的描述：3月份所有销售额的总和。您的数据库服务器有责任弄清楚如何有效地计算该结果。然而,   计算销售总额的上述 Java 代码片段是对如何计算销售总额的逐步描述。它以命令式的方式进行了精确描述。它几乎没有为Java运行时优化此计算留下空间。



### 使用 stream 流实现 map-filter-reduce 算法

因此,  我们需要引入 Stream API 。   Stream API的两个目标是

- 能够**创建更具可读性和表现力的代码**
- 为Java运行时提供一些回旋余地来**优化计算**。



### 为什么不用 Collection 接口实现map-filter-reduce算法

另外, [Processing Data in Memory Using the Stream API - Dev.java](https://dev.java/learn/api/streams/map-filter-reduce/#intermediate-operations)网站里面介绍了为什么不直接利用Collction 接口实现map-filter-reduce算法的原因:  **创建不必要的中间结构，对内存和CPU都有很高的开销。**

Stream 接口避免创建中间结构来存储映射或过滤的对象。