

容器有两种永久化存储方式：**卷（volumes）**和 **绑定挂载（bind mounts）**。

Docker默认下，所有文件将会存储在容器里的可写的容器层（container layer）。[[39\]](https://zh.wikipedia.org/zh-cn/Docker#cite_note-:02-39)

- 数据与容器为一体。随着容器消失，数据将消失；难以与其他程序（容器）共享。可以采用挂载文件的方式解决。
- 由于容器的写入层是与宿主机器紧紧耦合。所以你难以移动数据到其他机器。