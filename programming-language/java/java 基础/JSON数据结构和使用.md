# 一、JSON 概述

## 1.1 什么是 JSON

JSON（JavaScript Object Notation）是一种轻量级的数据交互格式。它去除了所有 JavaScript 执行代码，只保留 JavaScript 的对象格式。

它基于 ECMAScript (欧洲计算机协会制定的 js 规范)的一个子集，采用完全独立于编程语言的文本格式来存储和表示数据。



## 1.2  JSON 数据类型和语法

JSON适合表示层次结构，JSON采用键值对的方式来组织数据，仅支持以下几种数据类型：

- 键值对：`{"key": value}`
- 数组：`[1, 2, 3]`
- 字符串：`"abc"`
- 数值（整数和浮点数）：`12.34`
- 布尔值：`true`或`false`
- 空值：`null`

JSON的语法主要包含以下几点

- JSON数据以键值对形式存在，键值对的键和值之间用冒号 `:` 连接。不同键值对之间用逗号 `,` 隔开，
- JSON只允许使用双引号包裹 key， 特殊字符用 `\` 转义，

- `{}` 用于表示<font color="red"> **JSON对象**</font>，也称为键值对（key-value pairs）的集合，对应 js 中的 object 。
-  `[]` 用于表示 JSON数组，也称为值的有序列表，对应 js 中的 array。数组中的元素可以是字符串、数字、布尔值、对象、数组或者 `null`。

JSON格式的例子：

```json
{
  "name": "John Doe",
  "age": 30,
  "city": "New York",
  "isStudent": false,
  "hobbies": ["reading", "traveling", "coding"],
  "address": {
    "street": "123 Main Street",
    "zipCode": "10001"
  }
}
```





## 1.3 JSON 优势

JSON（JavaScript Object Notation）作为一种数据交换格式，具有多个优点，使其在各种应用中得到广泛使用：

- **轻量级：** JSON 是一种轻量级的数据交换格式，相对于其他格式如 XML，它的数据量较小。这有助于减少网络传输的数据量，提高传输效率。

- **易于解析和生成：** JSON 数据易于在不同的编程语言中解析和生成。几乎所有的编程语言都提供了 JSON 解析器和生成器，使得在应用程序中处理 JSON 数据变得方便。

  js中，json与对象的转换：`JSON.parse()`/`JSON.stringify()`;
  php中，json与数组的转换：`json_encode()`/`json_decode()`;
  java中，json与对象的转换：`fromJson()`/`toJson()`;
  python中，json与对象的转换：`json.dumps()`/`json.loads()`;

- **与多种平台和语言兼容：** JSON 不依赖于任何特定的编程语言，可以在不同平台和各种编程语言之间进行数据交换。这使得 JSON 成为跨平台和跨语言的数据传输格式。

- **浏览器原生支持：** 在前端开发中，JSON 在 JavaScript 中具有原生支持，可以直接在浏览器中解析和生成 JSON 数据，使得前后端数据交互更加方便。

  ```javascript
  // JSON string to JavaScript object:
  jsObj = JSON.parse(jsonStr);
  
  // JavaScript object to JSON string:
  jsonStr = JSON.stringify(jsObj);
  ```

  



# 二、使用 JSON

## 2.1 在 Java 程序中使用 JSON

在 Java 中，我们通常使用一些库来处理 JSON 数据，实现 JavaBean和 JSON 之间的转换。这些库主要有：

- Jackson
- Gson
- Fastjson

在 JavaBean和 JSON 之间的转换过程里，主要有两个概念：

- <font color="blue">**序列化**</font>：     将 JavaBean变为 JSON 对象
- <font color="blue">**反序列化**</font>：  将 JSON对象 解析为 JavaBean

具体地说，反序列化的过程包括以下步骤：

1. **获取JSON数据：** 从外部来源（例如网络、文件或其他应用程序）获取包含JSON数据的字符串。
2. **反序列化操作：** 使用相应的JSON解析库或框架，将JSON数据解析为 Java 对象。在Java中，常见的JSON解析库包括Jackson、Gson、org.JSON等。
3. **生成Java对象：** 解析后，将 JSON 数据映射到 Java对象的属性上，从而创建一个对应的 Java 对象。


在 Jackson 库中，<font color="red">**将 JSON 数据映射到 Java 对象的属性是通过 Java 对象的 Getter 和 Setter 方法以及字段的匹配来完成的**</font>。以下是映射的基本规则：

1. **属性名匹配：** JSON中的字段名需要与 Java对象的属性名一致。例如，如果JSON中有一个字段名为 `"id"`，则对应的Java对象需要有一个名为 `id` 的属性，并且Jackson库会通过调用 `getId()` 和 `setId()` 方法来映射该属性。
2. **驼峰命名规则：** 如果JSON中的字段名采用驼峰命名（例如 `"myField"`），Jackson库会自动匹配到Java对象的驼峰命名风格的属性（例如 `getMyField()` 和 `setMyField()` 方法）。
3. **类型匹配：** Jackson会尽量将JSON中的值转换为Java对象相应属性的类型。例如，如果JSON中的 `"age"` 字段是一个整数，Jackson会尝试将其转换为Java对象的 `int` 类型属性。
4. **嵌套对象：** 如果JSON中包含嵌套的对象，Jackson会递归地将这些嵌套对象映射到Java对象的嵌套属性中。
5. **默认构造函数：** Java对象需要具有默认构造函数（无参数的构造函数）。这是因为Jackson在映射时会创建Java对象的实例，然后通过Setter方法设置属性的值。



## 2.2 Jackson 的使用

### 2.2.1 Jackson 的简单介绍

Jackson 是一个用于 Java 平台的 JSON 处理库，主要用于 JSON 数据的序列化（将 Java 对象转换为 JSON 字符串）和反序列化（将 JSON 字符串转换为 Java 对象）。 Jackson是一个广泛使用且功能强大的库， 可以与 Spring 框架无缝集成，**是 Spring MVC 默认的 JSON 处理库**。

`ObjectMapper` 类是Jackson库的核心类，提供两种方法：

- `readValue` 方法被用于将JSON字符串解析为 JavaBean 对象，可以接受不同类型的输入，包括 JSON 字符串、JSON 输入流等。例如

  ```java
  ObjectMapper objectMapper = new ObjectMapper();
  String jsonString = "{ \"id\": 1, \"name\": \"John\" }";
  User user = objectMapper.readValue(jsonString, User.class);
  ```

- `writeValueAsString` 方法用于将 Java 对象序列化为 JSON 数据字符串。该方法接受一个 Java 对象，并返回其对应的 JSON 字符串。例如：

  ```java
  ObjectMapper objectMapper = new ObjectMapper();
  User user = new User(1, "John");
  String jsonString = objectMapper.writeValueAsString(user);
  ```

另外，我们还可以可以自定义`JsonSerializer`和`JsonDeserializer`来定制序列化和反序列化





### 2.2.2 反序列化

假设有一个JSON字符串表示一个用户信息，如下所示：

```json
{
  "id": 1,
  "username": "john_doe",
  "email": "john@example.com",
  "age": 30,
  "active": true
}
```

存在  `User` 类，如下所示：

```java
@Data
public class User {
    private long id;
    private String username;
    private String email;
    private int age;
    private boolean active;

