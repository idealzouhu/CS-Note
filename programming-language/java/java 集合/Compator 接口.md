## 自定义排序 Comparator

### Comparator 原理

[`Comparator`](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Comparator.html) 接口的 `compare` 方法是定义自定义排序逻辑的核心。它的作用是比较两个对象，并根据比较结果返回一个整数值。

```
@FunctionalInterface
public interface Comparator<T> {
	 int compare(T o1, T o2);
}
```

根据 compare()  的返回值，对象排序如下：

- 正整数： `o1` 会排在 `o2` 的后面，即 `[ o2, o1 ]`， 升序排列
- **零**：`o1` 和 `o2` 被认为是相等的，排序中它们的相对位置不变， 即 `[ o1, o2 ]`。
- **负整数**：在排序中，`o1` 会排在 `o2` 的前面,  即 `[ o1, o2 ]`， 降序排列

实际上，当 `compare(o1, o2) <= 0` 时，都不交换位置。另外，一般而言，<font color="red">**集合都默认升序排列**</font>。



### 使用方法

```java
 Arrays.sort(numbers, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o1 - o2; // 升序排列
            }
        });
        
        
Arrays.sort(numbers, (o1, o2) -> o1 - o2); // 升序排列
```



## 自定义排序 Comparable 接口

### Comparable 接口实现原理

`Comparable` 接口用于定义自然排序顺序。它要求实现该接口的类提供一个方法来比较对象的大小，以便能够自动排序。

```java
public interface Comparable<T> {
    int compareTo(T o);
}
```

`compareTo` 方法的返回值表示当前对象 this 与 o 的相对大小：

- 正整数：当前对象大于 o ， 排序为 `[o,  this ]`
- 零：当前对象等于 o,  排序中它们的相对位置不变
- 负整数：当前对象小于 o,    排序为 `[this,  o ]`



### 使用方法

```java
import java.util.*;

public class Person implements Comparable<Person> {
    private String name;
    private int age;
	
	// getter，setter， constructor
	
    @Override
    public int compareTo(Person other) {
        // 按年龄升序排列
        return Integer.compare(this.age, other.age);
    }

    @Override
    public String toString() {
        return name + " (" + age + ")";
    }
}

public class ComparableExample {
    public static void main(String[] args) {
        List<Person> people = Arrays.asList(
            new Person("Alice", 30),
            new Person("Bob", 25),
            new Person("Charlie", 35)
        );

        Collections.sort(people); // 排序基于 Person 类里面 compareTo 方法的逻辑
        System.out.println(people); // [Bob (25), Alice (30), Charlie (35)]
    }
}

```



### Comparator 和 Comparable  有什么区别

- **`Comparable`**：通常用于定义对象的**自然排序**顺序，比如 `Collections.sort()` 方法直接使用实现了 `Comparable` 接口的对象进行排序。例如， `Collections.sort(people);`

- **`Comparator`**：用于自定义排序规则，通常与 `Collections.sort()` 和 `Arrays.sort()` 一起使用，通过传入 `Comparator` 实现类来进行排序。





## 常见的排序方法

**`Collections.sort()`** 和 **`Arrays.sort()`** 分别用于 `List` 和数组的排序。

```java
 Arrays.sort(numbers, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o1 - o2; // 升序排列
            }
        });
                


Collections.sort(results, new Comparator<Integer>() {
            public int compare(Integer num1, Integer num2) {
                return num2 - num1;
            }
});
```

**`Stream.sorted()`** 适用于流的排序，适合函数式编程风格。

```java
 List<String> words = Arrays.asList("apple", "banana", "cherry", "date");

// 使用 Stream.sorted(Comparator) 进行降序排序
List<String> sortedWords = words.stream()
                                .sorted(Comparator.reverseOrder())
                                .toList();
```

**`PriorityQueue`** 和 **`TreeSet`** 提供了基于优先级或自然顺序的自动排序特性。







### 参考资料

[Java基础面试题 | 小林coding (xiaolincoding.com)](https://xiaolincoding.com/interview/java.html#有一个学生类-想按照分数排序-再按学号排序-应该怎么做)