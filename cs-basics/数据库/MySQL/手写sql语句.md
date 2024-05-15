

给你一个表，(id，userid，amount，month，day)

筛选出所有的日期在 3.1-3.10 且累计金额大于等于 100w 的数据

```mysql
SELECT userid,SUM(amount)As total_amount
FROM transactions
WHERE (month =3 AND day BETWEEN 1 AND 10)
GROUP BY userid
HAVING total amount >= 1000000;
```

补充： 我们应该如何优化上述语句？