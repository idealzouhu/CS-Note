### 分组字段

- 不能在 `SELECT` 中直接使用不在 `GROUP BY` 中的非聚合字段，
- 可以在 `SELECT` 中使用聚合函数来间接使用不在 `GROUP BY` 中的非聚合字段
- 如果你不想使用聚合函数，又有分组类似的目的，可以考虑窗口函数。





### 聚合函数和窗口函数冲突

在同一层次的 `SELECT` 列表中使用了聚合函数和窗口函数，并且**试图对聚合结果再次进行窗口计算**，这种操作是不允许的。

[每个商品的销售总额_牛客题霸_牛客网](https://www.nowcoder.com/practice/6d796e885ee44a9cb599f47b16a02ea4?tpId=375&tqId=10824294&ru=/exam/oj&qru=/ta/sql-big-write/question-ranking&sourceUrl=%2Fexam%2Foj%3FquestionJobId%3D10%26subTabName%3Donline_coding_page)

```sql
SELECT name AS product_name, 
    SUM(quantity) AS total_sales,
    RANK() OVER (PARTITION BY category ORDER BY total_sales DESC) AS category_rank
FROM products
INNER JOIN orders 
ON  products.product_id = orders.product_id
GROUP BY name, category
ORDER BY category, total_sales DESC
```





### 执行顺序

报错 `SQL_ERROR_INFO: "Unknown column 'days' in 'where clause'"`

```
SELECT user_id, ct.room_id, room_type, DATEDIFF(checkout_time, checkin_time) AS days
FROM guestroom_tb AS gt
INNER JOIN checkin_tb AS ct
ON gt.room_id = ct.room_id
WHERE ct.checkin_time >= '2022-06-12' AND days > 1
ORDER BY days, room_id, user_id DESC;
```

因为在 **`WHERE` 子句** 中使用了 `days` 列，而 `days` 是通过 `SELECT` 子句中的 `DATEDIFF()` 函数计算出来的**别名**。MySQL 的查询执行顺序决定了，在 `WHERE` 子句执行时，`SELECT` 子句中的别名（例如 `days`）还没有被计算出来，因此 `WHERE` 子句中无法引用它。





解决方案

- 直接在 `WHERE` 中使用 `DATEDIFF()` 
- 使用 `HAVING` 子句，因为 `HAVING` 是在 `SELECT` 之后执行的，因此 `days` 别名在 `HAVING` 子句中是有效的。

```
SELECT user_id, ct.room_id, room_type, DATEDIFF(checkout_time, checkin_time) AS days
FROM guestroom_tb AS gt
INNER JOIN checkin_tb AS ct
ON gt.room_id = ct.room_id
WHERE ct.checkin_time >= '2022-06-12' AND DATEDIFF(checkout_time, checkin_time) > 1
ORDER BY days, room_id, user_id DESC;
```





### NULL

`course == NULL`  会报错 

应该适用 `course is NULL` 