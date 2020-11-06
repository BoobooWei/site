---
title: 2020.08.11.xx业务.SQL优化
---

# 前情概要

5条业务待上线SQL语句，测试过程中发现有1条SQL语句有优化的余地，为此将SQL优化的建议给出，供业务参考。


# 待优化SQL语句

```sql
select p.id,d.id order_id,p.buyer_id,d.pre_qty,p.total_qty,d.create_date,d.order_date,p.order_num
from (select d.order_num,d.id,d.pre_qty,order_date create_date,upload_date order_date,terminal_num,order_status from vgp_order d where 1=1   and d.upload_date>=1534262400  and d.upload_date<=1534953599 ) d
inner join (select address,terminal_num,terminal_group,parent_id from vgp_terminal_config c where 1=1 ) c  on c.terminal_num=d.terminal_num
left join (select id,order_num,buyer_id,total_qty,other_order_num,pay_way,total_price,pay_status,locking,cash_earning,cash_refund from vgp_payment p where 1=1  and p.create_date>=1534262400  and p.create_date<=1534953599   ) p on   d.order_num=p.order_num
where 1=1  order by d.order_date desc
```

# SQL优化过程

## 获取 SQL 中涉及的库表的基础信息

## 使用Explain 分析 SQL 语句执行计划

## 分析 SQL 语句执行时间和消耗资源

## 优化索引

## 改写SQL


# 总结

## SQL执行效率对比

| 测试次数 | 原SQL      | 优化后    |
| :------- | :--------- | :-------- |
| 1        | 23.703 sec | 0.032 sec |
| 2        | 23.412 sec | 0.031 sec |
| 3        | 23.702 sec | 0.032 sec |

## 优化手段和建议

1. SQL改写

```sql
SELECT
   p.id,
   d.id order_id,
   p.buyer_id,
   d.pre_qty,
   p.total_qty,
   d.order_date create_date,
   d.upload_date order_date,
   p.order_num
    FROM
       vgp_order d
  INNER JOIN vgp_terminal_config c on c.terminal_num = d.terminal_num and d.upload_date >= 1534262400 AND d.upload_date <= 1534953599
  left join vgp_payment p on d.order_num = p.order_num and p.create_date >= 1534262400 AND p.create_date <= 1534953599
 ORDER BY create_date DESC
```


2. 建议

1. 将改写后的SQL在测试环境中进行业务验证，若通过则可进行业务变更。
2. 后续需注意嵌套子查询的执行效率。


