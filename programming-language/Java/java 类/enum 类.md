### 枚举类

`Enum` 类的特性：

- 类型安全

- 枚举常量
- 内置方法



### 构造函数和字段

枚举类型的常量一般采用大写字母命名，并且可以在枚举类中定义字段、方法和构造器。

**每个枚举常量都调用了枚举类的构造函数，传递了相应的参数**。





### 使用案例

```java
public enum DayEnum {
    MONDAY("Work day"),
    TUESDAY("Work day"),
    WEDNESDAY("Work day"),
    THURSDAY("Work day"),
    FRIDAY("Work day"),
    SATURDAY("Weekend"),
    SUNDAY("Weekend");

    private final String description;

    // 构造器
    DayEnum(String description) {
        this.description = description;
    }

    // 获取描述
    public String getDescription() {
        return description;
    }

    public static void main(String[] args) {
        // 使用枚举值
        DayEnum day = DayEnum.MONDAY;
        System.out.println("Today is " + day.name() + ", which is a " + day.getDescription()); // 输出 Today is MONDAY, which is a Work day
    }
}
```

