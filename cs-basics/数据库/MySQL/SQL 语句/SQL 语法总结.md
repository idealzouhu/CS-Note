## 普通字段

### Limit 字段

`LIMIT offset, row_count`

- **`offset`**：指定从哪一行开始返回数据（偏移量）。**第一行的索引为 0**。
- **`row_count`**：指定返回的行数。







### Group 查询

**`WHERE`**：在**分组之前**对数据进行筛选。

**`HAVING`**：在**分组之后**对聚合结果进行筛选。



```sql
SELECT column1, column2, AGG_FUNC(column3)
FROM table_name
WHERE condition
GROUP BY column1, column2
HAVING condition_on_aggregate_function
ORDER BY column1;
```



MySQL 允许在 `GROUP BY` 和 `ORDER BY` 子句中使用 `SELECT` 中计算的字段的别名。[获取指定客户每月的消费额_牛客题霸_牛客网](https://www.nowcoder.com/practice/ed04f148b63e469e8f62e051d06a46f5?tpId=375&tqId=10858424&ru=/exam/company&qru=/ta/sql-big-write/question-ranking&sourceUrl=%2Fexam%2Fcompany)

```
SELECT  DATE_FORMAT(t_time, '%Y-%m') AS time,  -- 提取年份-月份格式
        SUM(t_amount) AS total
FROM trade
INNER JOIN customer ON t_cus = c_id 
WHERE c_name = 'TOM' AND YEAR(t_time) = 2023 AND t_type = 1
GROUP BY time
ORDER BY time
```





### 逻辑操作符

- `NOT` 是一个 **逻辑操作符**，用于对一个条件进行取反。



### 子查询操作符

- `EXISTS` 是一个用于子查询的操作符，通常用于检查子查询是否返回任何行。如果子查询至少返回一行数据，`EXISTS` 就返回 `TRUE`。



### 集合操作符

- `IN` 是一个用于比较的操作符，用于判断某个值是否在指定的列表、子查询或范围内。如果某个值在列表、范围或子查询的返回结果中，则 `IN` 返回 `TRUE`。





### 存在

**`SELECT 1` 并不实际提取任何数据**，它只需要验证是否存在满足条件的行。如果有至少一行符合条件，`EXISTS` 就会返回 `TRUE`，否则返回 `FALSE`。

```sql
SELECT 1 
FROM Score sc1 
WHERE sc1.sid = s.sid AND sc1.cid = '01'
```

这段查询通常会被用在 `EXISTS` 或 `NOT EXISTS` 语句中

```sql
SELECT s.sid, s.sname
FROM student s
WHERE NOT EXISTS (
    SELECT 1 
    FROM Score sc1 
    WHERE sc1.sid = s.sid AND sc1.cid = '01'
);
```

`NOT EXISTS` 子查询会检查是否有记录存在。如果子查询返回任何记录，则 `NOT EXISTS` 为 `FALSE`，否则为 `TRUE`。



### 分组查询

**分组计算练习题**

Group by 和 Order By 支持多个字段 [分组计算练习题_牛客题霸_牛客网](https://www.nowcoder.com/practice/009d8067d2df47fea429afe2e7b9de45?tpId=199&tqId=1975670&ru=/exam/company&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Fcompany)

```sql
SELECT gender, university,
       count(id) as user_num, 
       AVG(active_days_within_30) as avg_active_day,
       AVG(question_cnt) as avg_question_cnt
FROM user_profile
GROUP BY gender, university
ORDER BY gender, university
```



### 多字段分组

**`GROUP BY` 子句** 将结果集分组为若干个小组。<font color="red">每个唯一的 `(month, song_name, song_id)` 组合会被视为一个独立的分组</font>。

**`COUNT(s.song_name)`** 统计每个分组内 `song_name` 列不为空的记录数。

```
ELECT
    MONTH(p.fdate) AS `month`,
    s.song_name,
    p.song_id,
    COUNT(s.song_name) AS play_pv
FROM
    play_log p
    LEFT JOIN song_info s ON p.song_id = s.song_id
    LEFT JOIN user_info u ON p.user_id = u.user_id
WHERE
    YEAR(p.fdate) = 2022
    AND u.age BETWEEN 18 AND 25
    AND s.singer_name = '周杰伦'
GROUP BY MONTH(p.fdate), s.song_name, p.song_id;
```





**常见问题**

 `only_full_group_by` 模式的问题。

当 `only_full_group_by` 模式启用时，如果查询中使用了 `GROUP BY` 子句，那么 `SELECT` 中列出的字段要么需要出现在 `GROUP BY` 子句中，要么需要使用聚合函数（如 `MIN()`、`MAX()`、`COUNT()` 等）

当出现这个问题时，替换方案为窗口函数 

[找出每个学校GPA最低的同学_牛客题霸_牛客网](https://www.nowcoder.com/practice/90778f5ab7d64d35a40dc1095ff79065?tpId=199&tqId=1980672&ru=/exam/company&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Fcompany)



### 多表查询

链接查询（JOIN）：关联多个表的数据

子查询（Subquery）：根据其他表的查询结果来筛选数据

组合查询( Union )：合并来自不同表、条件下的数据



#### **多表查询-子查询**

**子查询**是指在一个 SQL 查询中嵌套另一个查询。子查询可以出现在 `SELECT`、`FROM`、`WHERE`、`HAVING` 等语句中。它通常用于**查询一个表的数据，并将查询结果作为条件来筛选另一个表的数据**。

**多表查询**，通过**子查询**在 `question_practice_detail` 表中筛选出符合条件的记录。对于这种场景，MySQL **不需要子查询的别名**，因为它并没有把子查询的结果当作一个表来使用，而是直接作为筛选条件。

```sql
SELECT device_id, question_id, result
FROM question_practice_detail
WHERE device_id IN (
    SELECT device_id
    FROM user_profile
    WHERE university = "浙江大学"
)
```





当使用子查询时，**子查询必须要有别名**，这是 MySQL 的语法要求。如果不添加子查询别名，以下代码会报错。

```
SELECT device_id, university, gpa
FROM(
    SELECT device_id, university, gpa,
        RANK() OVER (PARTITION BY university ORDER BY gpa) AS row_num
    FROM user_profile
) AS subquery
WHERE row_num = 1
```



在子查询中，可以使用 **`with` 子句定义临时结果集**，支持普通查询和递归查询。

```
WITH StudentTotalScores AS (
    -- 计算每个学生的总分
    SELECT
        stu_id,
        SUM(score) AS total_score
    FROM
        student_score
    GROUP BY
        stu_id
),
RankedStudents AS (
    -- 给学生按总分排名
    SELECT
        stu_id,
        total_score,
        RANK() OVER(ORDER BY total_score DESC) AS ranking
    FROM
        StudentTotalScores
)
-- 获取排名在第 5 到第 10 的学生
SELECT
    stu_id,
    total_score
FROM
    RankedStudents
WHERE
    ranking BETWEEN 5 AND 10;

```

其中，`RANK() OVER(ORDER BY total_score DESC) AS ranking`   用于排名。**`RANK()`** 是一个窗口函数（**Window Function**），**`OVER` 子句** 定义了窗口函数的排序方式和作用范围。









#### **多表查询-链接查询（JOIN 查询）**

  [统计每个学校的答过题的用户的平均答题数_牛客题霸_牛客网](https://www.nowcoder.com/practice/88aa923a9a674253b861a8fa56bac8e5?tpId=199&tqId=1975674&ru=/exam/company&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Fcompany)

`FORMAT(AVG(answer_cnt), 4)`：将平均值格式化为**字符串**，保留 4 位小数，适合展示结果时使用。

`count(distinct)` 用来确保每个设备只计算一次，避免重复计数。

```sql
select university,
    count(question_id) / count(distinct qpd.device_id) as avg_answer_cnt
from question_practice_detail as qpd
inner join user_profile as up
on qpd.device_id=up.device_id
group by university
```





[查找所有员工的last_name和first_name以及对_牛客题霸_牛客网](https://www.nowcoder.com/practice/5a7975fabe1146329cee4f670c27ad55?tpId=82&tqId=29771&rp=1&ru=/exam/oj&qru=/exam/oj&sourceUrl=%2Fexam%2Foj%3FquestionJobId%3D10%26subTabName%3Donline_coding_page&difficulty=undefined&judgeStatus=undefined&tags=&title=)

一次性查询多个表，通常中间的表包含两外两个表的外键

使用 LEFT JOIN

```sql
SELECT e.last_name, e.first_name, dp.dept_name
FROM employees AS e
LEFT JOIN dept_emp AS de
ON e.emp_no = de.emp_no
LEFT JOIN departments AS dp
ON dp.dept_no = de.dept_no
```





#### **多表查询-组合查询（Union 查询）**

union all 可以保证结果不去重 [查找山东大学或者性别为男生的信息_牛客题霸_牛客网](https://www.nowcoder.com/practice/979b1a5a16d44afaba5191b22152f64a?tpId=199&tqId=1975677&ru=/exam/company&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Fcompany)

```sql
SELECT device_id, gender, age, gpa
FROM user_profile
WHERE university = "山东大学"
Union all
SELECT device_id, gender, age, gpa
FROM user_profile
WHERE gender = "male"
```







#### JOIN 条件和 WHERE 条件

首先无条件地连接 `student` 表和 `Score` 表。然后在连接的结果中，筛选出 `sc.cid = '02'` 的记录。

```
FROM student s
JOIN Score sc ON s.sid = sc.sid
WHERE sc.cid = '02'
```



连接表的时候就筛选出 `sc.cid = '02'` 的记录，然后再将符合条件的结果与 `student` 表连接。

```
FROM student s
JOIN Score sc 
ON s.sid = sc.sid AND sc.cid = '02'

```



**在 `JOIN` 子句中使用 `ON` 条件** 通常更高效，因为它减少了在连接时的记录数量。**在 `WHERE` 子句中使用条件** 则适用于在数据连接之后进一步筛选。





## 常用函数

### 聚合函数

在 **MySQL** 中，`COUNT`、`AVG` 等函数属于 **聚合函数（Aggregate Functions）**。它们的作用是对一组数据执行计算，并返回一个单一的结果，用于总结或概括数据。

聚合函数对一组数据执行计算，通常与 `GROUP BY` 、`HAVING`一起使用，适合用于统计和分析。如果在 MySQL 中**不使用 `GROUP BY` 分组**，直接使用聚合函数，它会将整个表或结果集视为**一个整体（单一分组）**进行计算。

**`NULL` 的处理：**

- `COUNT(*)` 会统计 `NULL`。
- 其他函数如 `AVG`、`SUM` 会忽略 `NULL`。



注意事项：

**不能混用普通列和聚合函数：** 如果查询中既包含普通列又包含聚合函数，则必须用 `GROUP BY` 分组，否则会报错：

```sql
SELECT department_id, COUNT(*) FROM employees;
```



### 条件函数

**`CASE`** 是一个条件函数，用于根据条件的不同返回不同的结果，类似于编程语言中的 `if-else` 语句。

```sql
# 根据某个字段的值返回不同的结果
CASE column_name
    WHEN value1 THEN result1
    WHEN value2 THEN result2
    ELSE result_default
END

# 根据布尔条件返回结果
CASE 
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    ELSE result_default
END AS new_colunm_name
```





CASE 条件函数和 GROUP 函数联合使用。[计算25岁以上和以下的用户数量_牛客题霸_牛客网](https://www.nowcoder.com/practice/30f9f470390a4a8a8dd3b8e1f8c7a9fa?tpId=199&tqId=1975678&ru=/exam/company&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Fcompany)

- `CASE` 的执行时机发生在 GROUP 函数之前, COUNT 的执行实际在 GROUP 函数之后.
- `age IS NULL` 有效, `age = NULL` 无效。在 SQL 中，`NULL` 表示“未知值”。任何与 `NULL` 的比较（无论是 `=`、`<>` 还是其他运算符）都会返回 `UNKNOWN`（一个三值逻辑的状态），而不是 `TRUE` 或 `FALSE`。

```sql
SELECT 
    CASE
    WHEN age < 25 OR age IS null THEN "25岁以下"
    WHEN age >= 25 THEN "25岁及以上"
    END AS age_cut,
    count(*) number
FROM user_profile
GROUP BY age_cut
```



[查看不同年龄段的用户明细_牛客题霸_牛客网](https://www.nowcoder.com/practice/ae44b2b78525417b8b2fc2075b557592?tpId=199&tqId=1975679&ru=/exam/company&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Fcompany)





### 日期时间函数

[SQL语法基础知识总结 | JavaGuide](https://javaguide.cn/database/sql/sql-syntax-summary.html#日期和时间处理)

| 函数名                            | 功能说明                                               | 示例                                               | 返回结果              |
| --------------------------------- | ------------------------------------------------------ | -------------------------------------------------- | --------------------- |
| `CURDATE()`                       | 获取当前日期（不包含时间）                             | `SELECT CURDATE();`                                | `2024-11-16`          |
| `CURRENT_DATE()`                  | 获取当前日期（不包含时间）                             | `SELECT CURRENT_DATE();`                           | `2024-11-16`          |
| `CURTIME()`                       | 获取当前时间（不包含日期）                             | `SELECT CURTIME();`                                | `14:23:45`            |
| `CURRENT_TIME()`                  | 获取当前时间（不包含日期）                             | `SELECT CURRENT_TIME();`                           | `14:23:45`            |
| `NOW()`                           | 获取当前日期和时间                                     | `SELECT NOW();`                                    | `2024-11-16 14:23:45` |
| `CURRENT_TIMESTAMP()`             | 获取当前日期和时间                                     | `SELECT CURRENT_TIMESTAMP();`                      | `2024-11-16 14:23:45` |
| `UNIX_TIMESTAMP()`                | 获取当前时间的 Unix 时间戳（从 1970-01-01 开始的秒数） | `SELECT UNIX_TIMESTAMP();`                         | `1731762225`          |
| `FROM_UNIXTIME(timestamp)`        | 将 Unix 时间戳转换为日期时间                           | `SELECT FROM_UNIXTIME(1731762225);`                | `2024-11-16 14:23:45` |
| `DATE()`                          | 提取日期部分                                           | `SELECT DATE('2024-11-16 14:23:45');`              | `2024-11-16`          |
| `TIME()`                          | 提取时间部分                                           | `SELECT TIME('2024-11-16 14:23:45');`              | `14:23:45`            |
| `YEAR(date)`                      | 获取年份                                               | `SELECT YEAR('2024-11-16');`                       | `2024`                |
| `MONTH(date)`                     | 获取月份（1-12）                                       | `SELECT MONTH('2024-11-16');`                      | `11`                  |
| `DAY(date)`                       | 获取日期（1-31）                                       | `SELECT DAY('2024-11-16');`                        | `16`                  |
| `HOUR(time)`                      | 获取小时部分                                           | `SELECT HOUR('14:23:45');`                         | `14`                  |
| `MINUTE(time)`                    | 获取分钟部分                                           | `SELECT MINUTE('14:23:45');`                       | `23`                  |
| `SECOND(time)`                    | 获取秒部分                                             | `SELECT SECOND('14:23:45');`                       | `45`                  |
| `WEEKDAY(date)`                   | 获取星期几（0=星期一, 6=星期日）                       | `SELECT WEEKDAY('2024-11-16');`                    | `5`                   |
| `DAYOFWEEK(date)`                 | 获取星期几（1=星期日, 7=星期六）                       | `SELECT DAYOFWEEK('2024-11-16');`                  | `7`                   |
| `DATEDIFF(date1, date2)`          | 计算两个日期之间的天数差                               | `SELECT DATEDIFF('2024-12-01', '2024-11-16');`     | `15`                  |
| `DATE_ADD(date, INTERVAL n unit)` | 日期增加指定时间量                                     | `SELECT DATE_ADD('2024-11-16', INTERVAL 10 DAY);`  | `2024-11-26`          |
| `DATE_SUB(date, INTERVAL n unit)` | 日期减少指定时间量                                     | `SELECT DATE_SUB('2024-11-16', INTERVAL 1 MONTH);` | `2024-10-16`          |
| `LAST_DAY(date)`                  | 获取指定日期所在月份的最后一天                         | `SELECT LAST_DAY('2024-11-16');`                   | `2024-11-30`          |
| `EXTRACT(unit FROM date)`         | 提取日期中的某个部分（如年、月、日、小时等）           | `SELECT EXTRACT(YEAR FROM '2024-11-16');`          | `2024`                |
| `STR_TO_DATE(str, format)`        | 按指定格式将字符串转换为日期                           | `SELECT STR_TO_DATE('16-11-2024', '%d-%m-%Y');`    | `2024-11-16`          |
| `DATE_FORMAT(date, format)`       | 按指定格式格式化日期                                   | `SELECT DATE_FORMAT('2024-11-16', '%Y/%m/%d');`    | `2024/11/16`          |

[计算用户8月每天的练题数量_牛客题霸_牛客网](https://www.nowcoder.com/practice/847373e2fe8d47b4a2c294bdb5bda8b6?tpId=199&tqId=1975680&ru=/exam/company&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Fcompany)

**date 可以直接使用字符串来比较**。

```
SELECT Day(date) AS day, Count(result) AS question_cnt
FROM question_practice_detail
WHERE date BETWEEN '2021-08-01' AND '2021-08-31'
GROUP BY date 
```



### 文本函数

在 MySQL 里面，**索引从 1 开始**，而不是 0。

以 `SUBSTRING_INDEX()` 函数为例，它的 **索引方式是左闭右闭的**，也就是说，包含了起始位置的字符，而截取的结束位置是 **包含的**（即右闭）。

> 模式匹配可以用来处理更加复杂的文本操作

| 函数名称        | 描述                                                         | 示例                                                         |
| --------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `CONCAT()`      | 将两个或多个字符串连接在一起。                               | `CONCAT('Hello', ' ', 'World')` 结果: `'Hello World'`        |
| `LENGTH()`      | 返回字符串的字符长度。                                       | `LENGTH('Hello')` 结果: `5`                                  |
| `LOWER()`       | 将字符串中的所有字母转换为小写。                             | `LOWER('Hello')` 结果: `'hello'`                             |
| `UPPER()`       | 将字符串中的所有字母转换为大写。                             | `UPPER('Hello')` 结果: `'HELLO'`                             |
| `SUBSTRING()`   | 从字符串中提取子字符串。                                     | `SUBSTRING('Hello World', 1, 5)` 结果: `'Hello'`             |
| `SUBSTRING_INDEX` | 返回指定分隔符之前或之后的子字符串   | `SELECT SUBSTRING_INDEX('apple,banana,cherry', ',', 2);` **返回结果:** 返回第 2 个逗号前的部分 `'apple,banana'` |
| `TRIM()`        | 去除字符串两端的空格。                                       | `TRIM('  Hello ')` 结果: `'Hello'`                           |
| `REPLACE()`     | 替换字符串中的部分内容。                                     | `REPLACE('Hello World', 'World', 'SQL')` 结果: `'Hello SQL'` |
| `INSTR()`       | 返回子字符串首次出现的位置。                                 | `INSTR('Hello World', 'World')` 结果: `7`                    |
| `LEFT()`        | 返回字符串的左边指定长度的子字符串。                         | `LEFT('Hello', 3)` 结果: `'Hel'`                             |
| `RIGHT()`       | 返回字符串的右边指定长度的子字符串。                         | `RIGHT('Hello', 3)` 结果: `'llo'`                            |
| `REVERSE()`     | 反转字符串中的字符顺序。                                     | `REVERSE('Hello')` 结果: `'olleH'`                           |
| `SOUNDEX()`     | 返回字符串的声音指纹，常用于模糊匹配。                       | `SOUNDEX('Hello')` 结果: `'H400'`                            |
| `CHAR_LENGTH()` | 返回字符串的字符数（与 `LENGTH()` 相似，但区别在于字符集的不同）。 | `CHAR_LENGTH('Hello')` 结果: `5`                             |
| `ASCII()`       | 返回字符串第一个字符的 ASCII 值。                            | `ASCII('A')` 结果: `65`                                      |





[统计每种性别的人数_牛客题霸_牛客网](https://www.nowcoder.com/practice/f04189f92f8d4f6fa0f383d413af7cb8?tpId=199&tqId=1975682&ru=/exam/company&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Fcompany)

`SUBSTRING_INDEX(profile, ',', -1)` 会截取 `profile` 字段中最后一个逗号（`,`）之后的部分，即获取 `profile` 字符串的最后一个子串。

```
SELECT SUBSTRING_INDEX(profile,',', -1) AS gender, COUNT(*) AS number
FROM user_submit
GROUP BY gender
```



[截取出年龄_牛客题霸_牛客网](https://www.nowcoder.com/practice/b8d8a87fe1fc415c96f355dc62bdd12f?tpId=199&tqId=1975684&ru=/exam/company&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Fcompany)

多次使用 `SUBSTRING_INDEX` 来获取任意位置的子字符串。

`SUBSTRING_INDEX(SUBSTRING_INDEX(profile, ',', 3), ',', -1)` 等价于 `REGEXP_SUBSTR(profile, '[^,]+', 1, 3)`

```
SELECT SUBSTRING_INDEX(SUBSTRING_INDEX(profile, ',', 3), ',', -1) AS age, 
        COUNT(*) AS number
FROM user_submit
GROUP BY age
```



[提取博客URL中的用户名_牛客题霸_牛客网](https://www.nowcoder.com/practice/26c8715f32e24d918f15db69518f3ad8?tpId=199&tqId=1975683&ru=/exam/company&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Fcompany)

```
SELECT device_id, SUBSTRING_INDEX(blog_url, '/', -1) AS user_name
FROM user_submit
```



### 模式匹配

`LIKE` 用于使用通配符在字符串中进行简单的模式匹配，区分大小写。常见的通配符包括：

| 通配符 | 描述                              |
| ------ | --------------------------------- |
| `%`    | 匹配任意长度的字符（包括0个字符） |
| `_`    | 匹配单个字符                      |



我们还可以使用更加复杂的正则表达式。`REGEXP` 使用正则表达式来匹配字符串，支持更复杂的匹配规则。正则表达式不区分大小写，除非使用 `BINARY`。

| 符号    | 描述                                              |
| ------- | ------------------------------------------------- |
| `.`     | 匹配任意单个字符                                  |
| `^`     | 匹配字符串的开始位置                              |
| `$`     | 匹配字符串的结束位置                              |
| `[]`    | 匹配括号内的任意字符                              |
| `[a-z]` | 匹配指定范围内的字符                              |
| `*`     | 匹配前面的字符0次或多次                           |
| `+`     | 匹配前面的字符1次或多次                           |
| `{n}`   | 匹配前面的字符恰好n次                             |
| `{n,}`  | 匹配前面的字符至少n次                             |
| `{n,m}` | 匹配前面的字符至少n次，至多m次                    |
| `|`     | 表示“或”，匹配符号两边的任意一个                  |
| `()`    | 分组，将多个字符视为一个整体                      |
| `\`     | 转义字符，用于匹配特殊字符（如 `\.` 匹配 `.`）    |
| `\d`    | 匹配任意数字，等价于 `[0-9]`                      |
| `\D`    | 匹配任意非数字字符                                |
| `\w`    | 匹配任意字母、数字、下划线，等价于 `[a-zA-Z0-9_]` |
| `\W`    | 匹配任意非字母、数字、下划线字符                  |
| `\s`    | 匹配空白字符（空格、制表符等）                    |
| `\S`    | 匹配非空白字符                                    |







### 窗口函数

窗口函数（Window Functions）是 SQL 中的一类特殊函数，它可以**对查询结果集中的每一行进行计算，并且与该行相关的其它行一起使用**，但不会像聚合函数那样将结果分组。

**窗口函数通常用于计算累计值、排名、移动平均等操作**。

窗口函数与普通的聚合函数的主要区别在于：

- 聚合函数通常会合并多行数据并返回单一的结果（例如 `SUM()`、`AVG()` 等）。
- 窗口函数则在不改变结果集行数的前提下，对每行进行计算。窗口函数通常会每行数据添加新的一列。

这也是分区和分组的区别。



窗口函数的语法如下：

- **`window_function()`**：表示窗口函数的名称，例如 `ROW_NUMBER()`, `RANK()`, `SUM()` 等。

- **`OVER`**：指定窗口函数的操作范围。

- **`PARTITION BY`**：可选，用来对数据进行分区，每个分区内的行会共享一个窗口。

- **`ORDER BY`**：可选，用于对窗口内的行进行排序。

```sql
SELECT 
    column1,
    column2,
    window_function() OVER (PARTITION BY column1 ORDER BY column2) AS window_result
FROM table_name;
```



窗口函数如下：

| 窗口函数         | 作用描述                                                     | 示例代码                                                     |
| ---------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `ROW_NUMBER()`   | 为每一行生成唯一的行号，按 `ORDER BY` 排序，通常用于去重。   | `ROW_NUMBER() OVER (ORDER BY hire_date)`                     |
| `RANK()`         | 根据排序字段进行排名，相同值会获得相同排名，且排名会跳跃（如：1, 2, 2, 4）。 | `RANK() OVER (PARTITION BY department_id ORDER BY salary DESC)` |
| `DENSE_RANK()`   | 类似于 `RANK()`，但排名不会跳跃（如：1, 2, 2, 3）。          | `DENSE_RANK() OVER (PARTITION BY department_id ORDER BY salary DESC)` |
| `NTILE(n)`       | 将结果集平均分成 `n` 份，并为每行分配一个桶号（从 1 到 n）。 | `NTILE(4) OVER (ORDER BY salary DESC)`                       |
| `SUM()`          | 计算当前窗口内的累计和。                                     | `SUM(salary) OVER (PARTITION BY department_id)`              |
| `AVG()`          | 计算当前窗口内的平均值。                                     | `AVG(salary) OVER (PARTITION BY department_id)`              |
| `MIN()`          | 返回当前窗口内的最小值。                                     | `MIN(salary) OVER (PARTITION BY department_id)`              |
| `MAX()`          | 返回当前窗口内的最大值。                                     | `MAX(salary) OVER (PARTITION BY department_id)`              |
| `COUNT()`        | 计算当前窗口内的行数。                                       | `COUNT(employee_id) OVER (PARTITION BY department_id)`       |
| `LAG()`          | 获取当前行前 `n` 行的值（默认前 1 行），可用于计算差异。     | `LAG(salary, 1) OVER (ORDER BY hire_date)`                   |
| `LEAD()`         | 获取当前行后 `n` 行的值（默认后 1 行），可用于计算差异。     | `LEAD(salary, 1) OVER (ORDER BY hire_date)`                  |
| `FIRST_VALUE()`  | 返回窗口内按排序的第一个值。                                 | `FIRST_VALUE(salary) OVER (PARTITION BY department_id ORDER BY hire_date)` |
| `LAST_VALUE()`   | 返回窗口内按排序的最后一个值。                               | `LAST_VALUE(salary) OVER (PARTITION BY department_id ORDER BY hire_date)` |
| `CUME_DIST()`    | 计算累计分布值，返回当前行的累积百分比（相对于分组）。       | `CUME_DIST() OVER (ORDER BY salary DESC)`                    |
| `PERCENT_RANK()` | 计算百分比排名，与 `RANK()` 类似，但范围在 0 到 1 之间。     | `PERCENT_RANK() OVER (ORDER BY salary DESC)`                 |
| `WINDOW FRAME`   | 控制窗口的范围（如 `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`）。 | `SUM(salary) OVER (ORDER BY hire_date ROWS BETWEEN 2 PRECEDING AND CURRENT ROW)` |





[找出每个学校GPA最低的同学_牛客题霸_牛客网](https://www.nowcoder.com/practice/90778f5ab7d64d35a40dc1095ff79065?tpId=199&tqId=1980672&ru=/exam/company&qru=/ta/sql-quick-study/question-ranking&sourceUrl=%2Fexam%2Fcompany)

窗口函数用于求每个大学的第一名

排名序号从 1 还是

```sql
SELECT device_id, university, gpa
FROM(
    SELECT device_id, university, gpa,
        RANK() OVER (PARTITION BY university ORDER BY gpa) AS row_num
    FROM user_profile
) subquery
WHERE row_num = 1
```









## 解决思路

先将多个表格直接合并起来： 直接暴力合并，然后再一步步筛选。

然后再逐一筛选



#### 常见 SQL 题目的难点

比较复杂的函数处理

小数处理

