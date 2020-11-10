---
title: MySQL 8.0的新增功能探索-字符集支持
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

- **字符集支持。** 默认字符集已从更改 `latin1`为`utf8mb4`。该`utf8mb4`字符集有几个新的排序规则，其中包括 `utf8mb4_ja_0900_as_cs`，提供对Unicode在MySQL中第一个日本语言特定的排序。有关更多信息，请参见 [第10.10.1节" Unicode字符集"](https://dev.mysql.com/doc/refman/8.0/en/charset-unicode-sets.html)。
