



```
# 插入数据
INSERT INTO stu(sid, sname) VALUES('s_1001', 'zhangSan');
INSERT INTO stu VALUES('s_1002', 'liSi', 32, 'female');
# 修改数据
UPDATE stu SET sname=’liSi’, age=’20’ WHERE age>50 AND gender=’male’;
# 删除数据
```



```
SELECT selection_list /*要查询的列名称*/
  FROM table_list /*要查询的表名称*/
  WHERE condition /*行条件*/
  GROUP BY grouping_columns /*对结果分组*/
  HAVING condition /*分组后的行条件*/
  ORDER BY sorting_columns /*对结果分组*/
  LIMIT offset_start, row_count /*结果限定*/
```



[MySQL常用基础语法_mysql的语法-CSDN博客](https://blog.csdn.net/qq_36969257/article/details/81364113)

[MySQL—内连接和外连接区别_内连接和外连接的区别-CSDN博客](https://blog.csdn.net/johnhan9/article/details/88686288)

