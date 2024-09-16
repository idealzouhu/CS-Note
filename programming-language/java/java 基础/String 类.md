## String 类

`String` 类是不可变的（immutable），这意味着一旦创建了一个 `String` 对象，它的内容就不能被改变。由于不可变这一特性，`String` 类将会具备以下特点：

- 线程安全。
- 开销较大。在拼接字符串的时候会产生无用的中间对象，降低性能。



### 对象不可变的底层实现原理

（1）**字段声明为 `final`**

保存字符串的数组 `value` 被  `final`  修饰且为私有的，这意味着它在对象创建后不能被重新分配。

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence,
               Constable, ConstantDesc {
	@Stable
    private final byte[] value;
}
```

（2）**没有修改方法**

`String` 类没有提供/暴露直接修改这个字符串的方法。例如，没有 `set` 方法可以改变 `String` 的内容。

（3）**构造方法创建新实例**

任何对 `String` 内容的更改操作（如连接或替换）都会创建新的 `String` 实例，而不是直接修改现有实例。例如，使用 `concat` 方法会返回一个新的 `String` 对象，而原始 `String` 对象保持不变。

```java
public class StringExample {
    public static void main(String[] args) {
        String s1 = "Hello";
        String s2 = s1.concat(" World");

        System.out.println(s1 == s2);  // false
    }
}
```





### 支持字符串池

Java 的字符串池（String Pool）机制利用了 `String` 的不可变性。字符串池是一个保存已经创建的 `String` 对象的缓存区域，当需要创建一个新的 `String` 对象时，Java 会首先检查池中是否已经存在相同内容的 `String` 对象。如果存在，新的 `String` 对象将引用池中的现有对象，而不是创建新的实例。

```java
public class StringExample {
    public static void main(String[] args) {
        // String 类支持字符串池
        String a = "Hello";
        String b = "Hello";
        System.out.println(a == b);  // true
    }
}
```





## StringBuild 类

StringBuild 类允许在字符串操作过程中对字符串进行修改，从而提高了性能，尤其是在需要频繁修改字符串内容的场景中。但是，StringBuild 类存在线程安全问题。



### 对象可变

StringBuild 类可以修改字符串内容，不会创建新的对象，提升性能。

性能对比开案如下：

```java
public class StringBuiderExample {
    public static void main(String[] args){
        long startTime, endTime;

        // 使用String进行多次拼接
        startTime = System.currentTimeMillis();
        String str = "";
        for (int i = 0; i < 10000; i++) {
            str += "a";
        }
        endTime = System.currentTimeMillis();
        System.out.println("使用String拼接耗时: " + (endTime - startTime) + "ms");

        // 使用StringBuilder进行多次拼接
        startTime = System.currentTimeMillis();
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 10000; i++) {
            sb.append("a");
        }
        endTime = System.currentTimeMillis();
        System.out.println("使用StringBuilder拼接耗时: " + (endTime - startTime) + "ms");
    }
}
```

输出结果为：

```java
使用String拼接耗时: 37ms
使用StringBuilder拼接耗时: 2ms
```





## StringBuffer 类

StringBuffer 类在支持对象可变的基础上，还保证了线程安全，但性能不如 StringBuild 类。



### 线程安全

StringBuffer 实现线程安全的方式是在许多关键方法（如 append, insert, delete 等）上**使用了 synchronized 关键字**。这意味着在多线程环境中，当一个线程正在执行这些方法时，其他线程必须等待该方法执行完毕才能继续执行。

```java
  @Override
    public synchronized StringBuffer append(Object obj) {
        toStringCache = null;
        super.append(String.valueOf(obj));
        return this;
    }
```



## 总结

这三种类的使用场景主要为：

- 操作少量数据使用 String
- 单线程下操作大量数据使用 StringBuilder
- 单线程下操作大量数据使用 StringBuffer