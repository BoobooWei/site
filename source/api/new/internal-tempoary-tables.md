---
title: MySQL 8.0的新增功能探索-内部临时表
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

- **内部临时表。** 的`TempTable`存储引擎替换`MEMORY`存储引擎作为默认发动机用于在内存中的内部临时表。该`TempTable`存储引擎提供了有效的存储 [`VARCHAR`](https://dev.mysql.com/doc/refman/8.0/en/char.html)和 [`VARBINARY`](https://dev.mysql.com/doc/refman/8.0/en/binary-varbinary.html)列。的 [`internal_tmp_mem_storage_engine`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_internal_tmp_mem_storage_engine) 会话变量定义了用于在存储器内的临时表的存储引擎。允许的值为 `TempTable`（默认值）和 `MEMORY`。该 [`temptable_max_ram`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_temptable_max_ram) 变量定义`TempTable`在将数据存储到磁盘之前存储引擎可以使用的最大内存量 。- **正在记录。** 错误记录已重写为使用MySQL组件体系结构。传统的错误日志记录是使用内置组件实现的，而使用系统日志的日志记录则是可加载的组件。此外，还提供了可加载的JSON日志编写器。要控制要启用的日志组件，请使用 [`log_error_services`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_log_error_services)系统变量。有关更多信息，请参见 [第5.4.2节"错误日志"](https://dev.mysql.com/doc/refman/8.0/en/error-log.html)。
