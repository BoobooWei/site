---
title: MySQL 8.0的新增功能探索-表加密管理
date: 2020-05-26T17:58:00.000Z
categories:
- [培训教程,MySQL高级管理教程]
tags:
- 培训
- MySQL高级管理教程-探索类
- MySQL8
---

- **表加密管理。** 现在，可以通过定义和强制执行加密默认值来全局管理表加密。该 [`default_table_encryption`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_default_table_encryption) 变量为新创建的模式和常规表空间定义加密默认值。`DEFAULT ENCRYPTION`创建模式时，也可以使用子句定义模式的加密默认值。默认情况下，表继承对其创建的架构或常规表空间的加密。通过启用 [`table_encryption_privilege_check`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_table_encryption_privilege_check) 变量。当使用不同于设置的加密设置创建或更改模式或常规表空间[`default_table_encryption`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_default_table_encryption) 时，或者使用不同于默认模式加密的加密设置创建或更改表时，将进行特权检查 。 启用该 [`TABLE_ENCRYPTION_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_table-encryption-admin) 特权后，将允许覆盖默认的加密设置 [`table_encryption_privilege_check`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_table_encryption_privilege_check)。有关更多信息，请参见 [为架构和常规表空间定义加密默认值](https://dev.mysql.com/doc/refman/8.0/en/innodb-data-encryption.html#innodb-schema-tablespace-encryption-default)。-
