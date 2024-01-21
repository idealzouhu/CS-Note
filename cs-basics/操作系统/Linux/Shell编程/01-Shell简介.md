





# 系统的合法shell与 /etc/shells功能

Linux系统有多少可以使用的shell？可以通过**检查/etc/shells 这个文件查询**。/etc/shells包含内容如下：

```
# /etc/shells: valid login shells
/bin/sh
/bin/bash
/usr/bin/bash
/bin/rbash
/usr/bin/rbash
/bin/dash
/usr/bin/dash
/bin/zsh
/usr/bin/zsh
/usr/bin/tmux
/usr/bin/screen

```

注意：其中，/bin/bash就是 **Linux 默认的 shell**

为什么我们系统上合法的 shell 要写入 /etc/shells 这个文件啊？ 这是因为系统某些服务在 运行过程中，会去**检查使用者能够使用的 shells** ，而这些 shell 的查询就是借由 /etc/shells 这个文件啰！





# Bash Shell

bash 是 GNU 计划中重 要的工具软件之一，目前也是 Linux distributions 的标准 shell 。 bash 主要相容于 sh ，并且依据一些使用者 需求而加强的 shell 版本。

## 命令编修能力(history)

命令编修能力(**history**)：记忆使用过的指令。

使用技巧：只要在命令行按“**上下键**”就可以找到前/后一个输入的指令

指令记录存放位置：在你的主文件夹内的 **.bash_history** 啦！ 不过，需要留意的是， ~/.bash_history 记录的是前一次登陆以前所执行过的指令， 而至于这一次登陆所执行的指令都被暂存在内存 中，当你成功的登出系统后，该指令记忆才会记录到 .bash_history 当中！

**优点**： 查询曾经做过的举动

**缺点**：如果被骇客入侵了，那么他只要翻你曾经执行过的指令， 刚好你的指令又跟系统有关，那你的服务器可就伤脑筋了