---
title: MySQL 8.0的新增功能探索-JSON模式验证
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

- **JSON模式验证。** MySQL 8.0.17添加了两个功能 [`JSON_SCHEMA_VALID()`](https://dev.mysql.com/doc/refman/8.0/en/json-validation-functions.html#function_json-schema-valid)， [`JSON_SCHEMA_VALIDATION_REPORT()`](https://dev.mysql.com/doc/refman/8.0/en/json-validation-functions.html#function_json-schema-validation-report) 用于再次验证JSON文档JSON模式。 `JSON_SCHEMA_VALID()`如果文档根据架构进行验证，则返回TRUE（1），否则通过FALSE（0）返回。 `JSON_SCHEMA_VALIDATION_REPORT()`返回一个JSON文档，其中包含有关验证结果的详细信息。以下语句适用于这两个功能：- 模式必须符合JSON模式规范的草案4。

  - `required` 支持属性。
  - `$ref` 不支持 外部资源和关键字。
  - 支持正则表达式模式；无效模式将被静默忽略。

  有关更多信息和示例，请参见[第12.17.7节" JSON模式验证函数"](https://dev.mysql.com/doc/refman/8.0/en/json-validation-functions.html)。
