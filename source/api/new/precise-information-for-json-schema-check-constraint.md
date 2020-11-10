---
title: MySQL 8.0的新增功能探索-有关JSON模式CHECK约束失败的精确信息
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

- **有关JSON模式CHECK约束失败的精确信息。** 当 [`JSON_SCHEMA_VALID()`](https://dev.mysql.com/doc/refman/8.0/en/json-validation-functions.html#function_json-schema-valid)用于指定`CHECK`约束时，MySQL 8.0.19及更高版本提供有关此类约束失败原因的准确信息。

有关示例和更多信息，请参见 [JSON_SCHEMA_VALID（）和CHECK约束](https://dev.mysql.com/doc/refman/8.0/en/json-validation-functions.html#json-validation-functions-constraints)。另请参见[第13.1.20.6节"检查约束"](https://dev.mysql.com/doc/refman/8.0/en/create-table-check-constraints.html)。
