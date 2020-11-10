---
title: MySQL 8.0的新增功能探索-TIMESTAMP和DATETIME的时区支持
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

- **TIMESTAMP和DATETIME的时区支持。** 从MySQL 8.0.19开始，服务器接受带有插入的datetime（[`TIMESTAMP`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html)和 [`DATETIME`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html)）值的时区偏移量。该偏移量使用与设置[`time_zone`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_time_zone)系统变量时使用的格式相同的格式，不同之处在于，当偏移量的小时部分小于10且`'-00:00'`不允许时，前导零是必需的 。日期时间文字，其中包括时区偏移的例子是 `'2019-12-11 10:40:30-05:00'`， `'2003-04-14 03:30:00+10:00'`和 `'2020-01-01 15:35:45+05:30'`。

选择日期时间值时不显示时区偏移量。

包含时区偏移量的Datetime文字可用作准备好的语句参数值。

作为这项工作的一部分，用于设置 [`time_zone`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_time_zone)系统变量的值现在也被限制 `-14:00`为`+14:00`，包括在内。（它仍然可以分配名称值以 `time_zone`诸如 `'EST'`， `'Posix/Australia/Brisbane'`和 `'Europe/Stockholm'`该变量，条件是MySQL的时区表被加载;另见 [填充的时区表](https://dev.mysql.com/doc/refman/8.0/en/time-zone-support.html#time-zone-installation)）。

有关更多信息和示例，请参见 [第5.1.14节" MySQL服务器时区支持"](https://dev.mysql.com/doc/refman/8.0/en/time-zone-support.html)以及 [第11.2.2节" DATE，DATETIME和TIMESTAMP类型"](https://dev.mysql.com/doc/refman/8.0/en/datetime.html)。
