### Collector 接口 

- `Collector` 是 Java Stream API 中的一个接口，定义了将流中元素累积到一个可变结果容器的操作。`Collector` 接口中定义了一系列方法，包括创建结果容器、将元素添加到容器、合并两个部分结果等。



### Colletors 工具类

- `Collectors` 是 `Collector` 接口的一个实用工具类，它提供了一些静态方法，用于创建常见的收集器实例。这些静态方法返回的收集器已经预先定义了一些常见的行为，例如将流中的元素收集到列表、集合、映射等中。

在简单的情况下，您可以直接使用 `Collectors` 中的静态方法，而不必手动实现 `Collector` 接口。但是，如果您需要自定义收集逻辑，或者需要处理特殊情况，可以实现 `Collector` 接口来创建自定义的收集器。



### 参考资料

[Using a Collector as a Terminal Operation - Dev.java](https://dev.java/learn/api/streams/using-collectors/#intermediate-collectors)