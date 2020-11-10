---
title: MySQL 8.0的新增功能探索-正则表达式支持
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

- **正则表达式支持。** 此前，MySQL的使用的亨利斯宾塞正则表达式库来支持正则表达式运算符（[`REGEXP`](https://dev.mysql.com/doc/refman/8.0/en/regexp.html#operator_regexp)， [`RLIKE`](https://dev.mysql.com/doc/refman/8.0/en/regexp.html#operator_regexp)）。使用Unicode国际组件（ICU）重新实现了对正则表达式的支持，该组件提供了完整的Unicode支持并且是多字节安全的。该 [`REGEXP_LIKE()`](https://dev.mysql.com/doc/refman/8.0/en/regexp.html#function_regexp-like)函数以[`REGEXP`](https://dev.mysql.com/doc/refman/8.0/en/regexp.html#operator_regexp)和 [`RLIKE`](https://dev.mysql.com/doc/refman/8.0/en/regexp.html#operator_regexp) 运算符的方式执行正则表达式匹配 ，它们现在是该函数的同义词。此外， [`REGEXP_INSTR()`](https://dev.mysql.com/doc/refman/8.0/en/regexp.html#function_regexp-instr)， [`REGEXP_REPLACE()`](https://dev.mysql.com/doc/refman/8.0/en/regexp.html#function_regexp-replace)，和 [`REGEXP_SUBSTR()`](https://dev.mysql.com/doc/refman/8.0/en/regexp.html#function_regexp-substr)函数可用于查找匹配位置并分别执行子字符串替换和提取。该 [`regexp_stack_limit`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_regexp_stack_limit)和 [`regexp_time_limit`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_regexp_time_limit)系统变量提供由发动机匹配了资源消耗的控制。有关更多信息，请参见 [第12.7.2节"正则表达式"](https://dev.mysql.com/doc/refman/8.0/en/regexp.html)。有关实现更改可能影响使用正则表达式的应用程序的方式的信息，请参见 [正则表达式兼容性注意事项](https://dev.mysql.com/doc/refman/8.0/en/regexp.html#regexp-compatibility)。
