### 什么是 `EXPLAIN` 

[`EXPLAIN`](https://dev.mysql.com/doc/refman/8.4/en/explain.html) 是一个用于分析查询的 SQL 语句，它可以帮助你理解查询执行计划。通过 `EXPLAIN`，你可以看到数据库如何优化并执行查询语句，了解索引使用情况、表连接顺序等信息。这对于优化查询性能非常重要。

通过 `EXPLAIN` ，我们可以优化查询性能：

- **索引调优**：如果查询没有使用索引，你可以通过 `EXPLAIN` 找到问题所在，然后添加或调整索引。

- **性能分析**：`EXPLAIN` 提供的行数估计和查询类型可以帮助你了解查询的性能瓶颈，判断是否查询了大量无用的数据，从而进一步优化 SQL。



### `EXPLAIN` 的作用

`EXPLAIN` 显示了查询执行的步骤和顺序，以及每个步骤的细节，包括：

1. **表的访问顺序**：`EXPLAIN` 通过展示表的读取顺序帮助你理解查询的处理顺序。
2. **索引的使用情况**：它可以帮助确认查询是否有效地使用了索引。如果查询没有使用索引，数据库将进行全表扫描，这可能会影响性能。
3. **行数估计**：`EXPLAIN` 会估计每一步操作影响的行数，可以帮助你识别潜在的性能瓶颈。
4. **连接类型**：在多个表的连接查询中，`EXPLAIN` 显示不同表之间的连接方式（如 `INNER JOIN`、`LEFT JOIN` 等）。
5. **查询的代价**：你可以看到查询的执行代价（成本），并据此做出优化决策。



### `EXPLAIN` 的输出字段

[MySQL :: MySQL 8.4 Reference Manual :: 10.8.2 EXPLAIN Output Format](https://dev.mysql.com/doc/refman/8.4/en/explain-output.html)

执行 `EXPLAIN` 语句后，通常会返回一个表，表中包含如下常见字段：

- **id**：查询中执行的顺序或步骤。`id` 值越大，优先级越高。
- **select_type**：表示查询的类型。常见类型有 `SIMPLE`（简单查询）、`PRIMARY`（主查询）、`SUBQUERY`（子查询）等。
- **table**：当前步骤所访问的表。
- type：表的访问方式，常见值有：
  - `ALL`：全表扫描（性能较差）。
  - `index`：全索引扫描（扫描索引中的所有数据）。
  - `range`：索引范围扫描。
  - `ref`：通过索引查找单个或多个匹配值。
  - `eq_ref`：通过索引查找精确匹配的一行数据。
  - `const/system`：查询一次后结果恒定的表。
- **possible_keys**：查询可能使用的索引。
- **key**：实际使用的索引。如果值为 `NULL`，表示没有使用索引。
- **key_len**：使用的索引的长度。
- **ref**：用于和索引进行比较的列或常量。
- **rows**：估计需要扫描的行数。
- **Extra**：其他额外信息，可能显示优化器对查询的处理。例如，`Using index` 表示使用了覆盖索引，`Using where` 表示查询使用了 `WHERE` 过滤条件。





### 参考资料

[MySQL :: MySQL 8.4 Reference Manual :: 10.8.2 EXPLAIN Output Format](https://dev.mysql.com/doc/refman/8.4/en/explain-output.html)