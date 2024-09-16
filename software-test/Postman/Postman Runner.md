### 使用案例

Postman Runner 的API 请求和 普通的单个 API 请求稍微有点区别。

在 Postman 中，`{{}}` 是用来表示**变量**的占位符。当你在请求的 URL、头部、参数、或其他位置中使用 `{{变量名}}` 的形式，Postman 会自动查找并替换其中的变量值。

<font color="red">**在使用批量测试的时候，我们必须使用占位符**</font>。

### 创建测试数据

以 GET 请求  `http://localhost:8080/test/{{id}}`  为例， 创建一个包含不同 `id` 值的 JSON 或 CSV 文件。

ts

```csv
id
1
2
3
4
5

```



```
[
  {"id": 1},
  {"id": 2},
  {"id": 3},
  {"id": 4},
  {"id": 5}
]
```



### 查询参数

`http://localhost:8080/info?id={{id}}`， 将实际请求参数用 {{id}} 表示。

否则的话，无法读取数据集里面的输出，从而替换









## 可能遇到的问题

### get 请求批量测试

在 get 请求批量测试时，所有请求的路径参数都为同一个值，并没有使用自定义数据集里面的参数。

原因：get 请求里面已经把路径参数给赋初值了

解决方案：删除复制

![image-20240910163940134](images/image-20240910163940134.png)











### 参考资料

[使用集合运行程序（Collection Runner） | Postman 官方帮助文档中文版 (xiniushu.com)](https://postman.xiniushu.com/docs/collections/running-collections/intro-to-collection-runs)

[Postman之Runner与Data数据文件处理_postman runner-CSDN博客](https://blog.csdn.net/yang_yang_heng/article/details/109103946)