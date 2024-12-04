**SQLW1** **每个月Top3的周杰伦歌曲**

[每个月Top3的周杰伦歌曲_牛客题霸_牛客网](https://www.nowcoder.com/practice/4ab6d198ea8447fe9b6a1cad1f671503?tpId=375&tqId=10737572&ru=/exam/company&qru=/ta/sql-big-write/question-ranking&sourceUrl=%2Fexam%2Fcompany)

**ROW_NUMBER()**: 给每个分区内的记录按照排序规则分配一个唯一的行号。

`ROW_NUMBER() OVER(PARTITION BY MONTH(p.fdate) ORDER BY COUNT(s.song_name) DESC, p.song_id ASC) AS ranking` 在分组后执行，**先按播放次数（COUNT(s.song_name)）降序排序**。如果播放次数相同，则按 `song_id` 升序排序。

```
WITH t AS (
    SELECT
        MONTH(p.fdate) AS `month`
        ,ROW_NUMBER() OVER(PARTITION BY MONTH(p.fdate) ORDER BY COUNT(s.song_name) DESC, p.song_id ASC) AS ranking
        ,s.song_name
        ,COUNT(s.song_name) AS play_pv
    FROM
        play_log p
        LEFT JOIN song_info s ON p.song_id = s.song_id
        LEFT JOIN user_info u ON p.user_id = u.user_id
    WHERE
        YEAR(p.fdate) = 2022         -- 在2022年
        AND u.age BETWEEN 18 AND 25  -- 18-25岁用户
        AND s.singer_name = '周杰伦'  -- 周杰伦的歌曲
    GROUP BY MONTH(p.fdate), s.song_name, p.song_id  -- 窗口函数中用到song_id，所以分组时要加上
)
SELECT * FROM t WHERE ranking <= 3  -- 每个月播放次数top 3
```





[获取指定客户每月的消费额_牛客题霸_牛客网](https://www.nowcoder.com/practice/ed04f148b63e469e8f62e051d06a46f5?tpId=375&tqId=10858424&ru=/exam/company&qru=/ta/sql-big-write/question-ranking&sourceUrl=%2Fexam%2Fcompany)

关键点在于善用 `DATE_FORMAT` 函数。

```
SELECT  DATE_FORMAT(t_time, '%Y-%m') AS time,  -- 提取年份-月份格式
        SUM(t_amount) AS total
FROM trade
INNER JOIN customer ON t_cus = c_id 
WHERE c_name = 'TOM' AND YEAR(t_time) = 2023 AND t_type = 1
GROUP BY time
ORDER BY time
```





[查询连续入住多晚的客户信息？_牛客题霸_牛客网](https://www.nowcoder.com/practice/5b4018c47dfd401d87a5afb5ebf35dfd?tpId=375&tqId=10858425&ru=/exam/company&qru=/ta/sql-big-write/question-ranking&sourceUrl=%2Fexam%2Fcompany)

SQL 题目经常要使用日期相关函数。

`DATEDIFF(checkout_time, checkin_time)`  求两个日期之间相隔的天数。

```
SELECT user_id, ct.room_id, room_type, DATEDIFF(checkout_time, checkin_time) AS days
FROM guestroom_tb AS gt
INNER JOIN checkin_tb AS ct
ON gt.room_id = ct.room_id
WHERE ct.checkin_time >= '2022-06-12' AND DATEDIFF(checkout_time, checkin_time) > 1
ORDER BY days, room_id, user_id DESC;
```





[统计所有课程参加培训人次_牛客题霸_牛客网](https://www.nowcoder.com/practice/98aad5807cf34a3b960cc8a70ce03f53?tpId=375&tqId=10858426&ru=/exam/company&qru=/ta/sql-big-write/question-ranking&sourceUrl=%2Fexam%2Fcompany)

关键在于文本处理，可以使用’

- 模式匹配  ， 通配符
- **文本处理函数**，这个文本处理函数比较难想到



```
WITH COURSENUMS AS (
     SELECT 
        CASE
            WHEN course IS NULL THEN 0
            WHEN course LIKE'course_,course_,course_' THEN 3
            WHEN course LIKE'course_,course_' THEN 2
            WHEN course LIKE'course_' THEN 1
            ELSE 0
       END AS course_nums
    FROM  cultivate_tb
)
SELECT SUM(course_nums) AS staff_nums
FROM COURSENUMS;
```



```sql
WITH COURSENUMS AS (
    SELECT 
        CASE
            WHEN course IS NULL THEN 0   -- 如果课程为 NULL
            WHEN course = '' THEN 0      -- 如果课程为空字符串
            ELSE LENGTH(course) - LENGTH(REPLACE(course, ',', '')) + 1  -- 计算课程数量
        END AS course_nums
    FROM cultivate_tb
)
SELECT SUM(course_nums) AS staff_nums
FROM COURSENUMS;
```







[查询培训指定课程的员工信息_牛客题霸_牛客网](https://www.nowcoder.com/practice/a0ef4574056e4a219ee7d651ba82efef?tpId=375&tqId=10858427&ru=/exam/company&qru=/ta/sql-big-write/question-ranking&sourceUrl=%2Fexam%2Fcompany)

模式匹配

```sql
SELECT st.staff_id, staff_name
FROM staff_tb AS st
INNER JOIN cultivate_tb AS ct
ON  st.staff_id = ct.staff_id
WHERE course LIKE '%course3%';
```





[推荐内容准确的用户平均评分_牛客题霸_牛客网](https://www.nowcoder.com/practice/2dcac73b647247f0aef0b261ed76b47e?tpId=375&tqId=10858428&ru=/exam/oj&qru=/ta/sql-big-write/question-ranking&sourceUrl=%2Fexam%2Foj%3FquestionJobId%3D10%26subTabName%3Donline_coding_page)

去重

```
WITH RecTrue AS(
    SELECT DISTINCT user_id, score
    FROM recommend_tb AS rt
    INNER JOIN user_action_tb AS ua
    ON rt.rec_user = ua.user_id AND rt.rec_info_l = ua.hobby_l
)
SELECT AVG(score) AS avg_score
FROM RecTrue;
```





[统计各岗位员工平均工作时长_牛客题霸_牛客网](https://www.nowcoder.com/practice/b7220791a95a4cd092801069aefa1cae?tpId=375&tqId=2452517&ru=/exam/oj&qru=/ta/sql-big-write/question-ranking&sourceUrl=%2Fexam%2Foj%3FquestionJobId%3D10%26subTabName%3Donline_coding_page)

`TIMESTAMPDIFF(MINUTE, first_clockin, last_clockin) / 60.0`： 统计员工工作时长（以小时为单位）。为了确保统计时长准确，可以通过计算分钟差再转换为小时，这样能够将多余的分钟部分纳入统计结果。

这是因为 `TIMESTAMPDIFF(HOUR, first_clockin, last_clockin)`  只会计算整小时的差值，忽略多余的分钟差。例如，`08:30` 到 `10:45` 的差值会被计算为 `2` 小时，而忽略了额外的 `15` 分钟。

```sql
WITH StaffHour AS(
    SELECT st.staff_id, post, TIMESTAMPDIFF(MINUTE, first_clockin, last_clockin) / 60.0 AS hours
    FROM staff_tb AS st
    INNER JOIN attendent_tb AS at
    ON st.staff_id = at.staff_id
)
SELECT post, AVG(hours) AS work_hours
FROM StaffHour
GROUP BY post
ORDER BY work_hours DESC;
```



[查找入职员工时间升序排名的情况下的倒数第三的员工所有信息_牛客题霸_牛客网](https://www.nowcoder.com/practice/ec1ca44c62c14ceb990c3c40def1ec6c?tpId=82&tqId=29754&rp=1&ru=/exam/oj&qru=/exam/oj&sourceUrl=%2Fexam%2Foj%3FquestionJobId%3D10%26subTabName%3Donline_coding_page&difficulty=undefined&judgeStatus=undefined&tags=&title=)

区分 DENSE_RANK() 和 RANK()  这两个函数的区别。

```
WITH RowNum AS(
    SELECT emp_no, birth_date, first_name, last_name, gender, hire_date,
        DENSE_RANK() OVER (ORDER BY hire_date DESC) AS row_num
    FROM employees
)
SELECT emp_no, birth_date, first_name, last_name, gender, hire_date
FROM RowNum
WHERE row_num = 3
ORDER BY emp_no;
```





[查找薪水记录超过15条的员工号emp_no以及其对应的记录次_牛客题霸_牛客网](https://www.nowcoder.com/practice/6d4a4cff1d58495182f536c548fee1ae?tpId=82&tqId=29759&rp=1&ru=/exam/oj&qru=/exam/oj&sourceUrl=%2Fexam%2Foj&difficulty=undefined&judgeStatus=undefined&tags=&title=)

聚合函数可以在 HAVING 语句中使用。

```
SELECT emp_no, COUNT(*) AS t
FROM salaries
GROUP BY emp_no
HAVING COUNT(*) >= 15;
```

