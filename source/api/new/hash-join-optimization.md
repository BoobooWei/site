---
title: MySQL 8.0的新增功能探索-哈希联接优化
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

**哈希联接优化。** 从MySQL 8.0.18开始，只要联接中的每对表都至少包含一个等联接条件，就使用哈希联接。哈希联接不需要索引，并且在大多数情况下比块嵌套循环算法更有效。可以通过这种方式优化如此处所示的联接：

```sql
SELECT *
    FROM t1
    JOIN t2
        ON t1.c1=t2.c1;

SELECT *
    FROM t1
    JOIN t2
        ON (t1.c1 = t2.c1 AND t1.c2 < t2.c2)
    JOIN t3
        ON (t2.c1 = t3.c1)
```

哈希联接也可以用于笛卡尔积-即，未指定联接条件时。

您可以使用[`EXPLAIN FORMAT=TREE`](https://dev.mysql.com/doc/refman/8.0/en/explain.html)或查看何时将哈希联接优化用于特定查询 `EXPLAIN ANALYZE`。

可以使用[`optimizer_switch`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_optimizer_switch)系统变量的`hash_join`标记（`on`默认情况下）以及 [`HASH_JOIN`](https://dev.mysql.com/doc/refman/8.0/en/optimizer-hints.html#optimizer-hints-table-level)和 [`NO_HASH_JOIN`](https://dev.mysql.com/doc/refman/8.0/en/optimizer-hints.html#optimizer-hints-table-level)优化程序提示来控制哈希联接的使用 。

哈希联接可用的内存量受的值限制 [`join_buffer_size`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_join_buffer_size)。在磁盘上执行需要更多内存的哈希联接；磁盘上的哈希联接可以使用的磁盘文件数受限制 [`open_files_limit`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_open_files_limit)。

从MySQL 8.0.19开始，MySQL 8.0.18中`hash_join` 引入的优化器开关不再起作用。这也适用于 [`HASH_JOIN`](https://dev.mysql.com/doc/refman/8.0/en/optimizer-hints.html#optimizer-hints-table-level)和 `NO_HASH_JOIN`优化器提示。开关和提示现在都已弃用，并将在将来的MySQL版本中删除。

在MySQL 8.0.20中，即使在查询不包含等联接条件的情况下，只要使用块嵌套循环，都将使用哈希联接。这适用于内部非等联接，半联接，反联接，左外部联接和右外部联接。此外，内部联接和外部联接（包括半联接和反联接）现在都可以使用批处理密钥访问（BKA），该批处理密钥访问（BKA）递增地分配联接缓冲内存，这样单个查询就不需要消耗解析所需的大量资源。 。从MySQL 8.0.18开始仅支持内部联接的BKA。

MySQL 8.0.20还用迭代器执行器替换了以前版本的MySQL中使用的执行器。这项工作包括为那些尚未优化为半联接的查询替换管理该格式查询的旧索引子查询引擎，以及以以前依赖于旧执行程序的相同形式实现的查询。 `WHERE *`value`* IN (SELECT *`column`* FROM *`table`* WHERE ...)``IN`

有关更多信息和示例，请参见 [第8.2.1.4节"哈希联接优化"](https://dev.mysql.com/doc/refman/8.0/en/hash-joins.html)。另请参阅 [批处理密钥访问联接](https://dev.mysql.com/doc/refman/8.0/en/bnl-bka-optimization.html#bka-optimization)。
