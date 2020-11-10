---
title: Backup Lock
date: 2020-06-10T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

## 功能描述

{% note info Backup lock%}
A new type of backup lock permits DML during an online backup while preventing operations that could result in an inconsistent snapshot. The new backup lock is supported by [`LOCK INSTANCE FOR BACKUP`](https://dev.mysql.com/doc/refman/8.0/en/lock-instance-for-backup.html) and [`UNLOCK INSTANCE`](https://dev.mysql.com/doc/refman/8.0/en/lock-instance-for-backup.html) syntax. The [`BACKUP_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_backup-admin) privilege is required to use these statements.

通过 Backup Lock 的机制，支持在线备份期间执行DML，防止可能导致快照不一致的操作。

[`LOCK INSTANCE FOR BACKUP`](https://dev.mysql.com/doc/refman/8.0/en/lock-instance-for-backup.html) 和 [`UNLOCK INSTANCE`](https://dev.mysql.com/doc/refman/8.0/en/lock-instance-for-backup.html)语法支持新的备份锁 。需要授予用户 [`BACKUP_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_backup-admin)权限。

{% endnote %}


## 功能学习

### Syntax

```sql
LOCK INSTANCE FOR BACKUP;

UNLOCK INSTANCE;
```

`LOCK INSTANCE FOR BACKUP`获取实例级别的*备份锁*，该*锁*在联机备份期间允许DML，同时防止可能导致快照不一致的操作。

### 权限

[`BACKUP_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_backup-admin)：启用[`LOCK INSTANCE FOR BACKUP`](https://dev.mysql.com/doc/refman/8.0/en/lock-instance-for-backup.html)语句的执行并访问Performance Schema [`log_status`](https://dev.mysql.com/doc/refman/8.0/en/log-status-table.html)表。

注意：此外[`BACKUP_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_backup-admin)，还需要表[`SELECT`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_select)上的 特权 [`log_status`](https://dev.mysql.com/doc/refman/8.0/en/log-status-table.html)才能对其进行访问。

当执行从早期版本到MySQL 8.0的就地升级时， 该[`BACKUP_ADMIN`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_backup-admin)特权将自动授予具有该[`RELOAD`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_reload)特权的用户 。

```sql
create user backup_user@'localhost' identified by 'Zyadmin@123';
grant backup_admin on *.* to backup_user@'localhost';
show grants for backup_user@'localhost';

+--------------------------------------------------------+
| Grants for backup_user@localhost                       |
+--------------------------------------------------------+
| GRANT USAGE ON *.* TO `backup_user`@`localhost`        |
| GRANT BACKUP_ADMIN ON *.* TO `backup_user`@`localhost` |
+--------------------------------------------------------+

# 为了测试给backup_user 用户更多的权限
grant all on *.* to backup_user@'localhost';
```

### 操作实践

```bash
# 会话1
mysql> use booboo;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> show tables;
+------------------+
| Tables_in_booboo |
+------------------+
| t1_d             |
| test001          |
+------------------+
2 rows in set (0.00 sec)

mysql> LOCK INSTANCE FOR BACKUP;
Query OK, 0 rows affected (0.00 sec)

# 会话2
mysql> create table tset002 (id int primary key);

# 会话1
mysql> UNLOCK INSTANCE;

# 会话2
mysql> create table tset002 (id int primary key);
Query OK, 0 rows affected (1 min 52.97 sec)
```

