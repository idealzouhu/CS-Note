# 一、反射概述

## 1.1 何谓反射

Java 反射是 Java 编程语言的一个重要特性，它**允许程序在运行时检查和操作 Java 类的成员变量、方法、构造函数等信息，而无需事先知道这些类的类型。**





# 二、Java 反射实现

## 2.1 常用的 Java 类

在 Java 里面，Java 反射提供了一组 API，使得程序可以在运行时动态地获取类的结构信息，并且可以在需要时操作这些类的成员。提供的工具主要有：

- `Class` ：用于表示 Java 类的类对象。在 Java 中，每个类都有一个对应的 `Class` 对象，该对象包含了类的结构信息，例如类的方法、字段、构造函数等。

- `Method` ： 用于表示类的方法。`Method` 类提供了许多方法，可以用于获取方法的各种信息，以及在运行时调用方法。
-  `Field` ： 用于表示类的字段（成员变量）。`Field` 类提供了许多方法，可以用于获取字段的各种信息，以及在运行时操作字段。

（1）

`Class` 类提供了一系列的方法，可以用于获取和操作类的结构信息。一些常用的 `Class` 类的方法包括：

- `getName()`：获取类的名称。
- `getMethods()`：获取类的所有公共方法。
- `getDeclaredMethods()`：获取类声明的所有方法，包括私有方法。
- `getFields()`：获取类的所有公共字段。
- `getDeclaredFields()`：获取类声明的所有字段，包括私有字段。
- `getConstructors()`：获取类的所有公共构造函数。
- `getDeclaredConstructors()`：获取类声明的所有构造函数，包括私有构造函数。

（）

一些常用的 `Method` 类的方法包括：

- `getName()`：获取方法的名称。

- `getReturnType()`：获取方法的返回类型。

- `getParameterTypes()`：获取方法的参数类型。

- `getModifiers()`：获取方法的修饰符。

- <font color="red">**`invoke(Object obj, Object... args)`**</font>：调用方法。第一个参数是方法所属的对象，如果是静态方法则可以传入 null；第二个参数是方法的参数列表。

  

（3）

一些常用的 `Field` 类的方法包括：

- `getName()`：获取字段的名称。
- `getType()`：获取字段的类型。
- `getModifiers()`：获取字段的修饰符。
- `get(Object obj)`：获取指定对象上此字段的值。
- `set(Object obj, Object value)`：设置指定对象上此字段的值。





## 2.2 Class 对象的获取方式

[Java 反射机制详解 | JavaGuide](https://javaguide.cn/java/basis/reflection.html#反射实战)





# 参考资料

[Java 反射机制详解 | JavaGuide](https://javaguide.cn/java/basis/reflection.html)