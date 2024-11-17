### 什么是 Predicate 类

`Predicate`  是一个函数式接口，通常用于**判断条件的场景**。

`boolean test(T t)` 是  `Predicate` 接口中的一个抽象方法，接受一个类型为 `T` 的输入参数，并返回一个布尔值（`true` 或 `false`）。这个方法通常用于测试某个条件，判断输入的对象是否满足该条件。

```java
package java.util.function;


@FunctionalInterface
public interface Predicate<T> {
	
	// 测试此谓词是否适用于给定的输入
	boolean test(T t);
	
	// 返回一个新的谓词，表示此谓词与另一个谓词的逻辑“与”操作
	default Predicate<T> and(Predicate<? super T> other) {
        Objects.requireNonNull(other);
        return (t) -> test(t) && other.test(t);
    }
    
    // 返回一个新的谓词，表示此谓词的逻辑“非”操作
    default Predicate<T> negate() {
        return (t) -> !test(t);
    }
    
    // 返回一个新的谓词，表示此谓词与另一个谓词的逻辑“或”操作
    default Predicate<T> or(Predicate<? super T> other) {
        Objects.requireNonNull(other);
        return (t) -> test(t) || other.test(t);
    }
    
    // 返回一个谓词，表示给定的目标引用 targetRef 是否等于输入对象 T
     static <T> Predicate<T> isEqual(Object targetRef) {
        return (null == targetRef)
                ? Objects::isNull
                : object -> targetRef.equals(object);
    }
    
    // 返回一个新的谓词，表示给定谓词的逻辑“非”操作
    static <T> Predicate<T> not(Predicate<? super T> target) {
        Objects.requireNonNull(target);
        return (Predicate<T>)target.negate();
    }

}
```



### 使用案例

```java
public class PredicateExample {
    public static void main(String[] args) {
        // 一个整数列表
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9);

        // 创建一个 Predicate，用于判断偶数
        Predicate<Integer> isEven = number -> number % 2 == 0;

        // 使用 Predicate 过滤偶数
        numbers.stream()
                .filter(isEven)  // 只保留符合 isEven 条件的元素
                .forEach(System.out::println);  // 打印偶数
    }
}
```

