### 一、main() 方法

Java程序的入口是`main`方法。

`main`方法可以接受一个命令行参数，它是一个`String[]`数组。

这个命令行参数由 JVM 接收用户输入并传给`main`方法：

```java
public class Main {
    public static void main(String[] args) {
        for (String arg : args) {
            System.out.println(arg);
        }
    }
}
```





### 二、命令行参数解析

如何解析命令行参数需要由程序自己实现。





# 参考资料

[命令行参数 - 廖雪峰的官方网站 (liaoxuefeng.com)](https://www.liaoxuefeng.com/wiki/1252599548343744/1259544070059520)