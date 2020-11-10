---
title: MySQL 8.0的新增功能探索-连接管理
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

- **连接管理。** MySQL服务器现在允许专门为管理连接配置TCP / IP端口。这提供了用于普通连接的网络接口上允许的单个管理连接的替代方法，即使 [`max_connections`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_max_connections) 已经建立连接也是如此。请参见 [第5.1.12.1节"连接接口"](https://dev.mysql.com/doc/refman/8.0/en/connection-interfaces.html)。MySQL现在提供了对压缩使用的更多控制，以最大程度减少通过与服务器的连接发送的字节数。以前，给定的连接未压缩或已使用`zlib`压缩算法。现在，也可以使用该 `zstd`算法，并选择`zstd`连接的压缩级别。可以在服务器端以及连接原始端配置允许的压缩算法，以通过客户端程序以及参与主/从复制或组复制的服务器进行连接。有关更多信息，请参见 [第4.2.6节"连接压缩控制"](https://dev.mysql.com/doc/refman/8.0/en/connection-compression-control.html)。
