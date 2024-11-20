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

