[TOC]

## 一、数组定义

### 1.1 全局作用域中定义数组

在全局作用域中定义数组大小，只能使用预处理器宏`#define`。

`const`变量在代码中表现得像常量，但它们**实际上并不是编译时常量**。在全局作用域中，不能使用`const`变量来定义数组大小。编译器要求在编译时就知道数组的大小，而`const`变量在编译时可能没有固定的值。

```c
#include <stdio.h>

#define NUM_ADJACENT_FRAMES 5
int ADJACENT_FRAMES[2 * NUM_ADJACENT_FRAMES + 1];

int main() {
     // 示例初始化
    for (int i = 0; i < 2 * NUM_ADJACENT_FRAMES + 1; i++) {
        ADJACENT_FRAMES[i] = i; // 示例初始化
    }
    
    // 打印数组内容
    for (int i = 0; i < 2 * NUM_ADJACENT_FRAMES + 1; i++) {
        printf("%d ", ADJACENT_FRAMES[i]);
    }
    printf("\n");
    return 0;
}
```



### 1.2 局部作用域定义数组

在局部作用域中定义数组大小，可以使用预处理器宏`#define` 或者 `const`

```c
#include <stdio.h>

// #define NUM_ADJACENT_FRAMES 5   
const int NUM_ADJACENT_FRAMES = 5;

void process_frames() {
    int ADJACENT_FRAMES[2 * NUM_ADJACENT_FRAMES + 1];
    
    // 示例初始化
    for (int i = 0; i < 2 * NUM_ADJACENT_FRAMES + 1; i++) {
        ADJACENT_FRAMES[i] = i;
    }

    // 打印数组内容
    for (int i = 0; i < 2 * NUM_ADJACENT_FRAMES + 1; i++) {
        printf("%d ", ADJACENT_FRAMES[i]);
    }
    printf("\n");
}

int main() {
    process_frames();
    return 0;
}
```



## 二、细节补充

### 2.1  `#define` 和 `const` 的区别

| 特性             | `#define`                            | `const`                                    |
| ---------------- | ------------------------------------ | ------------------------------------------ |
| **类型**         | 无类型，仅文本替换                   | 有类型，进行类型检查                       |
| **作用域**       | 从定义点到文件结束，或使用 `#undef`  | 根据定义位置，全局或局部                   |
| **生命周期**     | 编译时定义，在整个编译过程中有效     | 程序运行时有效，根据定义位置决定           |
| **初始化**       | 不进行初始化，只做文本替换           | 需要显式初始化                             |
| **内存分配**     | 无内存分配，仅文本替换               | 有内存分配，根据定义位置决定（栈或全局）   |
| **编译时常量**   | 是，可以用于定义全局数组大小         | 不是，在全局作用域不能用于定义数组大小     |
| **优点**         | 简单、直接、灵活                     | 类型安全，易于调试和优化                   |
| **缺点**         | 没有类型检查，容易引起难以发现的错误 | 不能在全局作用域定义数组大小               |
| **示例定义**     | `#define SIZE 10`                    | `const int SIZE = 10;`                     |
| **全局数组定义** | `int array[SIZE];`                   | 无法在全局作用域定义数组大小               |
| **局部数组定义** | `int array[SIZE];`                   | `int array[SIZE];`                         |
| **适用场景**     | 简单常量定义，宏函数，编译时常量使用 | 类型检查，需要类型信息的常量，局部数组定义 |





### 2.2 全局数组和局部数组的区别

（1）**局部数组的内存分配**：

- 局部变量（包括局部数组）在**函数调用时分配内存**。它们通常分配在栈上。
- 由于栈内存是在运行时管理的，因此局部数组的大小可以基于运行时的值（如函数参数或局部计算结果）。

（2）**全局数组的内存分配**：

- 全局变量在**程序启动时分配内存**，它们分配在数据段（.data 或 .bss 段）。
- 由于数据段的大小必须在编译时确定，因此全局数组的大小必须是编译时常量。



### 2.3 编译时常量 vs 运行时常量

**编译时常量**：值在编译时已知，并且编译器可以在编译过程中使用这些值进行优化和内存分配。

**运行时常量**：值在运行时确定，并且在程序运行过程中使用这些值进行内存分配和操作。



### 2.4 为什么局部作用域不要求编译时常量

在C语言中，局部作用域定义的数组不要求编译时常量，这主要是因为**局部变量在函数调用时分配内存，而不是在编译时分配**。这允许更灵活的内存管理和运行时行为。





## 总结

**`#define` 宏定义**：用于编译时常量替换，可以在全局范围内定义数组大小。

**`const` 变量**：在编译时不能作为编译时常量，不能在全局范围内定义数组大小，只能在局部作用域（如函数内部）使用。