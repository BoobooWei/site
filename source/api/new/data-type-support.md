---
title: MySQL 8.0的新增功能探索-数据类型支持
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

- **数据类型支持。** MySQL现在支持使用表达式作为数据类型规范中的默认值。这包括使用表达式作为默认值 [`BLOB`](https://dev.mysql.com/doc/refman/8.0/en/blob.html)， [`TEXT`](https://dev.mysql.com/doc/refman/8.0/en/blob.html)， `GEOMETRY`，和 [`JSON`](https://dev.mysql.com/doc/refman/8.0/en/json.html)数据类型，这在以前是根本不会被分配缺省值。有关详细信息，请参见[第11.6节"数据类型默认值"](https://dev.mysql.com/doc/refman/8.0/en/data-type-defaults.html)。
