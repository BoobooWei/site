---
title: MySQL 8.0的新增功能探索-常用表表达式
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

- **常用表表达式。** MySQL现在支持通用表表达式，包括非递归和递归的。公用表表达式允许使用命名的临时结果集，通过允许在[`WITH`](https://dev.mysql.com/doc/refman/8.0/en/with.html)语句之前的子句[`SELECT`](https://dev.mysql.com/doc/refman/8.0/en/select.html)和某些其他语句来实现。有关更多信息，请参见 [第13.2.15节" WITH（公用表表达式）"](https://dev.mysql.com/doc/refman/8.0/en/with.html)。从MySQL 8.0.19开始，[`SELECT`](https://dev.mysql.com/doc/refman/8.0/en/select.html)递归公用表表达式（CTE）的递归 部分支持 `LIMIT`子句。`LIMIT` 与`OFFSET`也支持。有关更多信息，请参见 [递归公用表表达式](https://dev.mysql.com/doc/refman/8.0/en/with.html#common-table-expressions-recursive)。
