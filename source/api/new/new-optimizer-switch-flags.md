---
title: MySQL 8.0的新增功能探索-新的optimizer_switch标志
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

- **新的optimizer_switch标志。** MySQL 8.0.21为[`optimizer_switch`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_optimizer_switch)系统变量添加了两个新标志， 如下表所示：

  - `prefer_ordering_index` 旗

    默认情况下， 只要优化器确定这将导致更快的执行速度，MySQL就会尝试对具有子句的任何查询`ORDER BY`或`GROUP BY`查询使用有序索引`LIMIT`。由于在某些情况下为此类查询选择不同的优化实际上可能会更好地执行，因此现在可以通过将`prefer_ordering_index`标志 设置为来禁用此优化 `off`。

    此标志的默认值为 `on`。

  - `subquery_to_derived` 旗

    当此标志设置`on`为时，优化程序将合格的标量子查询转换为派生表上的联接。例如，查询 `SELECT * FROM t1 WHERE t1.a > (SELECT COUNT(a) FROM t2)`被重写为 `SELECT t1.a FROM t1 JOIN ( SELECT COUNT(t2.a) AS c FROM t2 ) AS d WHERE t1.a > d.c`。

    这种优化可以应用到子查询其是的一部分`SELECT`， `WHERE`，`JOIN`，或 `HAVING`条款; 包含一个或多个聚合函数，但没有`GROUP BY` 子句；不相关 并且不使用任何不确定的函数。

    优化也可应用于表子查询这对参数`IN`， `NOT IN`，`EXISTS`，或 `NOT EXISTS`，并且其不包含`GROUP BY`。例如，查询`SELECT * FROM t1 WHERE t1.b < 0 OR t1.a IN (SELECT t2.a + 1 FROM t2)`被重写为`SELECT a, b FROM t1 LEFT JOIN (SELECT DISTINCT 1 AS e1, t2.a AS e2 FROM t2) d ON t1.a + 1 = d.e2 WHERE t1.b < 0 OR d.e1 IS NOT NULL`。

    通常禁用此优化，因为它在大多数情况下不会产生明显的性能优势，因此`off`默认情况下将标志设置为。

  有关更多信息，请参见 [第8.9.2节"可切换的优化"](https://dev.mysql.com/doc/refman/8.0/en/switchable-optimizations.html)。另请参见 [第8.2.1.19节" LIMIT查询优化"](https://dev.mysql.com/doc/refman/8.0/en/limit-optimization.html)， [第8.2.2.1节"使用半联接转换优化IN和EXISTS子查询谓词"](https://dev.mysql.com/doc/refman/8.0/en/semijoins.html)和 [第8.2.2.4节"通过合并优化派生表，视图引用和公用表表达式"或物化"](https://dev.mysql.com/doc/refman/8.0/en/derived-table-optimization.html)。
