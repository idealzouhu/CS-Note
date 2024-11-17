### 什么是 Optional 类

`Optional` 类的核心目的是防止程序中大量使用 `null` 值，提供了一种<font color="red">**更加优雅的处理空值的方式**</font>, 并且减少 `NullPointerException` 的发生。

实际上，`Optional` 是一个可以包含 `null` 或者某个值的容器，它能够明确地表明一个值可能为空。



### Optional 类的方法

创建 Optionl 对象的常见方法。

```
 // Optional.of() 方法用于创建一个非空的 Optional 对象。
 // 如果传入的参数为 null，则抛出 NullPointerException 异常。
 Optional<String> optional = Optional.of("Hello");

// 如果值可能为空，可以使用 ofNullable 来避免空指针异常
String value = null;
Optional<String> optionalNullable = Optional.ofNullable(value);
```

**`Optional` 提供了处理缺失值和空值的多种方式**

| 方法                                                   | 说明                                                         |
| ------------------------------------------------------ | ------------------------------------------------------------ |
| `isPresent()`                                          | 如果 `Optional` 中包含值，则返回 `true`，否则返回 `false`。  |
| `ifPresent(Consumer<T> action)`                        | 如果 `Optional` 中包含值，则执行给定的 `action` 操作。       |
| `get()`                                                | 返回 `Optional` 中的值，如果值不存在则抛出 `NoSuchElementException` 异常。 |
| `orElse(T other)`                                      | 如果 `Optional` 中包含值，返回值；否则返回给定的默认值 `other`。 |
| `orElseGet(Supplier<? extends T> other)`               | 如果 `Optional` 中包含值，返回值；否则执行给定的 `Supplier` 并返回结果。 |
| `orElseThrow(Supplier<? extends X> exceptionSupplier)` | 如果 `Optional` 中包含值，返回值；否则抛出由 `exceptionSupplier` 提供的异常。 |
| `map(Function<? super T, ? extends U> mapper)`         | 如果 `Optional` 中包含值，应用给定的 `mapper` 函数并返回一个新的 `Optional`。 |
| `flatMap(Function<? super T, Optional<U>> mapper)`     | 如果 `Optional` 中包含值，应用给定的 `mapper` 函数并返回一个新的 `Optional`，如果值为空，则返回 `Optional.empty()`。 |
| `filter(Predicate<? super T> predicate)`               | 如果 `Optional` 中的值符合给定的 `predicate`，返回包含该值的 `Optional`，否则返回空 `Optional`。 |
| `isEmpty()`                                            | 如果 `Optional` 中不包含值，则返回 `true`，否则返回 `false`。 |
| `stream()`                                             | 如果 `Optional` 中有值，返回包含该值的流；否则返回一个空的流。 |





### 使用方式


```java
public class OptionalExample {
    public static void main(String[] args) {
        // 假设我们有不同类型的对象
        String errorMessage = "An error occurred!";
        String nullMessage = null;

        // 使用 Optional 处理 errorMessage，获取默认消息
        String resultMessage = Optional.ofNullable(errorMessage)
                .orElse("No error message available");

        // 使用 Optional 处理 nullMessage，获取默认消息
        String nullResultMessage = Optional.ofNullable(nullMessage)
                .orElse("No error message available");

        // 打印结果
        System.out.println("Result Message: " + resultMessage);
        System.out.println("Null Result Message: " + nullResultMessage);
    }
}
```

