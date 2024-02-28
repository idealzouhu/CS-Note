





JVM通过环境变量 `classpath` 决定搜索`class`的路径和顺序；

不推荐设置系统环境变量 `classpath` ，始终建议通过 `-cp` 命令传入；





Spring工程编译后，会按照配置，将生成的.class文件和各类资源文件放到classes文件夹下。同时配置也决定了是直接放到classes根目录，还是带有文件夹路径。



# 参考资料

[classpath和jar - 廖雪峰的官方网站 (liaoxuefeng.com)](https://www.liaoxuefeng.com/wiki/1252599548343744/1260466914339296)

[详解Spring项目中的classpath路径_spring classpath-CSDN博客](https://blog.csdn.net/Vermont_/article/details/109118810)