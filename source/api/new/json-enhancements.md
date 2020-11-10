---
title: MySQL 8.0的新增功能探索-Json增强功能
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

- **JSON增强。** 对MySQL的JSON功能进行了以下增强或添加：- 添加了 [`->>`](https://dev.mysql.com/doc/refman/8.0/en/json-search-functions.html#operator_json-inline-path) （内联路径）运算符，等效于调用 [`JSON_UNQUOTE()`](https://dev.mysql.com/doc/refman/8.0/en/json-modification-functions.html#function_json-unquote)的结果[`JSON_EXTRACT()`](https://dev.mysql.com/doc/refman/8.0/en/json-search-functions.html#function_json-extract)。

  这是[`->`](https://dev.mysql.com/doc/refman/8.0/en/json-search-functions.html#operator_json-column-path) 对MySQL 5.7中引入的列路径运算符的改进 ； `col->>"$.path"`等同于 `JSON_UNQUOTE(col->"$.path")`。内联路径运算符可以用来随时随地可以使用 `JSON_UNQUOTE(JSON_EXTRACT())`，如 [`SELECT`](https://dev.mysql.com/doc/refman/8.0/en/select.html)列清单， `WHERE`和`HAVING` 条款，并`ORDER BY`和 `GROUP BY`条款。有关更多信息，请参见运算符的描述以及[JSON Path Syntax](https://dev.mysql.com/doc/refman/8.0/en/json.html#json-path-syntax)。

  - 添加了两个JSON聚合函数 [`JSON_ARRAYAGG()`](https://dev.mysql.com/doc/refman/8.0/en/group-by-functions.html#function_json-arrayagg)和 [`JSON_OBJECTAGG()`](https://dev.mysql.com/doc/refman/8.0/en/group-by-functions.html#function_json-objectagg)。 `JSON_ARRAYAGG()`将列或表达式作为其参数，并将结果聚合为单个[`JSON`](https://dev.mysql.com/doc/refman/8.0/en/json.html)数组。该表达式可以求值为任何MySQL数据类型；这不一定是一个`JSON`值。 `JSON_OBJECTAGG()`接受两列或表达式，将其解释为键和值；它将结果作为单个`JSON` 对象返回。有关更多信息和示例，请参见 [第12.20节"聚合（GROUP BY）函数"](https://dev.mysql.com/doc/refman/8.0/en/group-by-functions-and-modifiers.html)。

  - 添加了JSON实用程序功能[`JSON_PRETTY()`](https://dev.mysql.com/doc/refman/8.0/en/json-utility-functions.html#function_json-pretty)，该功能 [`JSON`](https://dev.mysql.com/doc/refman/8.0/en/json.html) 以易于阅读的格式输出现有值；每个JSON对象成员或数组值都打印在单独的一行上，并且子对象或数组相对于其父对象要有2个空格。

    此函数还可以与可解析为JSON值的字符串一起使用。

    有关更多详细信息和示例，请参见 [第12.17.8节" JSON实用程序函数"](https://dev.mysql.com/doc/refman/8.0/en/json-utility-functions.html)。

  - 现在，[`JSON`](https://dev.mysql.com/doc/refman/8.0/en/json.html)使用来对查询中的值进行 排序时`ORDER BY`，每个值现在都由sort键的可变长度部分表示，而不是由固定的1K大小的一部分表示。在许多情况下，这可以减少过多的使用。例如，标量`INT`甚至 `BIGINT`值实际上需要很少的字节，因此该空间的其余部分（最多90％或更多）被填充占用。此更改具有以下性能优势：

    - 现在可以更有效地使用排序缓冲区空间，因此文件排序不需要像固定长度排序键那样早或经常刷新到磁盘。这意味着可以在内存中整理更多数据，避免不必要的磁盘访问。
    - 比起较长的键，可以更快地比较较短的键，从而显着提高性能。对于完全在内存中执行的排序以及需要写入磁盘和从磁盘读取的排序，都是如此。

  - 在MySQL 8.0.2中添加了对`JSON`列值的部分就地更新的支持，这比完全删除现有JSON值并在其位置写入一个新的JSON效率更高，就像以前在更新任何`JSON`列时所做的那样 。要应用这种优化，更新，必须使用应用 [`JSON_SET()`](https://dev.mysql.com/doc/refman/8.0/en/json-modification-functions.html#function_json-set)， [`JSON_REPLACE()`](https://dev.mysql.com/doc/refman/8.0/en/json-modification-functions.html#function_json-replace)或 [`JSON_REMOVE()`](https://dev.mysql.com/doc/refman/8.0/en/json-modification-functions.html#function_json-remove)。无法将新元素添加到要更新的JSON文档中；文档中的值不能占用比更新前更多的空间。请参阅 [JSON值的部分更新](https://dev.mysql.com/doc/refman/8.0/en/json.html#json-partial-updates)，以详细讨论要求。

    可以将JSON文档的部分更新写入二进制日志，比记录完整的JSON文档占用更少的空间。使用基于语句的复制时，始终会记录部分更新。为了使其与基于行的复制一起使用，必须首先设置 [`binlog_row_value_options=PARTIAL_JSON`](https://dev.mysql.com/doc/refman/8.0/en/replication-options-binary-log.html#sysvar_binlog_row_value_options); 有关更多信息，请参见此变量的说明。

  - 添加了JSON实用程序功能 [`JSON_STORAGE_SIZE()`](https://dev.mysql.com/doc/refman/8.0/en/json-utility-functions.html#function_json-storage-size)和 [`JSON_STORAGE_FREE()`](https://dev.mysql.com/doc/refman/8.0/en/json-utility-functions.html#function_json-storage-free)。 `JSON_STORAGE_SIZE()`在进行任何部分更新之前，返回用于JSON文档的二进制表示形式的存储空间（以字节为单位）（请参阅上一项）。 `JSON_STORAGE_FREE()`显示[`JSON`](https://dev.mysql.com/doc/refman/8.0/en/json.html)使用`JSON_SET()`或 部分更新的类型的表列中剩余的空间量 `JSON_REPLACE()`。如果新值的二进制表示形式小于先前值的二进制表示形式，则该值大于零。

    每个函数还接受JSON文档的有效字符串表示形式。对于这样的值， `JSON_STORAGE_SIZE()`返回其转换为JSON文档后其二进制表示形式使用的空间。对于包含JSON文档的字符串表示形式的变量， `JSON_STORAGE_FREE()`返回零。如果无法将其（非null）参数解析为有效的JSON文档，并且`NULL`参数为，则 任何一个函数都会产生错误 `NULL`。

    有关更多信息和示例，请参见 [第12.17.8节" JSON实用程序函数"](https://dev.mysql.com/doc/refman/8.0/en/json-utility-functions.html)。

    `JSON_STORAGE_SIZE()`并 `JSON_STORAGE_FREE()`在MySQL 8.0.2中实现。

  - 在MySQL 8.0.2中添加了对范围（例如 `$[1 to 5]`XPath表达式）的支持。在此版本中还添加了对 `last`关键字和相对寻址的支持，因此`$[last]`始终选择数组中的最后一个（最高编号）元素以及 `$[last-1]`最后一个相邻元素。 `last`使用它的表达式也可以包含在范围定义中。例如， `$[last-2 to last-1]`返回最后两个元素，但返回数组中的一个。有关其他信息和示例，请参见 [搜索和修改JSON值](https://dev.mysql.com/doc/refman/8.0/en/json.html#json-paths)。

  - 添加了旨在符合[RFC 7396](https://tools.ietf.org/html/rfc7396)的JSON合并功能 。 [`JSON_MERGE_PATCH()`](https://dev.mysql.com/doc/refman/8.0/en/json-modification-functions.html#function_json-merge-patch)，当用于2个JSON对象时，将它们合并为一个具有以下集合的并集的单个JSON对象：

    - 第一个对象的每个成员，在第二个对象中不存在具有相同键的成员。
    - 第二个对象的每个成员，在第一个对象中没有成员具有相同的键，并且其值不是JSON `null`文字。
    - 每个成员都具有在两个对象中都存在的键，并且其在第二个对象中的值不是JSON `null`文字。

    作为这项工作的一部分，该 [`JSON_MERGE()`](https://dev.mysql.com/doc/refman/8.0/en/json-modification-functions.html#function_json-merge)功能已重命名 [`JSON_MERGE_PRESERVE()`](https://dev.mysql.com/doc/refman/8.0/en/json-modification-functions.html#function_json-merge-preserve)。 `JSON_MERGE()`仍然被认为是`JSON_MERGE_PRESERVE()`MySQL 8.0 的别名 ，但现在已被弃用，并且可能在将来的MySQL版本中删除。

    有关更多信息和示例，请参见 [第12.17.4节"修改JSON值的函数"](https://dev.mysql.com/doc/refman/8.0/en/json-modification-functions.html)。

  - 实现重复密钥的 " 最后重复密钥获胜 " 规范化，与 [RFC 7159](https://tools.ietf.org/html/rfc7159)和大多数JavaScript解析器一致。此行为的示例在此处显示，其中仅`x`保留具有密钥的最右边的成员：

    ```sql
    mysql> SELECT JSON_OBJECT('x', '32', 'y', '[true, false]',
         >                     'x', '"abc"', 'x', '100') AS Result;
    +------------------------------------+
    | Result                             |
    +------------------------------------+
    | {"x": "100", "y": "[true, false]"} |
    +------------------------------------+
    1 row in set (0.00 sec)
    ```

    插入MySQL [`JSON`](https://dev.mysql.com/doc/refman/8.0/en/json.html)列中的值 也以这种方式标准化，如以下示例所示：

    ```sql
    mysql> CREATE TABLE t1 (c1 JSON);

    mysql> INSERT INTO t1 VALUES ('{"x": 17, "x": "red", "x": [3, 5, 7]}');

    mysql> SELECT c1 FROM t1;
    +------------------+
    | c1               |
    +------------------+
    | {"x": [3, 5, 7]} |
    +------------------+
    ```

    与以前的MySQL版本相比，这是一个不兼容的更改， 在这种情况下，使用了" 首次重复键赢 "算法。

    有关更多信息和示例[，](https://dev.mysql.com/doc/refman/8.0/en/json.html#json-normalization)请参见[JSON值的规范化，合并和自动包装](https://dev.mysql.com/doc/refman/8.0/en/json.html#json-normalization)。

  - [`JSON_TABLE()`](https://dev.mysql.com/doc/refman/8.0/en/json-table-functions.html#function_json-table) 在MySQL 8.0.4中 添加了该功能。此函数接受JSON数据，并将其作为具有指定列的关系表返回。

    该函数的语法为 ，其中 是返回JSON数据的表达式，是应用于源的JSON路径以及 列定义的列表。这里显示一个示例： `JSON_TABLE(*`expr`*, *`path`* COLUMNS *`column_list`*) [AS] *`alias`*)`_`expr`**`path`**`column_list`_

    ```sql
    mysql> SELECT *
        -> FROM
        ->   JSON_TABLE(
        ->     '[{"a":3,"b":"0"},{"a":"3","b":"1"},{"a":2,"b":1},{"a":0},{"b":[1,2]}]',
        ->     "$[*]" COLUMNS(
        ->       rowid FOR ORDINALITY,
        ->
        ->       xa INT EXISTS PATH "$.a",
        ->       xb INT EXISTS PATH "$.b",
        ->
        ->       sa VARCHAR(100) PATH "$.a",
        ->       sb VARCHAR(100) PATH "$.b",
        ->
        ->       ja JSON PATH "$.a",
        ->       jb JSON PATH "$.b"
        ->     )
        ->   ) AS  jt1;
    +-------+------+------+------+------+------+--------+
    | rowid | xa   | xb   | sa   | sb   | ja   | jb     |
    +-------+------+------+------+------+------+--------+
    |     1 |    1 |    1 | 3    | 0    | 3    | "0"    |
    |     2 |    1 |    1 | 3    | 1    | "3"  | "1"    |
    |     3 |    1 |    1 | 2    | 1    | 2    | 1      |
    |     4 |    1 |    0 | 0    | NULL | 0    | NULL   |
    |     5 |    0 |    1 | NULL | NULL | NULL | [1, 2] |
    +-------+------+------+------+------+------+--------+
    ```

    JSON源表达式可以是产生有效JSON文档的任何表达式，包括JSON文字，表列或返回JSON的函数调用，例如[`JSON_EXTRACT(t1, data, '$.post.comments')`](https://dev.mysql.com/doc/refman/8.0/en/json-search-functions.html#function_json-extract)。有关更多信息，请参见 [第12.17.6节" JSON表函数"](https://dev.mysql.com/doc/refman/8.0/en/json-table-functions.html)。
