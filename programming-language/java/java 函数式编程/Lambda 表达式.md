## Lambda 表达式概述

### 什么是 Lambda 表达式

Lambda 表达式(也称为闭包)是一种匿名函数，它允许在代码中直接定义简短的函数。它可以被赋值给函数式接口类型的变量。

Lambda 表达式是一种轻量级的语法，用于简化函数式接口的实现。

> Lambda 表达式，也可称为闭包，它是推动 Java 8 发布的最重要新特性。





### 为什么引入 Lambda 表达式

到目前为止，在Java中传递一个代码段并不容易，你不能直接传递代码段。Java是一种面向对象语言，所以必须构造一个对象，这个对象的类需要有一个方法包含所需的代码。

```java
// 接口定义
interface Greeting {
    void greet(String message);
}

public class WithoutLambdaExample {
    public static void main(String[] args) {
    
        // 例子1： 使用匿名内部类实现接口
        Greeting greeting = new Greeting() {
            @Override
            public void greet(String message) {
                System.out.println("Hello, " + message + "!");
            }
        };
        
         // 例子2： 使用 Lambda 表达式实现接口
        Greeting greeting = message -> System.out.println("Hello, " + message + "!");

        // 调用接口方法
        greeting.greet("World");
    }
}
```

在这两个例子中，接口 `Greeting` 包含一个抽象方法 `greet`，它接受一个字符串参数并输出一条问候语。在第一个例子中，我们使用传统的匿名内部类来实现这个接口。在第二个例子中，我们使用 Lambda 表达式来实现相同的接口。

使用 Lambda 表达式的主要优势在于它能够**简化代码**，特别是在需要实现的接口只包含一个抽象方法(即函数式接口)的情况下。Lambda 表达式的语法更为紧凑，更易读，使得代码更为简洁。





### Lambda表达式语法

lambda 表达式的语法格式如下：

```
(parameters) -> expression        // 简单形式，表达式值作为Lambda表达式的返回值
(parameters) ->{ statements; }    // 复杂形式，代码块可以包含任意数量的语句，并且可以有return语句
```

在使用Lambda表达式时，编译器可以根据上下文推断出参数的类型，所以在大多数情况下，你无需显式指定参数的类型。

Lambda表达式通常用于函数式接口，即只包含一个抽象方法的接口。



### 变量作用域

Lambda表达式的变量作用域主要涉及两个方面: **作用域**和**变量捕获**：

- Lambda 表达式的作用域与匿名内部类类似，可以访问外部作用域的变量。但存在一些限制，这就引入了变量捕获的概念

- lambda 表达式可以捕获外围作用域中变量的值。这里有一条规则: lambda表达式中捕获的变量必须实际上是**事实最终变量**( effectively final)。事实最终变量是指，这个变量初始化之后就不会再为它赋新值。

[Capturing Local Values - Dev.java](https://dev.java/learn/lambdas/first-lambdas/#local-variables)





### 处理lambda表达式

现在，我们需要了解 如何编写方法处理lambda表达式。

使用lambda表达式的重点是 **延迟执行**（ deferred execution)。



## Lambda 表达式的应用

### lambda 表达式的实现

编写Lambda表达式可以分解为以下三个步骤：

1. **识别Lambda表达式的类型**：**Lambda表达式的类型必须是函数式接口。**在Java中，每个表达式和变量都有一个类型，并且这个类型在编译时是已知的。Lambda表达式的类型通常由上下文推断出来，具体取决于它赋值给的变量或参数的类型。

1. **找到正确的抽象方法**：Lambda表达式是该函数式接口中唯一抽象方法的实现。
2. **实现该方法**：编写Lambda表达式实现该抽象方法的逻辑。

例如：

```java
Predicate<String> isEmpty = s -> s.isEmpty();
isEmpty.test("hello");
```

在这个例子中，Lambda表达式`s -> s.isEmpty()`的类型是`Predicate<String>`，因为它被赋值给了一个`Predicate<String>`类型的变量。

具体细节查看 [Writing Your First Lambda Expression - Dev.java](https://dev.java/learn/lambdas/first-lambdas/#indentifying-the-type)





### 参考资料

[Writing Your First Lambda Expression - Dev.java](https://dev.java/learn/lambdas/first-lambdas/#finding-the-method-to-implement)