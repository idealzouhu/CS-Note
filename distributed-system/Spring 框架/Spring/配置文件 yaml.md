



# 配置信息读取

## @Value

`@Value` 注解用于从配置文件中**读取单个属性的值**，并将其注入到 Spring Bean 中的对应属性中。

例如，假设配置文件中有以下属性：

```
propertiesCopy code
library.name=My Library
```

在 Spring Bean 中可以使用 `@Value` 注解来注入 `library.name` 属性的值：

```java
@Component
public class MyComponent {

    @Value("${library.name}")
    private String libraryName;

    // Getter and setter
}
```

这样，在 `MyComponent` 类的 `libraryName` 属性中就能得到 "My Library" 这个值。



## @ConfigurationProperties

`@ConfigurationProperties` 注解用于**将配置文件中的属性映射到一个 JavaBean 类中的属性**上。其中 `prefix` 参数指定了配置属性的前缀，即在配置文件中以 `library` 为前缀的属性将会映射到被注解的 JavaBean 类的属性上。

例如，假设配置文件中有以下属性：

```
propertiesCopy codelibrary.name=My Library
library.location=123 Main St
```

而在 JavaBean 类中使用了 `@ConfigurationProperties(prefix = "library")` 注解，则可以这样定义该类：

```
javaCopy code@ConfigurationProperties(prefix = "library")
public class LibraryProperties {
    private String name;
    private String location;

    // Getters and setters
}
```

Spring Boot 将自动将配置文件中以 `library` 前缀开头的属性值注入到 `LibraryProperties` 类的对应属性中，如此一来，`name` 属性的值将为 "My Library"，`location` 属性的值将为 "123 Main St"。