    // Default constructor (required for Jackson)
    public User() {}

    // Parameterized constructor
    public User(long id, String username, String email, int age, boolean active) {
        this.id = id;
        this.username = username;
        this.email = email;
        this.age = age;
        this.active = active;
    }

```

我们将使用 Jackson 库进行 JSON 解析。首先，确保已经将 Jackson 库添加到项目的依赖中。接下来，使用以下代码将 JSON 字符串解析为Java对象：

```java
import com.fasterxml.jackson.databind.ObjectMapper;

public class JsonExample {
    public static void main(String[] args) {
        // JSON字符串
        String jsonString = "{ \"id\": 1, \"username\": \"john_doe\", \"email\": \"john@example.com\", \"age\": 30, \"active\": true }";

        // 创建ObjectMapper对象
        ObjectMapper objectMapper = new ObjectMapper();

        try {
            // 将JSON字符串解析为User对象
            User user = objectMapper.readValue(jsonString, User.class);

            // 使用解析得到的User对象
            System.out.println("ID: " + user.getId());
            System.out.println("Username: " + user.getUsername());
            System.out.println("Email: " + user.getEmail());
            System.out.println("Age: " + user.getAge());
            System.out.println("Active: " + user.isActive());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

在这个例子中，`User` 类是一个简单的Java类，用于表示用户信息。`ObjectMapper` 类是Jackson库的核心类，它提供了方法来解析JSON字符串并将其转换为Java对象。这个例子中，`readValue` 方法被用于将JSON字符串解析为`User`对象。

请注意，<font color="red">`User`类应该具有与JSON属性相对应的Setter方法，以便`ObjectMapper`能够Java对象的实例，然后通过Setter方法设置属性的值</font>。



### 2.2.3 序列化

```java
import com.fasterxml.jackson.databind.ObjectMapper;

public class JsonSerializationExample {
    public static void main(String[] args) {
        // 创建一个User对象
        User user = new User(1, "john_doe", "john@example.com", 30, true);

        // 创建ObjectMapper对象
        ObjectMapper objectMapper = new ObjectMapper();

        try {
            // 将User对象转换为JSON字符串
            String jsonString = objectMapper.writeValueAsString(user);

            // 打印JSON字符串
            System.out.println("JSON String: " + jsonString);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

在这个例子中，我们创建了一个`User`对象，然后使用`ObjectMapper`的`writeValueAsString`方法将其转换为JSON字符串。`writeValueAsString`方法将Java对象转换为JSON字符串，这样我们就可以在需要时将其传递给其他系统、写入文件或发送到网络。

请注意，<font color="red">`User`类应该具有与JSON属性相对应的Getter方法，以便`ObjectMapper`能够访问并获取Java对象的属性值</font>。





# 参考资料

[GitHub - FasterXML/jackson: Main Portal page for the Jackson project](https://github.com/FasterXML/jackson?tab=readme-ov-file)

[Jackson Tutorial | Baeldung（推荐）](https://www.baeldung.com/jackson)

[使用JSON - 廖雪峰的官方网站 (liaoxuefeng.com)](https://www.liaoxuefeng.com/wiki/1252599548343744/1320418650619938)

[什么是JSON - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/138511000)