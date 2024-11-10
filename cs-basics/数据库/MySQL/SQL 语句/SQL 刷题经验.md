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



### 多表查询

链接查询（JOIN）：关联多个表的数据

子查询（Subquery）：根据其他表的查询结果来筛选数据

组合查询( Union )：合并来自不同表、条件下的数据



#### **多表查询-子查询**

**子查询**是指在一个 SQL 查询中嵌套另一个查询。子查询可以出现在 `SELECT`、`FROM`、`WHERE`、`HAVING` 等语句中。它通常用于**查询一个表的数据，并将查询结果作为条件来筛选另一个表的数据**。

**多表查询**，通过**子查询**在 `question_practice_detail` 表中筛选出符合条件的记录。

```sql
SELECT device_id, question_id, result
FROM question_practice_detail
WHERE device_id IN (
    SELECT device_id
    FROM user_profile
    WHERE university = "浙江大学"
)
```





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



**多表查询-组合查询（Union 查询）**

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

