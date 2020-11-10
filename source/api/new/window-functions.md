---
title: MySQL 8.0的新增功能探索-窗口函数
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

- **窗口功能。** MySQL现在支持窗口函数，对于查询的每一行，都使用与该行相关的行来执行计算。这些包括诸如 [`RANK()`](https://dev.mysql.com/doc/refman/8.0/en/window-function-descriptions.html#function_rank)， [`LAG()`](https://dev.mysql.com/doc/refman/8.0/en/window-function-descriptions.html#function_lag)，和 [`NTILE()`](https://dev.mysql.com/doc/refman/8.0/en/window-function-descriptions.html#function_ntile)。此外，现在可以将几个现有的聚合函数用作窗口函数（例如 [`SUM()`](https://dev.mysql.com/doc/refman/8.0/en/group-by-functions.html#function_sum)和 [`AVG()`](https://dev.mysql.com/doc/refman/8.0/en/group-by-functions.html#function_avg)）。有关更多信息，请参见[第12.21节"窗口函数"](https://dev.mysql.com/doc/refman/8.0/en/window-functions.html)。
