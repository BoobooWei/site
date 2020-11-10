---
title: MySQL 8.0的新增功能探索-配置
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

- **组态。** 在整个MySQL中，主机名的最大允许长度已从以前的60个字符增加到255个ASCII字符。例如，这适用于数据字典中与主机名相关的列， `mysql`系统架构，性能架构`INFORMATION_SCHEMA`和 `sys`；陈述的 `MASTER_HOST`价值 [`CHANGE MASTER TO`](https://dev.mysql.com/doc/refman/8.0/en/change-master-to.html)；语句输出中的`Host`列 [`SHOW PROCESSLIST`](https://dev.mysql.com/doc/refman/8.0/en/show-processlist.html)；帐户名称中的主机名（例如帐户管理对帐单和 `DEFINER`属性）；以及与主机名相关的命令选项和系统变量。注意事项：

  - 允许的主机名长度增加会影响在主机名列上具有索引的表。例如，`mysql`系统架构中索引主机名的表现在具有显式 `ROW_FORMAT`属性， `DYNAMIC`以容纳更长的索引值。
  - 某些基于文件名的配置设置可能是基于服务器主机名构造的。允许的值受基础操作系统的约束，该操作系统可能不允许文件名足够长以包含255个字符的主机名。这会影响到 [`general_log_file`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_general_log_file)， [`log_error`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_log_error)， [`pid_file`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_pid_file)， [`relay_log`](https://dev.mysql.com/doc/refman/8.0/en/replication-options-slave.html#sysvar_relay_log)，和 [`slow_query_log_file`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_slow_query_log_file) 系统变量和相应的选项。如果基于主机名的值对于OS而言太长，则必须提供明确的较短值。
  - 尽管服务器现在支持255个字符的主机名，但是与使用该[`--ssl-mode=VERIFY_IDENTITY`](https://dev.mysql.com/doc/refman/8.0/en/connection-options.html#option_general_ssl-mode) 选项建立的服务器的连接 受到OpenSSL支持的最大主机名长度的限制。主机名匹配与SSL证书的两个字段有关，它们的最大长度如下：公用名：最大长度为64；最大名称为64。主题备用名称：根据RFC＃1034的最大长度。
