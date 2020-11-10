---
title: MySQL 8.0的新增功能探索-FORCE INDEX，IGNORE INDEX的优化程序提示
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

- **FORCE INDEX，IGNORE INDEX的优化程序提示。** MySQL 8.0引入了索引级优化器提示，这些提示与[第8.9.4节"索引提示"中](https://dev.mysql.com/doc/refman/8.0/en/index-hints.html)所述的传统索引提示类似。新的提示这里列出，与他们一起`FORCE INDEX`或`IGNORE INDEX` 等价物：

  - [`GROUP_INDEX`](https://dev.mysql.com/doc/refman/8.0/en/optimizer-hints.html#optimizer-hints-index-level)： 相当于 `FORCE INDEX FOR GROUP BY`

    [`NO_GROUP_INDEX`](https://dev.mysql.com/doc/refman/8.0/en/optimizer-hints.html#optimizer-hints-index-level)： 相当于 `IGNORE INDEX FOR GROUP BY`

  - [`JOIN_INDEX`](https://dev.mysql.com/doc/refman/8.0/en/optimizer-hints.html#optimizer-hints-index-level)： 相当于 `FORCE INDEX FOR JOIN`

    [`NO_JOIN_INDEX`](https://dev.mysql.com/doc/refman/8.0/en/optimizer-hints.html#optimizer-hints-index-level)： 相当于 `IGNORE INDEX FOR JOIN`

  - [`ORDER_INDEX`](https://dev.mysql.com/doc/refman/8.0/en/optimizer-hints.html#optimizer-hints-index-level)： 相当于 `FORCE INDEX FOR ORDER BY`

    [`NO_ORDER_INDEX`](https://dev.mysql.com/doc/refman/8.0/en/optimizer-hints.html#optimizer-hints-index-level)： 相当于 `IGNORE INDEX FOR ORDER BY`

  - [`INDEX`](https://dev.mysql.com/doc/refman/8.0/en/optimizer-hints.html#optimizer-hints-index-level)：与[`GROUP_INDEX`](https://dev.mysql.com/doc/refman/8.0/en/optimizer-hints.html#optimizer-hints-index-level)加 [`JOIN_INDEX`](https://dev.mysql.com/doc/refman/8.0/en/optimizer-hints.html#optimizer-hints-index-level)号 相同 [`ORDER_INDEX`](https://dev.mysql.com/doc/refman/8.0/en/optimizer-hints.html#optimizer-hints-index-level); 等同于`FORCE INDEX`没有修饰符

    [`NO_INDEX`](https://dev.mysql.com/doc/refman/8.0/en/optimizer-hints.html#optimizer-hints-index-level)：与[`NO_GROUP_INDEX`](https://dev.mysql.com/doc/refman/8.0/en/optimizer-hints.html#optimizer-hints-index-level)加 [`NO_JOIN_INDEX`](https://dev.mysql.com/doc/refman/8.0/en/optimizer-hints.html#optimizer-hints-index-level)号 相同 [`NO_ORDER_INDEX`](https://dev.mysql.com/doc/refman/8.0/en/optimizer-hints.html#optimizer-hints-index-level); 等同于`IGNORE INDEX`没有修饰符

  例如，以下两个查询是等效的：

  ```sql
  SELECT a FROM t1 FORCE INDEX (i_a) FOR JOIN WHERE a=1 AND b=2;

  SELECT /*+ JOIN_INDEX(t1 i_a) */ a FROM t1 WHERE a=1 AND b=2;
  ```

  前面列出的优化器提示在语法和用法上与现有索引级优化器提示遵循相同的基本规则。

  这些优化器提示旨在替换`FORCE INDEX`和`IGNORE INDEX`，我们计划在将来的MySQL版本中弃用和，然后从MySQL中删除。他们没有实现的单个精确等效项`USE INDEX`；相反，你可以使用一个或多个 [`NO_INDEX`](https://dev.mysql.com/doc/refman/8.0/en/optimizer-hints.html#optimizer-hints-index-level)， [`NO_JOIN_INDEX`](https://dev.mysql.com/doc/refman/8.0/en/optimizer-hints.html#optimizer-hints-index-level)， [`NO_GROUP_INDEX`](https://dev.mysql.com/doc/refman/8.0/en/optimizer-hints.html#optimizer-hints-index-level)，或 [`NO_ORDER_INDEX`](https://dev.mysql.com/doc/refman/8.0/en/optimizer-hints.html#optimizer-hints-index-level)达到同样的效果。

  有关更多信息和使用示例，请参见 [索引级优化器提示](https://dev.mysql.com/doc/refman/8.0/en/optimizer-hints.html#optimizer-hints-index-level)。
