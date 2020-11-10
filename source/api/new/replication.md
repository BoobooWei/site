---
title: MySQL 8.0的新增功能探索-复制
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

- **复制。** 对MySQL复制进行了以下增强：- MySQL复制现在支持使用紧凑的二进制格式对JSON文档的部分更新进行二进制日志记录，从而在记录完整的JSON文档时节省了日志空间。当使用基于语句的日志记录时，这种紧凑的日志记录会自动完成，并且可以通过将新的`binlog_row_value_options`系统变量设置为来启用 `PARTIAL_JSON`。有关更多信息，请参见[JSON值的部分更新](https://dev.mysql.com/doc/refman/8.0/en/json.html#json-partial-updates)以及的描述 [`binlog_row_value_options`](https://dev.mysql.com/doc/refman/8.0/en/replication-options-binary-log.html#sysvar_binlog_row_value_options)。
