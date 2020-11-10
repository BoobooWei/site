---
title: MySQL 8.0的新增功能探索-EXPLAIN ANALYZE语句
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

**EXPLAIN ANALYZE语句。** MySQL 8.0.18中实现了 一种新形式的[`EXPLAIN`](https://dev.mysql.com/doc/refman/8.0/en/explain.html) 语句，它以处理查询所使用的每个迭代器的格式[`EXPLAIN ANALYZE`](https://dev.mysql.com/doc/refman/8.0/en/explain.html#explain-analyze)提供了有关[`SELECT`](https://dev.mysql.com/doc/refman/8.0/en/select.html)语句 执行的扩展信息 `TREE`，并使得可以将估计成本与查询的实际成本进行比较。该信息包括启动成本，总成本，此迭代器返回的行数以及执行的循环数。

在MySQL 8.0.20和更高版本中，该语句还支持 `FORMAT=TREE`说明符。 `TREE`是唯一受支持的格式。

有关更多信息，请参见[使用EXPLAIN ANALYZE获取](https://dev.mysql.com/doc/refman/8.0/en/explain.html#explain-analyze)信息。
