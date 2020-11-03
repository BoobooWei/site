---
title: 全方位认识 sys 系统库
---

> 基于MySQL 5.7.18 版本整理

# 初识 SYS 系统库

[SYS系统库开发网站](https://github.com/mysql/mysql-sys)

## SYS系统库使用环境

在使用sys系统库之前，你需要确保你的数据库环境满足如下条件：

1. `sys`系统库支持MySQL 5.6或更高版本，5.5.x及其以下版本不支持；
2. `sys`系统库提供了一些代替直接访问`performance_schema`的视图，所以必须启用`performance_schema`(`performance_schema`系统参数设置为`ON`)之后`sys`系统库的大部分功能才能正常使用；
3. 要完全访问`sys`系统库，用户必须具有以下权限： 
	* 对所有`sys`表和视图具有`SELECT`权限 
	* 对所有`sys`存储过程和函数具有`EXECUTE`权限 
	* 对`sys_config`表具有`INSERT`、`UPDATE`权限 
	* 对某些特定的`sys`系统库存储过程和函数需要额外权限，如，`ps_setup_save()`存储过程，需要临时表相关的权限
4. `sys`系统库执行访问的对象相关的权限： 
	* 任何被`sys`系统库访问的`performance_schema`表需要有`SELECT`权限，如果要使用`sys`系统库对`performance_schema`相关表执行更新，则需要`performance_schema`相关表的`UPDATE`权限 
	* `INFORMATION_SCHEMA.INNODB_BUFFER_PAGE`表的`PROCESS`
5. 如果要充分使用`sys`系统库的功能，则必须启用某些`performance_schema`的`instruments`和`consumers`，如下： 
	* 所有`wait instruments` 
	* 所有`stage instruments` 
	* 所有`statement instruments` 
	* 对于所启用的类型事件的`instruments`，还需要启用对应类型的`consumers`(`xxx_current`和`xxx_history_long`)，要了解某存储过程具体做了什么事情可能通过`show create procedure procedure_name;`语句查看

### 实践1-启动所有需要的`instruments`和`consumers`

可以使用`sys`系统库本身来启用所有需要的`instruments`和`consumers`：

* 启用所有`wait instruments`：`CALL sys.ps_setup_enable_instrument('wait');`
* 启用所有`stage instruments`：`CALL sys.ps_setup_enable_instrument('stage');`
* 启用所有`statement instruments`：`CALL sys.ps_setup_enable_instrument('statement');`
* 启用所有事件类型的`current`表：`CALL sys.ps_setup_enable_consumer('current');`
* 启用所有事件类型的`history_long`表：`CALL sys.ps_setup_enable_consumer('history_long');`

```sql
# 启用所有`wait instruments`
CALL sys.ps_setup_enable_instrument('wait');
# 启用所有`stage instruments`
CALL sys.ps_setup_enable_instrument('stage');
# 启用所有`statement instruments`
CALL sys.ps_setup_enable_instrument('statement');
# 启用所有事件类型的`current`表
CALL sys.ps_setup_enable_consumer('current');
# 启用所有事件类型的`history_long`表
CALL sys.ps_setup_enable_consumer('history_long');
```

### 实践2-快速恢复到`performance_schema`的默认配置

```sql
CALL sys.ps_setup_reset_to_default(TRUE);
```



注意：
* `performance_schema`的默认配置就可以满足`sys`系统库的大部分数据收集功能。
* 启用上述所提及的所有instruments和consumers会对性能产生一定影响，因此最好仅启用所需的配置。
* 如果你在启用了一些默认配置之外的配置，则可以使用存储过程：`CALL sys.ps_setup_reset_to_default(TRUE); `来快速恢复到`performance_schema`的默认配置。
* 对于以上繁杂的权限要求，通常创建一个具有管理员权限的账号即可，当然如果你有明确的需求，那另当别论，但`sys`系统库通常都是提供给专业的DBA人员排查一些特定问题使用的，其下所涉及的各项查询或多或少都会对性能有一定影响（主要体现在performance_schema功能实现的性能开销），在不明需求的情况下，不建议开放这些功能来作为常规的监控手段使用。

## SYS系统库初体验

### 实践1-查看数据库版本

通过`version`视图可以查看`sys` 系统库和`mysql server`的版本号
```sql
# version视图可以查看sys 系统库和mysql server的版本号
mysql> USE sys;
mysql> SELECT * FROM version;
+ ------------- + ----------------- +
| sys_version | mysql_version |
+ ------------- + ----------------- +
| 1.5.0 | 5.7.9-debug-log |
+ ------------- + ----------------- +
```

也可以使用`db_name.view_name`、`db_name.procedure_name`、`db_name.func_name`等方式在不指定默认数据库的情况下访问sys 系统库中的对象(这叫做名称限定对象引用)，如下：

```sql
mysql> SELECT * FROM sys.version;
+ ------------- + ----------------- +
| sys_version | mysql_version |
+ ------------- + ----------------- +
| 1.5.0 | 5.7.9-debug-log |
+ ------------- + ----------------- +
```

### 实践2-视图数值展示的区别

PS：下文中的示例中，对于`sys`系统库的访问都是假定指定了默认数据库为 `sys` 系统库。

`SYS`系统库下包含许多视图，它们以各种方式对`performance_schema`表进行**聚合计算**展示。这些视图中大部分都是成对出现，两个视图名称相同，但有一个视图是带`x$`字符前缀的，例如：
* `host_summary_by_file_io`: 相关数值数据转化为人性化显示，例如毫秒、秒、分钟、小时、天等
* `x$host_summary_by_file_io` ：相关数值保持原始的数据(皮秒)

以上两个视图均代表`按照主机进行汇总统计的文件I/O性能数据`，两个视图访问数据源是相同的。

```sql
# x$host_summary_by_file_io视图汇总数据，显示未格式化的皮秒单位延迟时间，没有x$前缀字符的视图输出的信息经过单位换算之后可读性更高
mysql> SELECT * FROM host_summary_by_file_io;
+------------+-------+------------+
| host      | ios  | io_latency |
+------------+-------+------------+
| localhost  | 67570 | 5.38 s    |
| background |  3468 | 4.18 s    |
+------------+-------+------------+
# 对于带x$的视图显示原始的皮秒单位数值，对于程序或工具获取使用更易于数据处理
mysql> SELECT * FROM x$host_summary_by_file_io;
+------------+-------+---------------+
| host      | ios  | io_latency    |
+------------+-------+---------------+
| localhost  | 67574 | 5380678125144 |
| background |  3474 | 4758696829416 |
+------------+-------+---------------+
```

### 实践3-查看SYS库的对象DDL

要查看 `sys` 系统库对象定义语句，可以使用适当的`SHOW`语句或`INFORMATION_SCHEMA`库查询。例如，要查看`session`视图和`format_bytes()`函数的定义，可以使用如下语句：

```sql
mysql> SHOW CREATE VIEW session\G;
mysql> SHOW CREATE FUNCTION format_bytes\G;
```

### 实践4-导出导入SYS库

备份SYS库

```bash
mysqldump --databases --routines sys> sys_dump.sql
mysqlpump sys> sys_dump.sql
```
导入SYS库
```sql
mysql < sys_dump.sql
```

## SYS系统库的进度报告功能

从MySQL 5.7.9开始，`sys`系统库视图提供查看长时间运行的事务的进度报告，通过`processlist`和`session`以及`x$`前缀的视图进行查看，其中:

* `processlist`包含了后台线程和前台线程当前的事件信息

* `session`不包含后台线程和`command`为`Daemon`的线程

如下：

```sql
processlist
session
x$processlist
x$session
```

`session`视图是直接调用`processlist`视图过滤了后台线程和`command`为`Daemon`的线程（所以两个视图输出结果的字段相同），而`processlist`线程联结查询了`threads`、`events_waits_current`、`events_stages_current`、`events_statements_current`、`events_transactions_current`、`sys.x$memory_by_thread_by_current_bytes`、`session_connect_attrs`表，so，需要打开相应的`instruments`和`consumers`，否则谁没打开谁对应的信息字段列就为`NULL`，对于`trx_state`字段为`ACTIVE`的线程，`progress`可以输出百分比进度信息(只有支持进度的事件才会被统计并打印进来)

### 实践1-查看语句进度信息

```sql
# 查看当前正在执行的语句进度信息
select * from session where conn_id!=connection_id() and trx_state='ACTIVE';
# 查看已经执行完的语句相关统计信息
select * from session where conn_id!=connection_id() and trx_state='COMMITTED';
```

操作记录

```sql
# 查看当前正在执行的语句进度信息
admin@localhost : sys 06:57:21> select * from session where conn_id!=connection_id() and trx_state='ACTIVE'\G;
*************************** 1. row ***************************
            thd_id: 47
          conn_id: 5
              user: admin@localhost
                db: sbtest
          command: Query
            state: alter table (merge sort)
              time: 29
current_statement: alter table sbtest1 add index i_c(c)
statement_latency: 29.34 s
          progress: 49.70
      lock_latency: 4.34 ms
    rows_examined: 0
        rows_sent: 0
    rows_affected: 0
        tmp_tables: 0
  tmp_disk_tables: 0
        full_scan: NO
    last_statement: NULL
last_statement_latency: NULL
    current_memory: 4.52 KiB
        last_wait: wait/io/file/innodb/innodb_temp_file
last_wait_latency: 369.52 us
            source: os0file.ic:470
      trx_latency: 29.45 s
        trx_state: ACTIVE
    trx_autocommit: YES
              pid: 4667
      program_name: mysql
1 row in set (0.12 sec)
# 查看已经执行完的语句相关统计信息
admin@localhost : sys 07:02:21> select * from session where conn_id!=connection_id() and trx_state='COMMITTED'\G;
*************************** 1. row ***************************
            thd_id: 47
          conn_id: 5
              user: admin@localhost
                db: sbtest
          command: Sleep
            state: NULL
              time: 372
current_statement: NULL
statement_latency: NULL
          progress: NULL
      lock_latency: 4.34 ms
    rows_examined: 0
        rows_sent: 0
    rows_affected: 0
        tmp_tables: 0
  tmp_disk_tables: 0
        full_scan: NO
    last_statement: alter table sbtest1 add index i_c(c)
last_statement_latency: 1.61 m
    current_memory: 4.52 KiB
        last_wait: idle
last_wait_latency: Still Waiting
            source: socket_connection.cc:69
      trx_latency: 1.61 m
        trx_state: COMMITTED
    trx_autocommit: YES
              pid: 4667
      program_name: mysql
1 row in set (0.12 sec)
```

对于`stage`事件进度报告要求必须启用`events_stages_current consumers`，启用需要查看进度相关的`instruments`。例如：

```sql
stage/sql/Copying to tmp table
stage/innodb/alter table (end)
stage/innodb/alter table (flush)
stage/innodb/alter table (insert)
stage/innodb/alter table (log apply index)
stage/innodb/alter table (log apply table)
stage/innodb/alter table (merge sort)
stage/innodb/alter table (read PK and internal sort)
stage/innodb/buffer pool load
```

对于不支持进度的`stage` 事件，或者未启用所需的`instruments`或`consumers`的`stage`事件，则对应的进度信息列显示为`NULL`。

官方文档：

https://dev.mysql.com/doc/refman/5.7/en/sys-schema-progress-reporting.html

https://dev.mysql.com/doc/refman/5.7/en/sys-schema-prerequisites.html

https://dev.mysql.com/doc/refman/5.7/en/sys-schema-usage.html







