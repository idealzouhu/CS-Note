[TOC]

# 一、 介绍

在Android应用开发中，崩溃问题是常见的挑战之一。在Android应用崩溃时，利用backtrace的地址信息找到so出错的行数通常需要进行符号解析（symbol resolution）。我们可以借助工具进行符号解析和反汇编，比如：

- addr2line
- IDA Pro

 

# 二、崩溃日志分析

假设我们有以下崩溃日志：

```
textCopy codeI/DEBUG(349): signal 11 (SIGSEGV), code 2 (SEGV_ACCERR), fault addr 0xb170a219
...
I/DEBUG(349): backtrace:
I/DEBUG(349):     #00 pc 0011d680  /system/lib/gps_ma87.so (Method0+984)
I/DEBUG(349):     #01 pc 0007694c  /system/lib/gps_ma87.so (Method1+724)
...
```

我们关注 `Method0` 的崩溃位置。



## 2.1 使用 addr2line 进行符号解析

首先，我们使用 `addr2line` 解析地址 `0x0011d680` 对应的源代码位置：

```
addr2line -f -e /system/lib/gps_ma87.so 0x0011d680
```

`addr2line`将输出对应的源文件和行号，例如：

```
Method0
/path/to/source/file.cpp:123
```

这表明 `Method0` 在文件 `file.cpp` 中的第 123 行发生了崩溃



需要注意的是，addr2line可能得到行号为 ??:? 或 ??:0 的这两种情况，通常原因就是编译得到的so文件没有附加上符号表(symbolic)信息。在这种情况下，我们可以使用 IDA Pro 软件来定位。



## 2.2 使用IDA Pro进行深入分析

同时，我们还可以使用IDA工具来分析崩溃地址所对应的代码

1. **打开二进制文件：**启动IDA Pro，并打开包含崩溃地址的二进制文件。你可以选择“File” > “Open”来加载文件。
2. **导航到崩溃地址：**在IDA Pro中，你可以使用“View” > “Open Subviews” > “Functions”来查看函数列表。然后，在函数列表中找到并双击崩溃地址所在的函数。
3. **查看反汇编代码：**一旦你导航到了崩溃地址所在的函数，IDA Pro将显示该函数的反汇编代码。你可以使用这些反汇编代码来了解在崩溃时程序执行的指令。
4. **查看伪代码：**IDA Pro还提供了反汇编代码的伪代码视图。右键点击反汇编代码视图，然后选择“Jump to Pseudocode”以查看相应的伪代码（也可以按 F5 键）。伪代码更容易阅读，有助于理解代码逻辑。

> [IDA pro 7.0 下载地址](https://pan.baidu.com/s/1qYKDyCc)



# 三、 结论

通过结合 `addr2line `和 IDA Pro 的功能，我们成功地定位了 SO 文件中  `Method0`  的崩溃位置，并能够更深入地分析代码。这个过程帮助我们识别出错的函数和可能的原因，为修复崩溃问题提供了有力支持。

这篇博客演示了在Android应用崩溃分析中使用 `addr2line` 和 IDA Pro 的基本步骤。通过结合符号解析和反汇编，我们能够精确定位问题，并在实际修复中更有针对性。





# 参考资料

[利用backtrace的地址信息和addr2line命令定位so出错的行数_gdb查看so报错行数-CSDN博客](https://blog.csdn.net/u012587637/article/details/104297995)