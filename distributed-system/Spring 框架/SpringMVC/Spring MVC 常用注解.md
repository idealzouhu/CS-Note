# 一、Spring MVC 常用注解

## 1.1 控制器

### 1.1.1 @Controller

在Spring MVC中，`@Controller` 注解用于标识一个类作为控制器，用于处理Web请求。当将 `@Controller` 注解应用于类上时，Spring容器会自动将这个类识别为一个Bean，并将其实例化和管理。

同时，**`@Controller` 注解配合 `@RequestMapping` 注解使用**，`@RequestMapping` 用于将特定的HTTP请求映射到控制器的处理方法上。当 DispatcherServlet 在处理一个 HTTP 请求时，它会扫描应用程序中的所有`@Controller` 注解的类，然后根据 `@RequestMapping` 注解中定义的 URL 路径来确定调用哪个方法来处理请求。



### 1.1.2 @RestController

`@RestController` 是 Spring 框架中的一个注解，它是 `@Controller `注解和 `@ResponseBody` 注解的组合。它的作用是**将一个类标识为 RESTful 风格的控制器**，用于处理 HTTP 请求并返回数据，通常用于构建 RESTful API。

与`@Controller`注解不同的是，`@RestController` 注解会<font color="red">**自动将方法的返回值转换为 JSON 或 XML 格式的响应数据**</font>，并返回给客户端，而不是返回一个视图。具体区别查看 `@ResponseBody` 注解





## 1.2 HTTP 请求控制处理

### 1.2.1 @RequestMapping

@RequestMapping 注解是用来**映射 Web 请求** (访问路径和参数)、处理类和方法的。

@RequestMapping 可注解在类或方法上。注解在方法上的 @RequestMapping 路径会继承注解在类上的路径。

@RequestMapping 支持 Servlet 的request 和 response 作为参数, 也支持对request和 response 的媒体类型进行配置。



### 1.2.2 @ResponseBody 

@ResponseBody 支持**将返回值放在 response 体内**，而不是返回一个页面。

此注解可放置在返回值前或者方法上。



### 1.2.3 @RequestBody

通常，HTTP 请求中的参数会作为URL的一部分（例如，在GET请求中）或者作为表单数据（例如，在POST请求中）发送。但是有些情况下，特别是当需要发送复杂的数据结构（例如 JSON 或 XML 格式的数据）时，将参数放在请求体中更为方便。

@RequestBody 是 Spring 框架中用于接收 **HTTP 请求体**中的参数的注解，是构建RESTful API时常用的注解之一。

**@RequestBody 放置在参数前**。当在 Spring MVC 的控制器方法的参数上使用 @RequestBody 注解时，Spring 会自动将请求体中的数据反序列化成相应的 Java 对象，然后将其作为方法参数传递给控制器方法。下面是一个简单的示例：

```
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MyRestController {

    @PostMapping("/create")
    public String create(@RequestBody CreateUserRequest request) {
        // 处理请求体中的数据
        return "User created: " + request.getUsername();
    }
}

class CreateUserRequest {
    private String username;
    private String password;
    
    // Getter and Setter
}

```

在这个示例中，`create()`方法使用了`@PostMapping`注解，表示处理POST请求。方法的参数`request`前面使用了`@RequestBody`注解，表示接收请求体中的数据并将其反序列化成`CreateUserRequest`对象。然后，可以在方法中直接使用`request`对象获取请求体中的数据，完成相应的业务逻辑处理。





### 1.2.4  @PathVariable

`@PathVariable `是 Spring 框架中用于处理 URL 路径中的参数的注解。通常，HTTP 请求中的参数会作为URL的一部分发送，例如 `/users/{id}`，其中 `{id} `就是一个路径参数。`@PathVariable` 注解就是用来获取这些路径参数的值。

下面是一个简单的示例：

```
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MyRestController {

    @GetMapping("/users/{id}")
    public String getUserById(@PathVariable Long id) {
        // 根据路径参数id查询用户信息
        return "User ID: " + id;
    }
}
```

在这个示例中，`getUserById()`方法使用了`@GetMapping`注解，表示处理GET请求。方法的参数`id`前面使用了`@PathVariable`注解，表示获取路径中的参数值，并将其注入到方法的参数`id`中。在实际请求时，例如访问`/users/123`，Spring MVC会将路径中的`123`注入到`id`参数中，然后可以在方法中直接使用`id`参数进行业务逻辑的处理。





`@PathVariable`注解还支持**将路径参数赋予特定的名称**，例如：

```
@GetMapping("/users/{userId}")
public String getUserById(@PathVariable("userId") Long id) {
    // 根据路径参数userId查询用户信息
    return "User ID: " + id;
}
```

在这个示例中，`@PathVariable("userId")`表示将路径中的参数`userId`赋予给方法参数`id`，而不是默认的参数名。



### 1.2.5  `@RequestParam` 

请求参数（Request Parameters）是客户端向服务器发送请求时传递的数据，通常包含在 URL 查询字符串或请求体中。在 HTTP 请求中，参数以键值对的形式发送，例如 `?key1=value1&key2=value2`。

在 Spring MVC 中，可以使用 `@RequestParam` 注解来获取请求参数的值，如下所示：

```
javaCopy code@GetMapping("/example")
public String exampleHandler(@RequestParam String key) {
    // 使用 @RequestParam 获取请求参数值
    System.out.println("Value of key: " + key);
    return "examplePage";
}
```

在这个例子中，`@RequestParam` 注解用于获取名为 "key" 的请求参数的值。



# 二、前后端数据交互注解