---
title: Atomic DDL
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

## 功能描述

{% note info Atomic DDL%}

MySQL 8.0支持原子数据定义语言（DDL）语句。此功能称为 *Atomic DDL*。

Atomic DDL 将

* 对数据字典的更新
* 对存储引擎的操作
* 与DDL操作关联的二进制日志写入操作

组合到单个原子事务中。有关更多信息，请参见 [第13.1.1节"原子数据定义语句支持"](https://dev.mysql.com/doc/refman/8.0/en/atomic-ddl.html)。

即使服务器在操作过程中暂停，该操作也可以提交，并在数据字典，存储引擎和二进制日志中保留适用的更改，或者回滚。

当前仅`InnoDB`存储引擎支持原子DDL

{% endnote %}

## 功能学习

通过在MySQL 8.0中引入MySQL数据字典，得以实现Atomic DDL的功能。在早期的MySQL版本中，元数据存储在元数据文件，非事务表和存储引擎特定的字典中，这需要中间提交。MySQL数据字典提供的集中式事务性元数据存储消除了这一障碍，从而可以将DDL语句操作重组为原子性的。

本部分的以下主题下介绍了原子DDL功能：

- [支持的DDL语句](https://dev.mysql.com/doc/refman/8.0/en/atomic-ddl.html#atomic-ddl-supported-statements)
- [原子DDL特性](https://dev.mysql.com/doc/refman/8.0/en/atomic-ddl.html#atomic-ddl-characteristics)
- [DDL语句行为的变化](https://dev.mysql.com/doc/refman/8.0/en/atomic-ddl.html#atomic-ddl-statement-behavior)
- [存储引擎支持](https://dev.mysql.com/doc/refman/8.0/en/atomic-ddl.html#atomic-ddl-storage-engine-support)
- [查看DDL日志](https://dev.mysql.com/doc/refman/8.0/en/atomic-ddl.html#atomic-ddl-view-logs)

### 版本对比

对于与表相关的DDL操作，`InnoDB` 将DDL日志写入`mysql.innodb_ddl_log` 数据字典表。启用 [`innodb_print_ddl_logs`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_print_ddl_logs) 配置选项会将DDL恢复日志打印到 `stderr`。

原子DDL功能更改了某些语句的行为：

- [`DROP VIEW`](https://dev.mysql.com/doc/refman/8.0/en/drop-view.html)如果命名视图不存在，并且不进行任何更改，则失败并显示错误。以前，该语句返回一个错误，指示哪些视图不存在，但也删除了确实存在的视图。
- [`DROP TABLE`](https://dev.mysql.com/doc/refman/8.0/en/drop-table.html)如果命名表不存在，并且不进行任何更改，则失败并显示错误。以前，该语句返回错误，指示哪些表不存在，但还会删除那些存在的表。
- [`DROP TABLE`](https://dev.mysql.com/doc/refman/8.0/en/drop-table.html) 如果所有命名表都使用DDL支持的原子存储引擎，则为完全原子。
- [`DROP DATABASE`](https://dev.mysql.com/doc/refman/8.0/en/drop-database.html)如果所有表都使用原子DDL支持的存储引擎，则为原子。但是，最后一次从文件系统中删除数据库目录不是原子事务的一部分。如果由于文件系统错误或服务器暂停而导致数据库目录删除失败， [`DROP DATABASE`](https://dev.mysql.com/doc/refman/8.0/en/drop-database.html)则不会回滚事务。
- 使用受原子DDL支持的存储引擎的表上的中断DDL操作不再导致存储引擎，数据字典和二进制日志之间出现差异，也不会留下孤立文件。
- 不再部分执行帐户管理对帐单。帐户管理语句对所有命名用户都成功，也可以回滚，如果发生错误，则无效。

更改[`DROP TABLE`](https://dev.mysql.com/doc/refman/8.0/en/drop-table.html)， [`DROP VIEW`](https://dev.mysql.com/doc/refman/8.0/en/drop-view.html)和帐户管理报表行为具有跨版本复制配置的影响。

#### < 8.0.3

```bash
# 5.7.20
root@MySQL-01 14:10:  [test01]> show tables;
+------------------+
| Tables_in_test01 |
+------------------+
| Lower01          |
| mytest01         |
| test001          |
+------------------+
3 rows in set (0.00 sec)

root@MySQL-01 14:10:  [test01]> drop table test01,test001;
ERROR 1051 (42S02): Unknown table 'test01.test01'
root@MySQL-01 14:10:  [test01]> show tables;
+------------------+
| Tables_in_test01 |
+------------------+
| Lower01          |
| mytest01         |
+------------------+
2 rows in set (0.00 sec)
```

一条`DROP`表的命令中`test01`表不存在，报错；`test001`表存在被删除。

#### >= 8.0.3

```bash
# 8.0.20
mysql> show tables;
+------------------+
| Tables_in_booboo |
+------------------+
| t1_d             |
| test001          |
+------------------+
2 rows in set (0.00 sec)

mysql> drop table test01,test001;
ERROR 1051 (42S02): Unknown table 'booboo.test01'
mysql> show tables;
+------------------+
| Tables_in_booboo |
+------------------+
| t1_d             |
| test001          |
+------------------+
2 rows in set (0.00 sec)
```

一条`DROP`表的命令中`test01`表不存在，报错；`test001`表存在，没有被删除。



{% note info 对比%}

`drop table test01,test001;`

| DDL行为变化对比 | `test01`           | `test001` |
| --------------- | ------------------ | --------- |
| <8.0.3          | ERROR 1051 (42S02) | 删除      |
| >=8.0.3         | ERROR 1051 (42S02) | 未删除    |

{% endnote %}

##  探索DDL Log

### 参数设置

- [`innodb_print_ddl_logs`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_print_ddl_logs)

  | Property                                                     | Value                                |
  | ------------------------------------------------------------ | ------------------------------------ |
  | Command-Line Format                                          | `--innodb-print-ddl-logs[={OFF|ON}]` |
  | System Variable                                              | `innodb_print_ddl_logs`              |
  | Scope                                                        | Global                               |
  | Dynamic                                                      | Yes                                  |
  | [`SET_VAR`](https://dev.mysql.com/doc/refman/8.0/en/optimizer-hints.html#optimizer-hints-set-var) Hint Applies | No                                   |
  | Type                                                         | Boolean                              |
  | Default Value                                                | `OFF`                                |


### 数据字典 

 Enabling this option causes MySQL to write DDL logs to `stderr`. For more information, see [Viewing DDL Logs](https://dev.mysql.com/doc/refman/8.0/en/atomic-ddl.html#atomic-ddl-view-logs).

- `InnoDB`将DDL日志写入 `mysql.innodb_ddl_log`表以支持DDL操作的重做和回滚。该 `mysql.innodb_ddl_log`表是一个隐藏的数据字典表，位于 `mysql.ibd`数据字典表空间中。与其他隐藏数据字典表一样，该 `mysql.innodb_ddl_log`表不能在MySQL的非调试版本中直接访问。（请参见 [第14.1节“数据字典架构”](https://dev.mysql.com/doc/refman/8.0/en/data-dictionary-schema.html)。）`mysql.innodb_ddl_log`表的结构 与此定义相对应：

  ```sql
  CREATE TABLE mysql.innodb_ddl_log (
    id BIGINT UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY,
    thread_id BIGINT UNSIGNED NOT NULL,
    type INT UNSIGNED NOT NULL,
    space_id INT UNSIGNED,
    page_no INT UNSIGNED,
    index_id BIGINT UNSIGNED,
    table_id BIGINT UNSIGNED,
    old_file_path VARCHAR(512) COLLATE UTF8_BIN,
    new_file_path VARCHAR(512) COLLATE UTF8_BIN,
    KEY(thread_id)
  );
  ```

  - `id`：DDL日志记录的唯一标识符。
  - `thread_id`：每个DDL日志记录都分配了一个`thread_id`，用于重播和删除属于特定DDL操作的DDL日志。涉及多个数据文件操作的DDL操作会生成多个DDL日志记录。
  - `type`：DDL操作类型。类型包括`FREE`（删除索引树）， `DELETE`（删除文件）， `RENAME`（重命名文件）或 `DROP`（从`mysql.innodb_dynamic_metadata`数据字典表中删除元 数据）。
  - `space_id`：表空间ID。
  - `page_no`：包含分配信息的页面；例如，索引树的根页面。
  - `index_id`：索引ID。
  - `table_id`：表格ID。
  - `old_file_path`：旧表空间文件路径。由创建或删除表空间文件的DDL操作使用；也用于重命名表空间的DDL操作。
  - `new_file_path`：新表空间文件路径。由重命名表空间文件的DDL操作使用。

  该实施例表明能够 [`innodb_print_ddl_logs`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_print_ddl_logs)以查看写入到DDL日志`strderr`用于 `CREATE TABLE`操作。

### 调试版本

**使用MySQL的调试版本查看数据字典表**

数据字典表在默认情况下受保护，但可以通过使用调试支持（使用`-DWITH_DEBUG=1` **CMake**选项）编译MySQL 并指定 `+d,skip_dd_table_access_check` [`debug`](https://dev.mysql.com/doc/refman/8.0/en/server-options.html#option_mysqld_debug)选项和修饰符来访问。有关编译调试版本的信息，请参见 [第29.5.1.1节“编译MySQL以进行调试”](https://dev.mysql.com/doc/refman/8.0/en/compiling-for-debugging.html)。

{% note warn 警告%}

不建议直接修改或写入数据字典表，否则可能会使您的MySQL实例无法使用。

{% endnote %}

在使用调试支持编译MySQL之后，使用以下 [`SET`](https://dev.mysql.com/doc/refman/8.0/en/set.html)语句使数据字典表对[**mysql**](https://dev.mysql.com/doc/refman/8.0/en/mysql.html)客户端会话可见：

```sql
mysql> SET SESSION debug='+d,skip_dd_table_access_check';
```

使用此查询检索数据字典表的列表：

```sql
mysql> SELECT name, schema_id, hidden, type FROM mysql.tables where schema_id=1 AND hidden='System';
```

使用[`SHOW CREATE TABLE`](https://dev.mysql.com/doc/refman/8.0/en/show-create-table.html)查看数据字典表的定义。例如：

```sql
mysql> SHOW CREATE TABLE mysql.catalogs\G
```

### 操作实践

```bash
mysql> set global innodb_print_ddl_logs=1;
Query OK, 0 rows affected (0.00 sec)

mysql> select @@innodb_print_ddl_logs;
+-------------------------+
| @@innodb_print_ddl_logs |
+-------------------------+
|                       1 |
+-------------------------+
1 row in set (0.00 sec)
mysql> select table_name,engine from information_schema.tables where table_schema='mysql';
+---------------------------+--------+
| TABLE_NAME                | ENGINE |
+---------------------------+--------+
| columns_priv              | InnoDB |
| component                 | InnoDB |
| db                        | InnoDB |
| default_roles             | InnoDB |
| engine_cost               | InnoDB |
| func                      | InnoDB |
| general_log               | CSV    |
| global_grants             | InnoDB |
| gtid_executed             | InnoDB |
| help_category             | InnoDB |
| help_keyword              | InnoDB |
| help_relation             | InnoDB |
| help_topic                | InnoDB |
| innodb_index_stats        | InnoDB |
| innodb_table_stats        | InnoDB |
| password_history          | InnoDB |
| plugin                    | InnoDB |
| procs_priv                | InnoDB |
| proxies_priv              | InnoDB |
| role_edges                | InnoDB |
| server_cost               | InnoDB |
| servers                   | InnoDB |
| slave_master_info         | InnoDB |
| slave_relay_log_info      | InnoDB |
| slave_worker_info         | InnoDB |
| slow_log                  | CSV    |
| tables_priv               | InnoDB |
| time_zone                 | InnoDB |
| time_zone_leap_second     | InnoDB |
| time_zone_name            | InnoDB |
| time_zone_transition      | InnoDB |
| time_zone_transition_type | InnoDB |
| user                      | InnoDB |
+---------------------------+--------+
33 rows in set (0.00 sec)

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

mysql> drop table test01,test001;
ERROR 1051 (42S02): Unknown table 'booboo.test01'
mysql> select * from mysql.innodb_ddl_log;
ERROR 3554 (HY000): Access to system table 'mysql.innodb_ddl_log' is rejected.
```

由于开启需要编译并加上debug参数，此处不做拓展。