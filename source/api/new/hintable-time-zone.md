---
title: MySQL 8.0的新增功能探索-提示时区
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

**提示time_zone。** 从MySQL 8.0.17开始， [`time_zone`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_time_zone)使用可以提示会话变量 [`SET_VAR`](https://dev.mysql.com/doc/refman/8.0/en/optimizer-hints.html#optimizer-hints-set-var)。
