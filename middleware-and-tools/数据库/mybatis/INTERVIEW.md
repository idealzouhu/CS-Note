**Mybatis 怎么实现多表查询？**

- 创建 DTO 类。

- 使用 resultType。 可能会需要修改实体类。

[MyBatis实现多表查询_mybatis两表联查条件查询-CSDN博客](https://blog.csdn.net/Mq_sir/article/details/116400731)



**`#{}` 和 `${}` 的区别**

 默认情况下，使用 `#{}` 参数语法时，MyBatis 会创建 `PreparedStatement` 参数占位符，并通过**占位符**安全地设置参数（就像使用 ? 预处理一样）。`#{}`: 适用于传递任何动态参数，尤其是用户输入， **可以有效防止 SQL 注入攻击**。

 `${}` 主要是用于**字符串替换**，适用于需要动态替换 SQL 片段的场景，比如动态列名或表名，但可能会导致 SQL 注入。

举个例子，

```java
@Select("select * from user where ${column} = #{value}")
User findByColumn(@Param("column") String column, @Param("value") String value);
```

在执行时，如果 `column` 的值是 `name`，这个查询会变成：

```sql
SELECT * FROM user WHERE name = ?
```

这里的 `${column}` 是直接替换为 `name` 字符串，而 `#{value}` 则被安全地绑定为一个参数占位符。