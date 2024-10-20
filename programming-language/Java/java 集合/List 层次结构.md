### List 接口  

 [`List`](https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/util/List.html)相对于 [`Collection`](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Collection.html)  新增的功能主要有：

- 表示有序的集合，即元素按照插入顺序排列。（允许存储相同的元素，同一个元素可以出现多次）
- 列表的元素有一个对应的索引。因此，通过索引可以访问列表中的元素。

![The Collection Interface Hierarchy](images/01_interfaces-hierarchy.png)



相应地，List 也提供了一些新的 API，

- 根据索引访问元素，从而进行增删改查
- 找到元素对应的索引
- 获取 sublist 

具体情况查看 [Extending Collection with List - Dev.java](https://dev.java/learn/api/collections-framework/lists/#accessing-with-index)



### List 接口的实现类







### 是否可以添加 null 值

`ArrayList` 中可以存储任何类型的对象，包括 `null` 值。

