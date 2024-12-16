### SQL 查询的执行顺序

所有的查询语句都是从FROM开始执行，在执行过程中，每个步骤都会生成一个虚拟表，这个虚拟表将作为下一个执行步骤的输入，最后一个步骤产生的虚拟表即为输出结果。

```sql
(9) 	SELECT
(10) 		DISTINCT <column>
(6) 		AGG_FUNC(<column>) OR <expression>,
(1) 	FROM <left table>
(3) 	<join type> JOIN <right table> 
(2) 	ON <join condition>
(4) 	WHERE <where condition>
(5) 	GROUP BY <group_by list>
(7) 	WITH {CUBE | ROLLUP}
(8) 	HAVING <having condition>
(11) 	ORDER BY <order_by_list>
(12) 	LIMIT <limit number>;
```



1. **`FROM` 和 `JOIN`**：首先会从数据源（如表格、视图、子查询等）中获取数据。如果有连接（`JOIN`），会在此时进行连接操作。
2. **`WHERE`**：接下来会根据 `WHERE` 子句中的条件过滤数据。
3. **`GROUP BY`**：在 `SELECT` 之后会根据 `GROUP BY` 子句对数据进行分组。这里会按照 `CASE` 语句的结果来进行分组。
4. **`AGG FUNC`** 和 **`expression`**： 聚合函数，用于对一列数据进行汇总、计算、统计等操作。`<expression>` 是一个通用术语，表示可以计算出一个值的任何有效表达式。它可以是一个简单的列、常量、或更复杂的运算表达式。`<expression>` 可以包括算术运算、字符串操作、逻辑运算等。
5. **`HAVING`**：如果有 `HAVING` 子句，它会在分组后对分组结果进行筛选。
6. **`SELECT`**：然后会计算 `SELECT` 子句中的表达式，包括 `CASE` 函数。
7. **`ORDER BY`**：最后会按照 `ORDER BY` 子句中的条件排序结果。
8. **`LIMIT`**：如果有 `LIMIT` 子句，会在最后对结果集进行限制。









### 注意事项

如果想要对分组后的数据进行筛选，可以使用 HAVING， 不能使用 WHERE。











### 参考资料