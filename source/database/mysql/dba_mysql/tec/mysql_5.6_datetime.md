---
title: MySQL 5.6 DateTime
---

# 语法

`date_sub('2017-10-10 00:00:00',interval 30 day)`

# 案例

```sql
mysql>select count(*) from (select user_key,SUM(CASE WHEN card_level = 5 THEN 0
ELSE 1 END) AS a from fx_achievement where pay_date between date_sub('2017-10-10 00:00:00',interval 30 day)
and date_sub('2017-10-10 23:59:59',interval 1 day) group by user_key having a=0 and sum(sale) >= 2500) as t1;
+--------------------+
| count(*)           |
+--------------------+
| 7234               |
+--------------------+
共返回 1 行记录,花费 43200 ms.
mysql>select count(*) from (select user_key,SUM(CASE WHEN card_level = 5 THEN 0
ELSE 1 END) AS a from fx_achievement where pay_date between date_sub('2017-10-10 00:00:00',interval 30 day)
and date_sub('2017-10-10 00:00:00',interval 1 day) group by user_key having a=0 and sum(sale) >= 2500) as t1;
+--------------------+
| count(*)           |
+--------------------+
| 6748               |
+--------------------+
共返回 1 行记录,花费 42455 ms.
```
