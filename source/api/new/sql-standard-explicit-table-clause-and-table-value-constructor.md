---
title: MySQL 8.0的新增功能探索-SQL标准的显式表子句和表值构造函数
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

- **SQL标准的显式表子句和表值构造函数。** 根据SQL标准添加了表值构造函数和显式表子句。这些分别在MySQL 8.0.19中作为 [`TABLE`](https://dev.mysql.com/doc/refman/8.0/en/table.html)语句和 [`VALUES`](https://dev.mysql.com/doc/refman/8.0/en/values.html)语句实现。

该[`TABLE`](https://dev.mysql.com/doc/refman/8.0/en/table.html)语句具有格式，并且等效于。它支持 和 子句（后者带有optional ），但不允许选择单个表列。 可以在您使用等效 语句的任何地方使用；这包括连接，联合， ，， 语句和子查询。例如： `TABLE *`table_name`*``SELECT * FROM *`table_name`*``ORDER BY``LIMIT``OFFSET``TABLE`[`SELECT`](https://dev.mysql.com/doc/refman/8.0/en/select.html)[`INSERT ... SELECT`](https://dev.mysql.com/doc/refman/8.0/en/insert-select.html)[`REPLACE`](https://dev.mysql.com/doc/refman/8.0/en/replace.html)[`CREATE TABLE ... SELECT`](https://dev.mysql.com/doc/refman/8.0/en/create-table-select.html)

- `TABLE t1 UNION TABLE t2` 相当于 `SELECT * FROM t1 UNION SELECT * FROM t2`
- `CREATE TABLE t2 TABLE t1` 相当于 `CREATE TABLE t2 SELECT * FROM t1`
- `SELECT a FROM t1 WHERE b > ANY (TABLE t2)`等同于`SELECT a FROM t1 WHERE b > ANY (SELECT * FROM t2)`。

  [`VALUES`](https://dev.mysql.com/doc/refman/8.0/en/values.html)可用于一个表值提供给一个[`INSERT`](https://dev.mysql.com/doc/refman/8.0/en/insert.html)， [`REPLACE`](https://dev.mysql.com/doc/refman/8.0/en/replace.html)或 [`SELECT`](https://dev.mysql.com/doc/refman/8.0/en/select.html)语句，和由的`VALUES`关键字随后进行了一系列行构造（的`ROW()`）由逗号分隔。例如，该语句 `INSERT INTO t1 VALUES ROW(1,2,3), ROW(4,5,6), ROW(7,8,9)`提供与SQL兼容的等效于MySQL特定的`INSERT INTO t1 VALUES (1,2,3), (4,5,6), (7,8,9)`。您还可以像选择[`VALUES`](https://dev.mysql.com/doc/refman/8.0/en/values.html)表一样从表值构造函数中进行选择 ，请记住，这样做时必须提供表别名，并且必须[`SELECT`](https://dev.mysql.com/doc/refman/8.0/en/select.html)像使用其他别名一样使用它 。这包括联接，联合和子查询。

  有关详细信息`TABLE`，并 `VALUES`和其使用的示例，请参阅本文档的以下部分：

- [第13.2.12节" TABLE语句"](https://dev.mysql.com/doc/refman/8.0/en/table.html)

- [第13.2.14节" VALUES语句"](https://dev.mysql.com/doc/refman/8.0/en/values.html)
- [第13.1.20.4节" CREATE TABLE ... SELECT语句"](https://dev.mysql.com/doc/refman/8.0/en/create-table-select.html)
- [第13.2.6.1节" INSERT ... SELECT语句"](https://dev.mysql.com/doc/refman/8.0/en/insert-select.html)
- [第13.2.10.2节" JOIN子句"](https://dev.mysql.com/doc/refman/8.0/en/join.html)
- [第13.2.11节"子查询"](https://dev.mysql.com/doc/refman/8.0/en/subqueries.html)
- [第13.2.10.3节" UNION子句"](https://dev.mysql.com/doc/refman/8.0/en/union.html)
