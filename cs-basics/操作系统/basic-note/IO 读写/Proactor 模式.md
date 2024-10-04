### 什么是 Proactor 模式

Proactor 模式是一种异步事件处理模式，通常用于将 I/O 操作的执行和事件处理解耦。

在Proactor模式中，**I/O操作的执行由操作系统处理，应用程序只需要关注I/O操作完成后的事件响应**。。

















### Proactor 模式 和 Reactor 模式的区别

**Reactor 是非阻塞同步网络模式，感知的是就绪可读写事件**。

**Proactor 是异步网络模式， 感知的是已完成的读写事件**。



Reactor 模式是基于「待完成」的 I/O 事件，而 Proactor 模式则是基于「已完成」的 I/O 事件。