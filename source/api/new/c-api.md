---
title: MySQL 8.0的新增功能探索-C API
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

- **C API。** MySQL C API现在支持异步功能，用于与MySQL服务器的非阻塞通信。每个功能都是现有同步功能的异步对应项。如果从服务器连接读取或写入服务器连接，则必须等待同步功能。异步功能使应用程序可以检查服务器连接上的工作是否准备就绪。如果不是，应用程序可以执行其他工作，然后再进行检查。请参见 [第28.7.11节" C API异步接口"](https://dev.mysql.com/doc/refman/8.0/en/c-api-asynchronous-interface.html)。-
