

# 一、Shell 简介







## 1.3 运行 Shell 脚本

### 1.3.1 作为可执行程序

在这种方法下，脚本本身被直接执行作为一个可执行程序。为了使脚本可执行，需要添加执行权限，并且可以通过以下步骤来运行：

```sh
chmod +x your_script.sh  # 添加执行权限
./your_script.sh         # 执行脚本
```

这种方法适用于那些脚本的第一行指定了解释器路径的情况，比如以 `#!/bin/bash` 开头的 Bash 脚本。

注意：在Linux系统中，执行脚本或二进制程序时确实需要注意当前目录是否在系统的PATH（环境变量）中。当你运行一个命令时，系统会按照 PATH 的顺序在指定的目录中(比如 `/bin`, `/sbin`, `/usr/bin`，`/usr/sbin` 等 )查找可执行文件。如果当前目录不在PATH中，直接输入脚本 `your_script.sh` 或二进制文件名可能导致系统找不到该文件。因此，我们要一定要写成 `./your_script.sh`



### 1.3.2 作为解释器参数

在这种方法下，脚本文件作为解释器的参数传递给解释器。通常，这适用于没有设置执行权限或者不希望直接运行脚本的情况。

```shell
/bin/sh your_script.sh 
sh your_script.sh
```



# 二、Shell 种类



## 系统的合法shell与 /etc/shells功能

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







# 参考资料

[Shell 教程 | 菜鸟教程 (runoob.com)](https://www.runoob.com/linux/linux-shell.html)