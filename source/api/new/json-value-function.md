---
title: MySQL 8.0的新增功能探索-JSON_VALUE()函数
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

- **JSON_VALUE（）函数。** MySQL 8.0.21实现了一个[`JSON_VALUE()`](https://dev.mysql.com/doc/refman/8.0/en/json-search-functions.html#function_json-value)旨在简化[`JSON`](https://dev.mysql.com/doc/refman/8.0/en/json.html) 列索引的新功能 。在最基本的形式中，它将JSON文档和指向该文档中单个值的JSON路径作为参数，以及（可选）允许您使用`RETURNING`关键字指定返回类型 。 等效于此： `JSON_VALUE(*`json_doc`*, *`path`* RETURNING *`type`*)`

  ```sql
  CAST(
      JSON_UNQUOTE( JSON_EXTRACT(json_doc, path) )
      AS type
  );
  ```

  您还可以指定`ON EMPTY`， `ON ERROR`或两个子句，与一起使用 [`JSON_TABLE()`](https://dev.mysql.com/doc/refman/8.0/en/json-table-functions.html#function_json-table)。

  您可以使用`JSON_VALUE()`在这样的`JSON`列上的表达式上创建索引：

  ```sql
  CREATE TABLE t1(
      j JSON,
      INDEX i1 ( (JSON_VALUE(j, '$.id' RETURNING UNSIGNED)) )
  );

  INSERT INTO t1 VALUES ROW('{"id": "123", "name": "shoes", "price": "49.95"}');
  ```

  使用此表达式的查询（例如此处所示）可以使用索引：

  ```sql
  SELECT name, price FROM t1
      WHERE JSON_VALUE(j, '$.id' RETURNING UNSIGNED) = 123;
  ```

  在许多情况下，这比从该`JSON`列创建一个生成的列然后在生成的列上创建索引要简单得多。

  有关更多信息和示例，请参见的描述 [`JSON_VALUE()`](https://dev.mysql.com/doc/refman/8.0/en/json-search-functions.html#function_json-value)。
