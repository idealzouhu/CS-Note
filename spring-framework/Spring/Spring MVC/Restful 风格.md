

### Restful 的关键特性

**无状态（Stateless）**：每个请求都是独立的，客户端和服务器之间的每个请求都包含所有需要的信息，以便服务器处理请求。服务器不需要记住前一个请求的任何状态。

**统一接口**：通过统一的接口（如 HTTP）访问资源，简化了系统架构。









### Restful 控制器

Restful API 与 网站两者都涉及对 HTTP 请求的响应，关键区别在于响应内容的不同，即

- 网站是用 **HTML** 响应这些请求
- REST API通常以面向数据的格式（如 **JSON** 或 XML）响应。



**Spring MVC HTTP 请求处理注解**主要有：

| 注解            | HTTP 方法        | 典型用法     |
| :-------------- | :--------------- | :----------- |
| @GetMapping     | HTTP GET 请求    | 读取资源数据 |
| @PostMapping    | HTTP POST 请求   | 创建资源     |
| @PutMapping     | HTTP PUT 请求    | 更新资源     |
| @PatchMapping   | HTTP PATCH 请求  | 更新资源     |
| @DeleteMapping  | HTTP DELETE 请求 | 删除资源     |
| @RequestMapping | 通用请求处理     |              |







[手摸手之掌握架构师的编程规范 (yuque.com)](https://www.yuque.com/magestack/12306/bmatdhq46cegg2xe#QuBzK)



**不用使用动词定义 URL**