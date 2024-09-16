### Java 对象概述

在 Java 世界里一切皆 Object 对象。

从虚拟机的角度来看， Java 有两种对象：

- **Object 对象**： 描述类的元数据，存放在方法区里面。
- **Class 对象**：类的具体实例，存放在堆里面。

`Class` 对象和 `Object` 对象在 Java 中是两个不同的概念,  但<font color="red">**类的每个实例（`Object` 对象）在 JVM 内部都会关联到一个唯一的 `Class` 对象，描述这个类的结构和行为（比如方法、字段、构造函数等）**</font>。



### 什么是 Class 对象

Class 对象表示每个类运行时的类型信息，包含与类名称、继承关系、字段、方法有关的信息。同时，`Class`  对象是  Java 反射机制的基础，它允许程序在运行时动态地获取类的元数据。

在类加载过程中，JVM 将一个类加载入自己的方法区内存时，会为其创建一个 Class 对象。对于一个类来说，其 **Class 对象是唯一的**。 

Class 类没有公共的构造方法，Class 对象是在类加载的时候由 Java 虚拟机调用类加载器中的 defineClass 方法自动构造的，因此**不能显式地声明一个 Class 对象**。 

所有的类都是在第一次使用时被动态加载到JVM中（**懒加载**），其各个类都是在必需时才加载的。这一点与许多传统语言（如 C++ ）都不同，JVM为动态加载机制配套了一个判定动态加载使 能的行为，使得类加载器首先检查这个类的 Class 对象是否已经被加载。如果尚未加载，类加载器 会根据类的全限定名查找 `.class` 文件，验证后加载到JVM的方法区内存，并构造其对应的 Class 对象。



### 什么是 Object 对象

`Object` 类是 Java 中的根类，所有类都直接或间接地继承自 `java.lang.Object`。这意味着 Java 中的每个对象（无论是用户定义的类、Java 标准库的类等）都可以被视为 `Object` 对象。

在创建对象时，每个 `Object` 实例在运行时都通过 JVM 绑定到一个唯一的 `Class` 对象。



### 为什么 Class 类继承 Object 类 

在 Java 中，所有的类（无论是用户定义的类还是标准库中的类）都继承自 `Object`，这确保了所有类都具有基本的对象行为。**`Class` 本身作为一个对象，也继承了 `Object` 的通用行为**。

```java
Class<?> clazz = MyClass.class;

// 调用 Object 类的方法
System.out.println(clazz.toString());      // 输出 Class 对象的字符串表示
System.out.println(clazz.hashCode());      // 获取 Class 对象的哈希码
System.out.println(clazz.equals(MyClass.class));  // 比较 Class 对象是否相等
```





### oop-class 模型

HotSpot JVM 并没有将 Java 实例对象直接一对一的映射到本地（native）的 C++ 对象，而是设计了一个 oop-klass 模型。

每当在Java代码中创建一个对象时，JVM会创建一个 instanceOopDesc 实例来表示这个对象，此 对象实例存放在堆区。类似地，每当在 Java 代码中创建一个数组时，JVM 会创建一个 arrayOopDesc 实例来表示。所以，一个普通 Java 对象的底层为一个 instanceOopDesc 实例。









### 参考资料

《 极致经典（卷2）：Java高并发核心编程(卷2 加强版) -特供v21-release》