---
title: MySQL 8.0的新增功能探索-优化器
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

- **优化器。** 添加了这些优化器增强功能：- MySQL现在支持不可见索引。优化器根本不会使用不可见的索引，但否则它会正常维护。默认情况下，索引可见。不可见的索引使测试删除索引对查询性能的影响成为可能，而无需进行破坏性的更改，如果确实需要索引，则必须撤消该更改。请参见 [第8.3.12节"不可见索引"](https://dev.mysql.com/doc/refman/8.0/en/invisible-indexes.html)。

  - MySQL现在支持降序索引： `DESC`索引定义不再被忽略，而是导致键值以降序存储。以前，索引可以以相反的顺序进行扫描，但会降低性能。降序索引可以按正向顺序进行扫描，这样效率更高。当最有效的扫描顺序混合某些列的升序和其他列的降序时，降序索引还使优化程序可以使用多列索引。请参见[第8.3.13节"降序索引"](https://dev.mysql.com/doc/refman/8.0/en/descending-indexes.html)。

  - MySQL现在支持创建索引表达式值而不是列值的功能索引键部分。功能性关键部分支持对无法通过其他方式索引的值（例如[`JSON`](https://dev.mysql.com/doc/refman/8.0/en/json.html)值）进行索引 。有关详细信息，请参见[第13.1.15节" CREATE INDEX语句"](https://dev.mysql.com/doc/refman/8.0/en/create-index.html)。

  - 在MySQL 8.0.14和更高版本中，`WHERE`在准备过程中而不是在优化过程中删除了常量文字表达式引起的琐碎 条件。在过程的早期删除条件可以简化具有琐碎条件的外部联接的查询的联接，例如：

    ```sql
    SELECT * FROM t1 LEFT JOIN t2 ON condition_1 WHERE condition_2 OR 0 = 1
    ```

    现在，优化器在准备过程中会看到0 = 1始终为false，从而使其成为`OR 0 = 1` 冗余，然后将其删除，从而保持以下状态：

    ```sql
    SELECT * FROM t1 LEFT JOIN t2 ON condition_1 where condition_2
    ```

    现在，优化器可以将查询重写为内部联接，如下所示：

    ```sql
    SELECT * FROM t1 LEFT JOIN t2 WHERE condition_1 AND condition_2
    ```

    有关更多信息，请参见 [第8.2.1.9节"外部联接优化"](https://dev.mysql.com/doc/refman/8.0/en/outer-join-optimization.html)。

  - 在MySQL 8.0.16及更高版本中，MySQL可以在优化时使用常量折叠来处理列与常量值之间的比较，其中常量超出范围或相对于列的类型在范围边界上，而不是执行因此对于执行时的每一行。例如，给定一个`t`带有`TINYINT UNSIGNED`列的表 `c`，优化器可以重写条件，例如`WHERE c < 256`to `WHERE 1`（并完全优化该条件）或 `WHERE c >= 255`to `WHERE c = 255`。

    有关更多信息[，](https://dev.mysql.com/doc/refman/8.0/en/constant-folding-optimization.html)请参见[第8.2.1.14节"恒定折叠优化"](https://dev.mysql.com/doc/refman/8.0/en/constant-folding-optimization.html)。

  - 从MySQL 8.0.16开始，与`IN`子查询一起使用的半联接优化现在也可以应用于`EXISTS`子查询。另外，优化器现在在`WHERE`附加到子查询的条件中对琐碎相关的相等谓词进行解相关 ，以便可以将它们与子查询中的表达式进行类似的处理`IN`。这适用于`EXISTS`和 `IN`子查询。

    有关更多信息，请参见[第8.2.2.1节"使用半联接转换优化IN和EXISTS子查询谓词"](https://dev.mysql.com/doc/refman/8.0/en/semijoins.html)。

  - 在MySQL 8.0.17及更高版本中，`WHERE` 具有或 的条件在内部转换为反连接。（一个反联接返回表中没有与联接条件相匹配的行的表中的所有行，并且符合联接条件。）这将删除子查询，因为该子查询的表现在在顶部处理，因此可以更快地执行查询。水平。 `NOT IN (*`subquery`*)``NOT EXISTS (*`subquery`*)`

    这类似于并重用现有的`IS NULL`（`Not exists`）外连接优化。请参阅 [EXPLAIN Extra Information](https://dev.mysql.com/doc/refman/8.0/en/explain-output.html#explain-extra-information)。
