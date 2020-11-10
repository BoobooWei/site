---
title: MySQL 8.0的新增功能探索-多值索引
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

**多值索引。** 从MySQL 8.0.17开始， [`InnoDB`](https://dev.mysql.com/doc/refman/8.0/en/innodb-storage-engine.html)支持创建多值索引，该索引是在[`JSON`](https://dev.mysql.com/doc/refman/8.0/en/json.html)存储值数组的列上定义的辅助索引，并且单个数据记录可以具有多个索引记录。这样的索引使用诸如的关键部分定义 `CAST(data->'$.zipcode' AS UNSIGNED ARRAY)`。MySQL优化程序会自动使用多值索引进行合适的查询，如的输出所示 [`EXPLAIN`](https://dev.mysql.com/doc/refman/8.0/en/explain.html)。

作为这项工作的一部分，MySQL添加了一个新功能 [`JSON_OVERLAPS()`](https://dev.mysql.com/doc/refman/8.0/en/json-search-functions.html#function_json-overlaps)和一个[`MEMBER OF()`](https://dev.mysql.com/doc/refman/8.0/en/json-search-functions.html#operator_member-of)用于处理[`JSON`](https://dev.mysql.com/doc/refman/8.0/en/json.html)文档的新 运算符，此外，还[`CAST()`](https://dev.mysql.com/doc/refman/8.0/en/cast-functions.html#function_cast)使用一个新`ARRAY`关键字扩展了该 功能， 如下表所示：

- `JSON_OVERLAPS()`比较两个 [`JSON`](https://dev.mysql.com/doc/refman/8.0/en/json.html)文档。如果它们包含任何共同的键值对或数组元素，则该函数返回TRUE（1）; 否则返回FALSE（0）。如果两个值都是标量，则该函数将执行一个简单的相等性测试。如果一个参数是JSON数组，另一个参数是标量，则将标量视为数组元素。因此，可 `JSON_OVERLAPS()`作为的补充[`JSON_CONTAINS()`](https://dev.mysql.com/doc/refman/8.0/en/json-search-functions.html#function_json-contains)。
- `MEMBER OF()`测试第一个操作数（标量或JSON文档）是否是作为第二个操作数传递的JSON数组的成员，如果是则返回TRUE（1），否则返回FALSE（0）。不执行操作数的类型转换。
- `CAST(expression AS type ARRAY)`允许通过将在JSON文档中找到的JSON数组_`json_path`_转换为SQL数组来创建功能索引 。类型说明符仅限于由已经支持的那些`CAST()`，以除外 `BINARY`（不支持）。`CAST()`（和 `ARRAY`关键字）的这种用法 仅受支持 [`InnoDB`](https://dev.mysql.com/doc/refman/8.0/en/innodb-storage-engine.html)，并且仅用于创建多值索引。

有关多值索引的详细信息（包括示例），请参见 [多值索引](https://dev.mysql.com/doc/refman/8.0/en/create-index.html#create-index-multi-valued)。 [第12.17.3节"搜索JSON值的函数"](https://dev.mysql.com/doc/refman/8.0/en/json-search-functions.html)，提供了有关`JSON_OVERLAPS()`和的 信息`MEMBER OF()`以及使用示例。
