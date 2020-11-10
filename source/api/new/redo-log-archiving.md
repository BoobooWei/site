---
title: MySQL 8.0的新增功能探索-重做日志归档
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

**重做日志归档。** 从MySQL 8.0.17开始，`InnoDB`支持重做日志归档。在进行备份操作时，复制重做日志记录的备份实用程序有时可能无法跟上重做日志生成的步伐，由于这些记录被覆盖，导致丢失重做日志记录。重做日志归档功能通过将重做日志记录顺序写入存档文件来解决此问题。备份实用程序可以根据需要从存档文件复制重做日志记录，从而避免潜在的数据丢失。有关更多信息，请参阅[重做日志归档](https://dev.mysql.com/doc/refman/8.0/en/innodb-redo-log.html#innodb-redo-log-archiving)。
