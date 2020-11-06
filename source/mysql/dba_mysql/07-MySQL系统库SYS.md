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



# SYS 系统库配置

## SYS_CONFIG 表

该表包含SYS系统库的配置选项，每个配置选项一行记录。该表是INNODB表，可以通过客户端更新此表来持久化配置，SERVER重启不会丢失。

SYS_CONFIG表字段含义如下：

- VARIABLE：配置选项名称
- VALUE：配置选项值
- SET_TIME：该行配置最近修改时间
- SET_BY：最近一次对改行配置进行修改的帐户名。如果自SERVER安装SYS 系统库以来，该行配置从未被更改过，则该列值为NULL


为了减少对SYS_CONFIG表直接读取的次数，SYS 系统库中的视图、存储过程在需要使用到这些配置选项时，会优先检查这些配置选项对应的用户自定义配置选项变量(用户自定义配置选项变量与该表中的配置选项都具有相同的名称，例如：表中的DIAGNOSTICS.INCLUDE_RAW选项，对应的自定义配置选项变量是@SYS.DIAGNOSTICS.INCLUDE_RAW)。如果用户定义的配置选项变量存在于当前会话作用域中并且是非空的，那么SYS 系统库中的函数、存储过程将优先使用该配置选项变量值。否则，该SYS 系统库函数和存储过程将使用SYS_CONFIG表中的配置选项值(从表中读取配置选项值之后，会将SYS_CONFIG表中的配置选项时同时更新到用户自定义配置选项变量中，以便在同一会话后续对该值的引用时使用变量值，而不必再次从SYS_CONFIG表中读取)，示例：STATEMENT_TRUNCATE_LEN配置选项控制FORMAT_STATEMENT()函数返回的语句的最大长度。默认值为64.如果要临时将当前会话的值更改为32，可以设置对应的@SYS.STATEMENT_TRUNCATE_LEN用户定义的配置选项变量：

```bash
# STATEMENT_TRUNCATE_LEN配置选项默认是64，直接调用FORMAT_STATEMENT()函数返回是64字节长度，在未调用任何涉及到该配置选项的函数之前，该自定义变量值为NULL，此时函数需要从表中查询默认值
ADMIN@LOCALHOST : SYS 11:47:37> SELECT @SYS.STATEMENT_TRUNCATE_LEN;
+-----------------------------+
| @SYS.STATEMENT_TRUNCATE_LEN |
+-----------------------------+
| NULL                        |
+-----------------------------+
1 ROW IN SET (0.00 SEC)
ADMIN@LOCALHOST : SYS 11:51:53> SET @STMT = 'SELECT VARIABLE, VALUE, SET_TIME, SET_BY FROM SYS_CONFIG';
QUERY OK, 0 ROWS AFFECTED (0.00 SEC)
ADMIN@LOCALHOST : SYS 11:52:04> SELECT FORMAT_STATEMENT(@STMT);
+----------------------------------------------------------+
| FORMAT_STATEMENT(@STMT)                                  |
+----------------------------------------------------------+
| SELECT VARIABLE, VALUE, SET_TIME, SET_BY FROM SYS_CONFIG |
+----------------------------------------------------------+
1 ROW IN SET (0.01 SEC)
# 调用过一次FORMAT_STATEMENT()函数之后，表中的默认值会被更新到该自定义配置选项变量中
ADMIN@LOCALHOST : SYS 11:52:12> SELECT @SYS.STATEMENT_TRUNCATE_LEN;
+-----------------------------+
| @SYS.STATEMENT_TRUNCATE_LEN |
+-----------------------------+
| 64                          |
+-----------------------------+
1 ROW IN SET (0.00 SEC)
# 在会话级别中修改为32
ADMIN@LOCALHOST : SYS 11:52:20> SET @SYS.STATEMENT_TRUNCATE_LEN = 32;
QUERY OK, 0 ROWS AFFECTED (0.00 SEC)
ADMIN@LOCALHOST : SYS 11:52:34> SELECT @SYS.STATEMENT_TRUNCATE_LEN;
+-----------------------------+
| @SYS.STATEMENT_TRUNCATE_LEN |
+-----------------------------+
|                          32 |
+-----------------------------+
1 ROW IN SET (0.00 SEC)
# 再次调用FORMAT_STATEMENT()函数值，可以发现返回结果中的长度缩短了，说明使用了SESSION级别修改的值32
ADMIN@LOCALHOST : SYS 11:52:41> SELECT FORMAT_STATEMENT(@STMT);
+-----------------------------------+
| FORMAT_STATEMENT(@STMT)          |
+-----------------------------------+
| SELECT VARIABL ... ROM SYS_CONFIG |
+-----------------------------------+
1 ROW IN SET (0.00 SEC)
```

要停止使用用户定义的配置选项变量并恢复使用SYS_CONFIG表中的值，可以将会话中的配置选项变量设置为NULL，或者结束当前会话（结束会话会使得用户定义的变量被销毁）重新开启一个新的会话：

```bash
MYSQL> SET @SYS.STATEMENT_TRUNCATE_LEN = NULL;
MYSQL> SELECT FORMAT_STATEMENT(@STMT);
+----------------------------------------------------------+
| FORMAT_STATEMENT(@STMT)                                  |
+----------------------------------------------------------+
| SELECT VARIABLE, VALUE, SET_TIME, SET_BY FROM SYS_CONFIG |
+----------------------------------------------------------+
```


注意：如果用户在会话中设置了自定义配置选项变量值，然后再更新了SYS_CONFIG表中相同名称的配置选项，则对于当前会话，SYS_CONFIG表中的配置选项值不生效（除非设置自定义配置选项变量值为NULL），只对于新的会话且不存在自定义配置选项变量或者自定义配置选项值为NULL生效（因为此时会从SYS_CONFIG表中读取）。

SYS_CONFIG表中的选项和相应的用户定义的配置选项变量相关描述如下：

- DIAGNOSTICS.ALLOW_I_S_TABLES，@SYS.DIAGNOSTICS.ALLOW_I_S_TABLES：如果此选项为ON，则DIAGNOSTICS()存储过程在调用时会扫描INFORMATION_SCHEMA.TABLES表找到所有的基表与STATISTICS表执行联结查询，扫描每个表的统计信息。如果基表非常多，该操作可能比较昂贵。默认为OFF。此选项在MYSQL 5.7.9中新增
- DIAGNOSTICS.INCLUDE_RAW，@SYS.DIAGNOSTICS.INCLUDE_RAW：如果此选项为ON，则DIAGNOSTICS()存储过程的输出信息中会包括METRICS视图中的原始输出信息（该存储过程中会调用METRICS视图）。默认为OFF。此选项在MYSQL 5.7.9中新增
- PS_THREAD_TRX_INFO.MAX_LENGTH，@SYS.PS_THREAD_TRX_INFO.MAX_LENGTH：由PS_THREAD_TRX_INFO()函数生成的JSON输出结果的最大长度。默认值为65535字节。此选项在MYSQL 5.7.9中新增
- STATEMENT_PERFORMANCE_ANALYZER.LIMIT，@SYS.STATEMENT_PERFORMANCE_ANALYZER.LIMIT：不具有内置限制的视图返回的最大行数。默认值为100（例如，STATEMENTS_WITH_RUNTIMES_IN_95TH_PERCENTILE视图具有内置限制，即只返回平均执行时间为占总执行时间分布的95百分位数的语句）。此选项在MYSQL 5.7.9中新增
- STATEMENT_PERFORMANCE_ANALYZER.VIEW，@SYS.STATEMENT_PERFORMANCE_ANALYZER.VIEW：给STATEMENT_PERFORMANCE_ANALYZER()存储过程当作入参使用的自定义查询或视图名称（STATEMENT_PERFORMANCE_ANALYZER()存储过程由DIAGNOSTICS()存储过程内部调用）。如果该选项值包含空格，则将其值解释为查询语句。否则解释为视图名称，且这个视图必须是提前创建好的用于查询PERFORMANCE_SCHEMA.EVENTS_STATEMENTS_SUMMARY_BY_DIGEST表的视图。如果STATEMENT_PERFORMANCE_ANALYZER.LIMIT配置选项值大于0，则STATEMENT_PERFORMANCE_ANALYZER.VIEW配置选项指定的查询语句或视图中不能有任何LIMIT子句(因为STATEMENT_PERFORMANCE_ANALYZER.LIMIT选项在STATEMENT_PERFORMANCE_ANALYZER()存储过程中是作为一个条件判断值决定是否要添加一个LIMIT子句，如果你再自行添加一个LIMIT会导致语法错误)。STATEMENT_PERFORMANCE_ANALYZER.VIEW配置选项默认值为NULL。此选项在MYSQL 5.7.9中新增
- STATEMENT_TRUNCATE_LEN，@SYS.STATEMENT_TRUNCATE_LEN：控制FORMAT_STATEMENT()函数返回的语句文本的最大长度。超过该长度的语句文本会被截断，只保留该配置选项定义的长度文本。默认值为64字节

其他选项可以被添加到SYS_CONFIG表中。例如：如果存在DEBUG配置选项且不为NULL值，则DIAGNOSTICS()和EXECUTE_PREPARED_STMT()存储过程调用时会执行检查并做相应的判断，但默认情况下，此选项在SYS_CONFIG表中不存在，因为DEBUG输出通常只能临时启用，通过会话级别设置自定义配置选项变量实现，如：SET @SYS.DEBUG='ON';

```
# 如果所有会话都需要使用，则可以将DEBUG选项INSERT到SYS_CONFIG表中
MYSQL> INSERT INTO SYS_CONFIG (VARIABLE, VALUE) VALUES('DEBUG', 'ON');
# 要更改表中的调试配置选项值，可以使用UPDATE语句更新该配置选项值
## 首先，修改表中的值：
MYSQL> UPDATE SYS_CONFIG SET VALUE = 'OFF' WHERE VARIABLE = 'DEBUG';
## 然后，为了确保当前会话中的存储过程调用时使用表中的更改后的值，需要将相应的用户定义的变量设置为NULL
MYSQL> SET @SYS.DEBUG = NULL;
```

记录内容示例

```sql
ADMIN@LOCALHOST : SYS 09:48:46> SELECT * FROM SYS_CONFIG;
+--------------------------------------+-------+---------------------+--------+
| VARIABLE                            | VALUE | SET_TIME            | SET_BY |
+--------------------------------------+-------+---------------------+--------+
| DIAGNOSTICS.ALLOW_I_S_TABLES        | OFF  | 2017-07-06 12:43:53 | NULL  |
| DIAGNOSTICS.INCLUDE_RAW              | OFF  | 2017-07-06 12:43:53 | NULL  |
| PS_THREAD_TRX_INFO.MAX_LENGTH        | 65535 | 2017-07-06 12:43:53 | NULL  |
| STATEMENT_PERFORMANCE_ANALYZER.LIMIT | 100  | 2017-07-06 12:43:53 | NULL  |
| STATEMENT_PERFORMANCE_ANALYZER.VIEW  | NULL  | 2017-07-06 12:43:53 | NULL  |
| STATEMENT_TRUNCATE_LEN              | 64    | 2017-07-06 12:43:53 | NULL  |
+--------------------------------------+-------+---------------------+--------+
6 ROWS IN SET (0.00 SEC)
```

PS：对SYS_CONFIG表的INSERT和UPDATE操作会触发SYS_CONFIG_INSERT_SET_USER和SYS_CONFIG_UPDATE_SET_USER触发器，而该触发器在5.7.X版本中新增了一个用户MYSQL.SYS，且这俩触发器定义时指定了DEFINER=`MYSQL.SYS`@`LOCALHOST`（表示该触发器只能用MYSQL.SYS用户调用），SO..该用户必须存在(对MYSQL 做安全加固的小朋友要注意了，别直接对MYSQL.USER表做TRUNCATE之类的操作，先看一眼表中存在着哪些用户)，否则对SYS_CONFIG表操作时就算是超级管理员用户也无法修改（报错：ERROR 1449 (HY000): THE USER SPECIFIED AS A DEFINER ('MYSQL.SYS'@'LOCALHOST') DOES NOT EXIST），如果不小心删除了MYSQL.SYS用户 ，可以使用如下语句重新创建(注意：使用CREATE语句创建用户会失败，报错：ERROR 1396 (HY000): OPERATION CREATE USER FAILED FOR 'MYSQL.SYS'@'LOCALHOST'，所以，强烈不建议删除MYSQL.SYS用户，因为GRANT创建用户的语法即将废弃，当然，如果在不支持GRANT语句创建用户的MYSQL版本中删了MYSQL.SYS用户，也有办法补救，比如：直接INSERT用户权限表或者DROP掉触发器再指定INVOKER=`MYSQL.SYS`@`LOCALHOST`)

```sql
GRANT TRIGGER ON SYS.* TO 'MYSQL.SYS'@'LOCALHOST' IDENTIFIED BY 'LETSG0';
# 注意：MYSQL.SYS用户初始化默认对表SYS.SYS_CONFIG表只有SELECT权限，无法调用SYS_CONFIG_INSERT_SET_USER和SYS_CONFIG_UPDATE_SET_USER触发器完成更新SET_BY字段为当前操作用户名，会报错
# ERROR 1143 (42000): UPDATE COMMAND DENIED TO USER 'MYSQL.SYS'@'LOCALHOST' FOR COLUMN 'SET_BY' IN TABLE 'SYS_CONFIG'，所以要实现这个功能，针对SYS.SYS_CONFIG表还需要添加INSERT和UPDATE权限给MYSQL.SYS用户
GRANT SELECT,INSERT,UPDATE ON SYS.SYS_CONFIG TO 'MYSQL.SYS'@'LOCALHOST' IDENTIFIED BY 'LETSG0';
```

## SYS_CONFIG_INSERT_SET_USER触发器

当对SYS_CONFIG表执行INSERT语句添加配置选项行时，SYS_CONFIG_INSERT_SET_USER触发器会将SYS_CONFIG表的SET_BY列设置为当前用户名。

- 注意事项：要使得该触发器生效，有如下三个条件： 

  ***** MYSQL.SYS用户必须存在，因为定义语句中DEFINER='MYSQL.SYS'@'LOCALHOST' 表示只有该用户才能够调用该触发器，当然，为了方便，你可以删掉这个触发器，然后使用INVOKER='MYSQL.SYS'@'LOCALHOST'子句创建 

- ***** MYSQL.SYS用户初始化默认对SYS.SYS_CONFIG表只有SELECT权限，无法调用SYS_CONFIG_INSERT_SET_USER和SYS_CONFIG_UPDATE_SET_USER触发器完成更新SET_BY字段为当前操作用户名，会报错ERROR 1143 (42000): UPDATE COMMAND DENIED TO USER 'MYSQL.SYS'@'LOCALHOST' FOR COLUMN 'SET_BY' IN TABLE 'SYS_CONFIG'，所以要实现这个功能，针对SYS.SYS_CONFIG表还需要添加INSERT和UPDATE权限给MYSQL.SYS用户 

- ***** @SYS.IGNORE_SYS_CONFIG_TRIGGERS自定义变量必须为0值，任何非0值将导致该触发器不执行更新SET_BY字段操作

SYS_CONFIG_INSERT_SET_USER触发器定义语句如下：

```sql
DROP TRIGGER IF EXISTS SYS_CONFIG_INSERT_SET_USER;
DELIMITER $$
CREATE DEFINER='MYSQL.SYS'@'LOCALHOST' TRIGGER SYS_CONFIG_INSERT_SET_USER BEFORE INSERT ON SYS_CONFIG
FOR EACH ROW
BEGIN
IF @SYS.IGNORE_SYS_CONFIG_TRIGGERS != TRUE AND NEW.SET_BY IS NULL THEN
    SET NEW.SET_BY = USER();
END IF;
END$$
DELIMITER ;
```

## SYS_CONFIG_UPDATE_SET_USER触发器

当对SYS_CONFIG表执行UPDATE语句添加配置选项行时，SYS_CONFIG_UPDATE_SET_USER触发器会将SYS_CONFIG表的SET_BY列设置为当前用户名

- 注意事项：同SYS_CONFIG_INSERT_SET_USER触发器注意事项

SYS_CONFIG_UPDATE_SET_USER触发器定义语句如下：

```
DROP TRIGGER IF EXISTS SYS_CONFIG_UPDATE_SET_USER;
DELIMITER $$
CREATE DEFINER='MYSQL.SYS'@'LOCALHOST' TRIGGER SYS_CONFIG_UPDATE_SET_USER BEFORE UPDATE ON SYS_CONFIG
FOR EACH ROW
BEGIN
IF @SYS.IGNORE_SYS_CONFIG_TRIGGERS != TRUE AND NEW.SET_BY IS NULL THEN
    SET NEW.SET_BY = USER();
END IF;
END$$
DELIMITER ;
```

内容参考链接如下：

- HTTPS://DEV.MYSQL.COM/DOC/REFMAN/5.7/EN/SYS-SYS-CONFIG-UPDATE-SET-USER.HTML
- HTTPS://DEV.MYSQL.COM/DOC/REFMAN/5.7/EN/SYS-SCHEMA-TABLES.HTML
- HTTPS://DEV.MYSQL.COM/DOC/REFMAN/5.7/EN/SYS-SYS-CONFIG-INSERT-SET-USER.HTML
- HTTPS://DEV.MYSQL.COM/DOC/REFMAN/5.7/EN/SYS-SYS-CONFIG.HTML



# 按 host 分组统计视图

## host_summary_by_file_io,x$host_summary_by_file_io



按主机（与用户账号组成中的host值相同）分组统计的文件I/O的IO总数和IO延迟时间，默认按照总I/O等待时间降序排序。数据来源：performance_schema.events_waits_summary_by_host_by_event_name表，调用了sys.format_time()自定义函数、sum()聚合函数对查询结果进行求和运算并转换时间单位。


下面我们看看使用该视图查询返回的结果集。

```sql
# 从查询的结果中可以看到，延迟时间带有单位秒，对人类来说更易读
mysql> SELECT * FROM host_summary_by_file_io;
+------------+-------+------------+
| host      | ios  | io_latency |
+------------+-------+------------+
| localhost  | 67570 | 5.38 s    |
| background |  3468 | 4.18 s    |
+------------+-------+------------+
# 带x$前缀的同名视图范围的时间值未经过可读格式装换，单位为皮秒（万亿分之一秒，可读性比较差）
mysql> SELECT * FROM x$host_summary_by_file_io;
+------------+-------+---------------+
| host      | ios  | io_latency    |
+------------+-------+---------------+
| localhost  | 67574 | 5380678125144 |
| background |  3474 | 4758696829416 |
+------------+-------+---------------+
```



视图字段含义如下：

- host：客户端连接的主机名或IP。在Performance Schema表中的HOST列为NULL的行在这里假定为后台线程，且在该视图host列显示为background
- ios：文件I/O事件总次数，即可以认为就是io总数
- io_latency：文件I/O事件的总等待时间（执行时间）



PS：没有x$前缀的视图旨在提供对用户更加友好和更易于阅读的输出格式。而带x$前缀的视图输出的原始格式值更适用于一些工具类的程序使用。没有x$前缀的视图中将会调用如下函数中的一个或者多个进行数值单位转换再输出（后续其他视图的可读格式转换视图相同，下文不再赘述）：

- 字节值使用format_bytes()函数格式化并转换单位，详见后续章节
- 时间值使用format_time()函数格式化并转换单位。详见后续章节
- 使用format_statement()函数将SQL语句文本截断为statement_truncate_len配置选项设置的显示宽度。详见后续章节
- 路径名称使用format_path()函数截取并替换为相应的系统变量名称。详见后续章节
- 该视图只统计文件IO等待事件信息("wait/io/file/%")

## host_summary,x$ host_summary



按照主机分组统计的语句延迟（执行）时间、次数、相关的文件I/O延迟、连接数和内存分配大小等摘要信息，数据来源：performance_schema.accounts、sys.x$host_summary_by_statement_latency、sys.x$host_summary_by_file_io



下面我们看看使用该视图查询返回的结果集。

```sql
# 不带x$前缀的视图
root@localhost : sys 12:38:11> select * from host_summary limit 1\G
* 1. row *
              host: 192.168.2.122
        statements: 9
statement_latency: 13.22 ms
statement_avg_latency: 1.47 ms
      table_scans: 0
          file_ios: 11
  file_io_latency: 53.33 us
current_connections: 1
total_connections: 1
      unique_users: 1
    current_memory: 0 bytes
total_memory_allocated: 0 bytes
1 row in set (0.01 sec)
# 带x$前缀的视图
root@localhost : sys 12:38:14> select * from x$host_summary limit 1\G
* 1. row *
              host: 192.168.2.122
        statements: 9
statement_latency: 13218739000
statement_avg_latency: 1468748777.7778
      table_scans: 0
          file_ios: 11
  file_io_latency: 53332848
current_connections: 1
total_connections: 1
      unique_users: 1
    current_memory: 0
total_memory_allocated: 0
1 row in set (0.01 sec)
```



视图字段含义如下：

- host：客户端连接的主机名或IP。在Performance Schema表中的HOST列为NULL的行在这里假定为后台线程，且在该视图host列显示为background
- statements：语句总执行次数
- statement_latency：语句总延迟时间（执行时间）
- statement_avg_latency：语句的平均延迟时间(执行时间)
- table_scans：语句的表扫描总次数
- file_ios：文件I/O事件总次数
- file_io_latency：文件I/O事件总延迟时间（执行时间）
- current_connections：当前连接数
- total_connections：总历史连接数
- unique_users：不同（去重）用户数量
- current_memory：当前内存使用量
- total_memory_allocated：总的内存分配量



PS：该视图只统计文件IO等待事件信息(`wait/io/file/%`)

## host_summary_by_file_io_type,x$host_summary_by_file_io_type



按照主机和事件名称分组的文件I/O事件次数、延迟统计信息，默认按照主机和总I/O延迟时间降序排序。数据来源：performance_schema.events_waits_summary_by_host_by_event_name，调用了sys.format_time()自定义函数转换时间单位。

下面我们看看使用该视图查询返回的结果集。

```sql
# 不带x$前缀的视图
root@localhost : sys 12:39:51> select * from host_summary_by_file_io_type limit 3;
+---------------+--------------------------------------+-------+---------------+-------------+
| host          | event_name                          | total | total_latency | max_latency |
+---------------+--------------------------------------+-------+---------------+-------------+
| 192.168.2.122 | wait/io/file/sql/binlog              |    11 | 53.33 us      | 24.33 us    |
| background    | wait/io/file/innodb/innodb_data_file |  1631 | 5.85 s        | 35.48 ms    |
| background    | wait/io/file/sql/FRM                |  2151 | 3.89 s        | 26.10 ms    |
+---------------+--------------------------------------+-------+---------------+-------------+
3 rows in set (0.01 sec)
# 带x$前缀的视图
root@localhost : sys 12:39:54> select * from x$host_summary_by_file_io_type limit 3;
+---------------+--------------------------------------+-------+---------------+-------------+
| host          | event_name                          | total | total_latency | max_latency |
+---------------+--------------------------------------+-------+---------------+-------------+
| 192.168.2.122 | wait/io/file/sql/binlog              |    11 |      53332848 |    24334839 |
| background    | wait/io/file/innodb/innodb_data_file |  1631 | 5851714703037 | 35476899531 |
| background    | wait/io/file/sql/FRM                |  2151 | 3894316306089 | 26099526756 |
+---------------+--------------------------------------+-------+---------------+-------------+
3 rows in set (0.00 sec)
```



视图字段含义如下：

- host：客户端连接的主机名或IP。在Performance Schema表中的HOST列为NULL的行在这里假定为后台线程，且在该视图host列显示为background
- EVENT_NAME：文件I/O事件名称
- total：文件I/O事件发生总次数
- total_latency：文件I/O事件的总延迟时间（执行时间）
- max_latency：文件I/O事件的单次最大延迟时间（执行时间）

PS：该视图只统计文件IO等待事件信息(`wait/io/file/%`)



## host_summary_by_stages,x$host_summary_by_stages



按照主机和事件名称分组的阶段事件总次数、总执行时间、平均执行时间等统计信息，默认按照主机和总的延迟（执行）时间降序排序。数据来源：performance_schema.events_stages_summary_by_host_by_event_name，调用了sys.format_time()自定义函数转换时间单位。



下面我们看看使用该视图查询返回的结果集。

```sql
# 不带x$前缀的视图
root@localhost : sys 12:39:57> select * from host_summary_by_stages limit 3;
+------------+-------------------------------+-------+---------------+-------------+
| host      | event_name                    | total | total_latency | avg_latency |
+------------+-------------------------------+-------+---------------+-------------+
| background | stage/innodb/buffer pool load |    1 | 4.68 s        | 4.68 s      |
+------------+-------------------------------+-------+---------------+-------------+
1 row in set (0.00 sec)
# 带x$前缀的视图
root@localhost : sys 12:40:15> select * from x$host_summary_by_stages limit 3;
+------------+-------------------------------+-------+---------------+---------------+
| host      | event_name                    | total | total_latency | avg_latency  |
+------------+-------------------------------+-------+---------------+---------------+
| background | stage/innodb/buffer pool load |    1 | 4678671071000 | 4678671071000 |
+------------+-------------------------------+-------+---------------+---------------+
1 row in set (0.00 sec)
```

视图字段含义如下：

- host：客户端连接的主机名或IP。在Performance Schema表中的HOST列为NULL的行在这里假定为后台线程，且在该视图host列显示为background
- EVENT_NAME：阶段事件名称
- total：阶段事件总发生次数
- total_latency：阶段事件总延迟(执行)时间
- avg_latency：阶段事件平均延迟(执行)时间

## host_summary_by_statement_latency,x$host_summary_by_statement_latency


按照主机和事件名称分组的语句事件总次数、总执行时间、最大执行时间、锁时间以及数据行相关的统计信息，默认按照总延迟（执行）时间降序排序。数据来源：performance_schema.events_statements_summary_by_host_by_event_name


下面我们看看使用该视图查询返回的结果集。

```sql
# 不带x$前缀的视图
root@localhost : sys 12:40:19> select * from host_summary_by_statement_latency limit 3;
+---------------+-------+---------------+-------------+--------------+-----------+---------------+---------------+------------+
| host          | total | total_latency | max_latency | lock_latency | rows_sent | rows_examined | rows_affected | full_scans |
+---------------+-------+---------------+-------------+--------------+-----------+---------------+---------------+------------+
| localhost    |  3447 | 539.61 ms    | 89.37 ms    | 131.90 ms    |      3023 |        40772 |            0 |        108 |
| 192.168.2.122 |    9 | 13.22 ms      | 12.55 ms    | 0 ps        |        5 |            0 |            0 |          0 |
| background    |    0 | 0 ps          | 0 ps        | 0 ps        |        0 |            0 |            0 |          0 |
+---------------+-------+---------------+-------------+--------------+-----------+---------------+---------------+------------+
3 rows in set (0.01 sec)
# 带x$前缀的视图
root@localhost : sys 12:40:36> select * from x$host_summary_by_statement_latency limit 3;
+---------------+-------+---------------+-------------+--------------+-----------+---------------+---------------+------------+
| host          | total | total_latency | max_latency | lock_latency | rows_sent | rows_examined | rows_affected | full_scans |
+---------------+-------+---------------+-------------+--------------+-----------+---------------+---------------+------------+
| localhost    |  3528 |  544883806000 | 89365202000 | 132140000000 |      3026 |        41351 |            0 |        109 |
| 192.168.2.122 |    9 |  13218739000 | 12550251000 |            0 |        5 |            0 |            0 |          0 |
| background    |    0 |            0 |          0 |            0 |        0 |            0 |            0 |          0 |
+---------------+-------+---------------+-------------+--------------+-----------+---------------+---------------+------------+
3 rows in set (0.01 sec)
```



视图字段含义如下：

- host：客户端连接的主机名或IP。在Performance Schema表中的HOST列为NULL的行在这里假定为后台线程，且在该视图host列显示为background
- total：语句总执行次数
- total_latency：语句总延迟(执行)时间
- max_latency：语句单个最大延迟(执行)时间
- lock_latency：语句总锁延迟(执行）时间
- rows_sent：语句返回给客户端的总数据行数
- rows_examined：语句从存储引擎层读取的总数据行数
- rows_affected：语句执行时受影响（DML会返回数据发生变更的受影响行数，select等不会产生数据变更的语句执行时不会有受影响行数返回）的总数据行数
- full_scans：语句全表扫描总次数

## host_summary_by_statement_type,x$host_summary_by_statement_type



按照主机和语句分组的当前语句事件总次数、总执行时间、最大执行时间、锁时间以及数据行相关的统计信息（与performance_schema.host_summary_by_statement_latency 视图比起来，该视图只返回执行时间不为0的统计信息，且多了一个statement字段显示语句事件名称层级中的最后一部分字符），数据来源:performance_schema.events_statements_summary_by_host_by_event_name



下面我们看看使用该视图查询返回的结果集。

```sql
# 不带x$前缀的视图
root@localhost : sys 12:40:40> select * from host_summary_by_statement_type limit 3;
+---------------+----------------+-------+---------------+-------------+--------------+-----------+---------------+---------------+------------+
| host          | statement      | total | total_latency | max_latency | lock_latency | rows_sent | rows_examined | rows_affected | full_scans |
+---------------+----------------+-------+---------------+-------------+--------------+-----------+---------------+---------------+------------+
| 192.168.2.122 | select        |    5 | 12.92 ms      | 12.55 ms    | 0 ps        |        5 |            0 |            0 |          0 |
| 192.168.2.122 | set_option    |    3 | 258.22 us    | 166.40 us  | 0 ps        |        0 |            0 |            0 |          0 |
| 192.168.2.122 | Register Slave |    1 | 37.68 us      | 37.68 us    | 0 ps        |        0 |            0 |            0 |          0 |
+---------------+----------------+-------+---------------+-------------+--------------+-----------+---------------+---------------+------------+
3 rows in set (0.00 sec)
# 带x$前缀的视图
root@localhost : sys 12:41:00> select * from x$host_summary_by_statement_type limit 3;
+---------------+----------------+-------+---------------+-------------+--------------+-----------+---------------+---------------+------------+
| host          | statement      | total | total_latency | max_latency | lock_latency | rows_sent | rows_examined | rows_affected | full_scans |
+---------------+----------------+-------+---------------+-------------+--------------+-----------+---------------+---------------+------------+
| 192.168.2.122 | select        |    5 |  12922834000 | 12550251000 |            0 |        5 |            0 |            0 |          0 |
| 192.168.2.122 | set_option    |    3 |    258224000 |  166400000 |            0 |        0 |            0 |            0 |          0 |
| 192.168.2.122 | Register Slave |    1 |      37681000 |    37681000 |            0 |        0 |            0 |            0 |          0 |
+---------------+----------------+-------+---------------+-------------+--------------+-----------+---------------+---------------+------------+
3 rows in set (0.01 sec)
```

视图字段含义如下：

- statement：显示语句事件名称层级中的最后一部分字符，如：statement/com/Prepare instruments，在statement字段中就显示Prepare
- 其他字段含义与performance_schema.host_summary_by_statement_latency 视图字段含义相同


参考链接如下：

- https://dev.mysql.com/doc/refman/5.7/en/sys-host-summary-by-statement-type.html
- https://dev.mysql.com/doc/refman/5.7/en/sys-schema-views.html
- https://dev.mysql.com/doc/refman/5.7/en/sys-host-summary-by-file-io.html
- https://dev.mysql.com/doc/refman/5.7/en/sys-host-summary-by-file-io-type.html
- https://dev.mysql.com/doc/refman/5.7/en/sys-host-summary-by-stages.html
- https://dev.mysql.com/doc/refman/5.7/en/sys-host-summary-by-statement-latency.html
- https://dev.mysql.com/doc/refman/5.7/en/sys-host-summary.html

# 按 user 分组统计视图

## user_summary,x$user_summary

查看活跃连接中按用户分组的总执行时间、平均执行时间、总的IOS、总的内存使用量、表扫描数量等统计信息，默认按照总延迟时间(执行时间)降序排序。数据来源：
*  `performance_schema.accounts`
*  `sys.x$user_summary_by_statement_latency`
*  `sys.x$user_summary_by_file_io`
*  `sys.x$memory_by_user_by_current_bytes`

下面我们看看使用该视图查询返回的结果。

```sql
# 不带x$前缀的视图
admin@localhost : sys 12:54:32> select * from user_summary limit 1\G
* 1. row *
              user: admin
        statements: 90530
statement_latency: 2.09 h
statement_avg_latency: 83.12 ms
      table_scans: 498
          file_ios: 60662
  file_io_latency: 31.05 s
current_connections: 4
total_connections: 1174
      unique_hosts: 2
    current_memory: 85.34 MiB
total_memory_allocated: 7.21 GiB
1 row in set (0.04 sec)
# 带x$前缀的视图
admin@localhost : sys 12:55:48> select * from x$user_summary limit 1\G
* 1. row *
              user: admin
        statements: 90752
statement_latency: 7524792139504000
statement_avg_latency: 82915992369.3583
      table_scans: 500
          file_ios: 60662
  file_io_latency: 31053125849250
current_connections: 4
total_connections: 1174
      unique_hosts: 2
    current_memory: 89381384
total_memory_allocated: 7755173436
1 row in set (0.02 sec)
```

视图字段含义如下：

- user：客户端访问用户名。如果在performance_schema表中user列为NULL，则假定为后台线程，该字段为'background',如果为前台线程，则该字段对应具体的用户名
- statements：对应用户执行的语句总数量
- statement_latency：对应用户执行的语句总延迟时间(执行时间)
- statement_avg_latency：对应用户执行的语句中，平均每个语句的延迟时间(执行时间)(SUM(stmt.total_latency/SUM(stmt.total))
- table_scans：对应用户执行的语句发生表扫描总次数
- file_ios：对应用户执行的语句产生的文件I/O事件总次数
- file_io_latency：对应用户执行的语句产生的文件I/O事件的总延迟时间(执行时间)
- current_connections：对应用户的当前连接数
- total_connections：对应用户的历史总连接数
- unique_hosts：对应用户来自不同主机(针对主机名去重)连接的数量
- current_memory：对应用户的连接当前已使用的内存分配量
- total_memory_allocated：对应用户的连接的历史内存分配量

PS：该视图只统计文件IO等待事件信息(`wait/io/file/%`)

## user_summary_by_file_io,x$user_summary_by_file_io

按照用户分组的文件I/O延迟时间、IOS统计信息，默认按照总文件I/O时间延迟时间(执行时间)降序排序。数据来源：
* `performance_schema.events_waits_summary_by_user_by_event_name`


下面我们看看使用该视图查询返回的结果。

```sql
# 不带x$前缀的视图
admin@localhost : sys 12:56:18> select * from user_summary_by_file_io limit 3;
+------------+-------+------------+
| user      | ios  | io_latency |
+------------+-------+------------+
| admin      | 30331 | 15.53 s    |
| background | 10119 | 2.49 s    |
| qfsys      |  281 | 4.69 ms    |
+------------+-------+------------+
3 rows in set (0.01 sec)
# 带x$前缀的视图
admin@localhost : sys 12:56:21> select * from x$user_summary_by_file_io limit 3;
+------------+-------+----------------+
| user      | ios  | io_latency    |
+------------+-------+----------------+
| admin      | 30331 | 15526562924625 |
| background | 10122 |  2489231563125 |
| qfsys      |  281 |    4689150375 |
+------------+-------+----------------+
3 rows in set (0.00 sec)
```



视图字段含义如下：

- user：客户端用户名。如果在performance_schema表中user列为NULL，则假定为后台线程，该字段为'background',如果为前台线程，则该字段对应具体的用户名
- ios：对应用户的文件I/O事件总次数
- io_latency：对应用户的文件I/O事件的总延迟时间(执行时间)

PS：该视图只统计文件IO等待事件信息("wait/io/file/%")

## user_summary_by_file_io_type,x$user_summary_by_file_io_type

按照用户和事件类型(事件名称)分组的文件I/O延迟和IOS统计信息，默认情况下按照用户名和总文件I/O时间延迟时间(执行时间)降序排序。数据来源：`performance_schema.events_waits_summary_by_user_by_event_name`

下面我们看看使用该视图查询返回的结果。

```sql
# 不带x$前缀的视图
admin@localhost : sys 12:56:24> select * from user_summary_by_file_io_type limit 3;
+-------+-------------------------------------+-------+---------+-------------+
| user  | event_name                          | total | latency | max_latency |
+-------+-------------------------------------+-------+---------+-------------+
| admin | wait/io/file/sql/io_cache          | 27955 | 10.53 s | 67.61 ms    |
| admin | wait/io/file/innodb/innodb_log_file |  912 | 2.14 s  | 28.22 ms    |
| admin | wait/io/file/sql/binlog            |  879 | 2.05 s  | 31.75 ms    |
+-------+-------------------------------------+-------+---------+-------------+
3 rows in set (0.00 sec)
# 带x$前缀的视图
admin@localhost : sys 12:56:48> select * from x$user_summary_by_file_io_type limit 3;
+-------+-------------------------------------+-------+----------------+-------------+
| user  | event_name                          | total | latency        | max_latency |
+-------+-------------------------------------+-------+----------------+-------------+
| admin | wait/io/file/sql/io_cache          | 27955 | 10534662677625 | 67608294000 |
| admin | wait/io/file/innodb/innodb_log_file |  912 |  2143870695375 | 28216455000 |
| admin | wait/io/file/sql/binlog            |  879 |  2054976453000 | 31745275125 |
+-------+-------------------------------------+-------+----------------+-------------+
3 rows in set (0.01 sec)
```

视图字段含义如下：

- user：客户端用户名。如果在performance_schema表中user列为NULL，则假定为后台线程，该字段为'background',如果为前台线程，则该字段对应具体的用户名
- EVENT_NAME：文件I/O事件名称
- total：对应用户发生的文件I/O事件总次数
- latency：对应用户的文件I/O事件的总延迟时间(执行时间)
- max_latency：对应用户的单次文件I/O事件的最大延迟时间(执行时间)

PS：该视图只统计文件IO等待事件信息("wait/io/file/%")

## user_summary_by_stages,x$user_summary_by_stages

按用户分组的阶段事件统计信息，默认情况下按照用户名和阶段事件总延迟时间(执行时间)降序排序。数据来源：performance_schema.events_stages_summary_by_user_by_event_name

下面我们看看使用该视图查询返回的结果。

```sql
# 不带x$前缀的视图
admin@localhost : sys 12:56:51> select * from user_summary_by_stages limit 3;
+------------+-------------------------------+-------+---------------+-------------+
| user      | event_name                    | total | total_latency | avg_latency |
+------------+-------------------------------+-------+---------------+-------------+
| background | stage/innodb/buffer pool load |    1 | 12.56 s      | 12.56 s    |
+------------+-------------------------------+-------+---------------+-------------+
1 row in set (0.01 sec)
# 带x$前缀的视图
admin@localhost : sys 12:57:10> select * from x$user_summary_by_stages limit 3;
+------------+-------------------------------+-------+----------------+----------------+
| user      | event_name                    | total | total_latency  | avg_latency    |
+------------+-------------------------------+-------+----------------+----------------+
| background | stage/innodb/buffer pool load |    1 | 12561724877000 | 12561724877000 |
+------------+-------------------------------+-------+----------------+----------------+
1 row in set (0.00 sec)
```



视图字段含义如下：

- user：客户端用户名。如果在performance_schema表中user列为NULL，则假定为后台线程，该字段为'background',如果为前台线程，则该字段对应具体的用户名
- EVENT_NAME：阶段事件名称
- total：对应用户的阶段事件的总次数
- total_latency：对应用户的阶段事件的总延迟时间(执行时间)
- avg_latency：对应用户的阶段事件的平均延迟时间(执行时间)

## user_summary_by_statement_latency,x$user_summary_by_statement_latency


按照用户分组的语句统计信息，默认情况下按照语句总延迟时间(执行时间)降序排序。数据来源：`performance_schema.events_statements_summary_by_user_by_event_name`

下面我们看看使用该视图查询返回的结果。

```sql
# 不带x$前缀的视图
admin@localhost : sys 12:57:13> select * from user_summary_by_statement_latency limit 3;
+------------+-------+---------------+-------------+--------------+-----------+---------------+---------------+------------+
| user      | total | total_latency | max_latency | lock_latency | rows_sent | rows_examined | rows_affected | full_scans |
+------------+-------+---------------+-------------+--------------+-----------+---------------+---------------+------------+
| admin      | 45487 | 1.05 h        | 45.66 m    | 19.02 s      |      6065 |      17578842 |          1544 |        258 |
| qfsys      |    9 | 929.43 ms    | 928.68 ms  | 0 ps        |        5 |            0 |            0 |          0 |
| background |    0 | 0 ps          | 0 ps        | 0 ps        |        0 |            0 |            0 |          0 |
+------------+-------+---------------+-------------+--------------+-----------+---------------+---------------+------------+
3 rows in set (0.00 sec)
# 带x$前缀的视图
admin@localhost : sys 12:57:34> select * from x$user_summary_by_statement_latency limit 3;
+------------+-------+------------------+------------------+----------------+-----------+---------------+---------------+------------+
| user      | total | total_latency    | max_latency      | lock_latency  | rows_sent | rows_examined | rows_affected | full_scans |
+------------+-------+------------------+------------------+----------------+-----------+---------------+---------------+------------+
| admin      | 45562 | 3762457232413000 | 2739502018445000 | 19019928000000 |      6068 |      17579421 |          1544 |        259 |
| qfsys      |    9 |    929429421000 |    928682487000 |              0 |        5 |            0 |            0 |          0 |
| background |    0 |                0 |                0 |              0 |        0 |            0 |            0 |          0 |
+------------+-------+------------------+------------------+----------------+-----------+---------------+---------------+------------+
3 rows in set (0.00 sec)
```

视图字段含义如下：

- user：客户端用户名。如果在performance_schema表中user列为NULL，则假定为后台线程，该字段为'background',如果为前台线程，则该字段对应具体的用户名
- total：对应用户执行的语句总数量
- total_latency：对应用户执行的语句总延迟时间(执行时间)
- max_latency：对应用户执行的语句单次最大延迟时间(执行时间)
- lock_latency：对应用户执行的语句锁等待的总时间
- rows_sent：对应用户执行的语句返回给客户端的总数据行数
- rows_examined：对应用户执行的语句从存储引擎读取的总数据行数
- rows_affected：对应用户执行的语句影响的总数据行数
- full_scans：对应用户执行的语句的全表扫描总次数


## user_summary_by_statement_type,x$user_summary_by_statement_type


按用户和语句事件类型（事件类型名称为语句事件的event_name截取最后一部分字符串，也是语句command类型字符串类似）分组的语句统计信息，默认情况下按照用户名和对应语句的总延迟时间(执行时间)降序排序。数据来源：performance_schema.events_statements_summary_by_user_by_event_name

下面我们看看使用该视图查询返回的结果。

```sql
# 不带x$前缀的视图
admin@localhost : sys 12:57:38> select * from user_summary_by_statement_type limit 3;
+-------+-------------+-------+---------------+-------------+--------------+-----------+---------------+---------------+------------+
| user  | statement  | total | total_latency | max_latency | lock_latency | rows_sent | rows_examined | rows_affected | full_scans |
+-------+-------------+-------+---------------+-------------+--------------+-----------+---------------+---------------+------------+
| admin | alter_table |    2 | 56.56 m      | 43.62 m    | 0 ps        |        0 |            0 |            0 |          0 |
| admin | select      |  3662 | 5.53 m        | 2.02 m      | 4.73 s      |      6000 |      17532984 |            0 |        148 |
| admin | insert      |  1159 | 36.04 s      | 337.22 ms  | 14.23 s      |        0 |            0 |          1159 |          0 |
+-------+-------------+-------+---------------+-------------+--------------+-----------+---------------+---------------+------------+
3 rows in set (0.00 sec)
# 带x$前缀的视图
admin@localhost : sys 12:57:50> select * from x$user_summary_by_statement_type limit 3;
+-------+-------------+-------+------------------+------------------+----------------+-----------+---------------+---------------+------------+
| user  | statement  | total | total_latency    | max_latency      | lock_latency  | rows_sent | rows_examined | rows_affected | full_scans |
+-------+-------------+-------+------------------+------------------+----------------+-----------+---------------+---------------+------------+
| admin | alter_table |    2 | 3393877088372000 | 2617456143674000 |              0 |        0 |            0 |            0 |          0 |
| admin | select      |  3663 |  331756087959000 |  121243627173000 |  4733109000000 |      6003 |      17533557 |            0 |        149 |
| admin | insert      |  1159 |  36041502943000 |    337218573000 | 14229439000000 |        0 |            0 |          1159 |          0 |
+-------+-------------+-------+------------------+------------------+----------------+-----------+---------------+---------------+------------+
3 rows in set (0.00 sec)
```

视图字段含义如下：

- user：客户端用户名。如果在performance_schema表中user列为NULL，则假定为后台线程，该字段为'background',如果为前台线程，则该字段对应具体的用户名
- statement：语句事件名称的最后一部分字符串，与语句的command类型字符串类似
- 其他字段含义与 user_summary_by_statement_latency,x$user_summary_by_statement_latency 视图的字段含义相同


参考链接如下：

https://dev.mysql.com/doc/refman/5.7/en/sys-user-summary-by-statement-type.html

https://dev.mysql.com/doc/refman/5.7/en/sys-user-summary-by-file-io.html

https://dev.mysql.com/doc/refman/5.7/en/sys-user-summary-by-file-io-type.html

https://dev.mysql.com/doc/refman/5.7/en/sys-user-summary-by-stages.html

https://dev.mysql.com/doc/refman/5.7/en/sys-user-summary-by-statement-latency.html

https://dev.mysql.com/doc/refman/5.7/en/sys-user-summary.html



# 按 file 分组统计视图

## io_by_thread_by_latency,x$io_by_thread_by_latency

按照thread ID、processlist ID、用户名分组的 I/O等待时间开销统计信息，默认情况下按照总I/O等待时间降序排序。数据来源：performance_schema.events_waits_summary_by_thread_by_event_name、performance_scgema.threads

下面我们看看使用该视图查询返回的结果。

```sql
# 不带x$前缀的视图
root@localhost : sys 12:42:44> select * from io_by_thread_by_latency limit 3;
+-----------------+-------+---------------+-------------+-------------+-------------+-----------+----------------+
| user            | total | total_latency | min_latency | avg_latency | max_latency | thread_id | processlist_id |
+-----------------+-------+---------------+-------------+-------------+-------------+-----------+----------------+
| buf_dump_thread |  880 | 4.67 s        | 2.94 us    | 5.30 ms    | 27.33 ms    |        40 |          NULL |
| main            |  2214 | 3.63 s        | 409.05 ns  | 2.28 ms    | 35.48 ms    |        1 |          NULL |
| root@localhost  |    21 | 88.87 ms      | 527.22 ns  | 2.03 ms    | 21.31 ms    |        49 |              7 |
+-----------------+-------+---------------+-------------+-------------+-------------+-----------+----------------+
3 rows in set (0.01 sec)

# 带x$前缀的视图
root@localhost : sys 12:43:24> select * from x$io_by_thread_by_latency limit 3;
+-----------------+-------+---------------+-------------+-----------------+-------------+-----------+----------------+
| user            | total | total_latency | min_latency | avg_latency    | max_latency | thread_id | processlist_id |
+-----------------+-------+---------------+-------------+-----------------+-------------+-----------+----------------+
| buf_dump_thread |  880 | 4667572388808 |    2938797 | 5304059238.0000 | 27331328412 |        40 |          NULL |
| main            |  2214 | 3626928831147 |      409050 | 2283656763.0000 | 35476899531 |        1 |          NULL |
| root@localhost  |    21 |  88867469637 |      527220 | 2026334846.2500 | 21312776994 |        49 |              7 |
+-----------------+-------+---------------+-------------+-----------------+-------------+-----------+----------------+
3 rows in set (0.01 sec)
```

视图字段含义如下：

- user：对于前台线程，该列显示与线程关联的account名称（user@host格式），对于后台线程，该列显示后台线程的名称
- total：I/O事件总次数
- total_latency：I/O事件的总延迟时间（执行时间）
- min_latency：I/O事件的单次最小延迟时间（执行时间）
- avg_latency：I/O事件的平均延迟时间（执行时间）
- max_latency：I/O事件的单次最大延迟时间（执行时间）
- thread_id：内部thread ID
- processlist_id：对于前台线程，该列显示为processlist ID，对于后台线程，该列显示为NULL

PS：该视图只统计文件IO等待事件信息("wait/io/file/%")


## io_global_by_file_by_bytes,x$io_global_by_file_by_bytes

按照文件路径+名称分组的全局I/O读写字节数、读写文件I/O事件数量进行统计，默认情况下按照总I/O(读写字节数)进行降序排序。数据来源：performance_schema.file_summary_by_instance

下面我们看看使用该视图查询返回的结果。

```sql
# 不带x$前缀的视图
root@localhost : sys 12:43:27> select * from io_global_by_file_by_bytes limit 3;
+---------------------------------+------------+------------+-----------+-------------+---------------+-----------+-----------+-----------+
| file                            | count_read | total_read | avg_read  | count_write | total_written | avg_write | total    | write_pct |
+---------------------------------+------------+------------+-----------+-------------+---------------+-----------+-----------+-----------+
| @@innodb_data_home_dir/ibtmp1  |          0 | 0 bytes    | 0 bytes  |        2798 | 55.53 MiB    | 20.32 KiB | 55.53 MiB |    100.00 |
| @@innodb_undo_directory/undo002 |        874 | 13.66 MiB  | 16.00 KiB |          0 | 0 bytes      | 0 bytes  | 13.66 MiB |      0.00 |
| @@innodb_data_home_dir/ibdata1  |        31 | 2.50 MiB  | 82.58 KiB |          3 | 64.00 KiB    | 21.33 KiB | 2.56 MiB  |      2.44 |
+---------------------------------+------------+------------+-----------+-------------+---------------+-----------+-----------+-----------+
3 rows in set (0.00 sec)

# 带x$前缀的视图
root@localhost : sys 12:43:44> select * from x$io_global_by_file_by_bytes limit 3;
+-----------------------------------------------+------------+------------+------------+-------------+---------------+------------+----------+-----------+
| file                                          | count_read | total_read | avg_read  | count_write | total_written | avg_write  | total    | write_pct |
+-----------------------------------------------+------------+------------+------------+-------------+---------------+------------+----------+-----------+
| /home/mysql/data/mysqldata1/innodb_ts/ibtmp1  |          0 |          0 |    0.0000 |        2798 |      58228736 | 20810.8420 | 58228736 |    100.00 |
| /home/mysql/data/mysqldata1/undo/undo002      |        874 |  14319616 | 16384.0000 |          0 |            0 |    0.0000 | 14319616 |      0.00 |
| /home/mysql/data/mysqldata1/innodb_ts/ibdata1 |        31 |    2621440 | 84562.5806 |          3 |        65536 | 21845.3333 |  2686976 |      2.44 |
+-----------------------------------------------+------------+------------+------------+-------------+---------------+------------+----------+-----------+
3 rows in set (0.00 sec)
```



视图字段含义如下：

- file：文件路径+名称
- count_read：读I/O事件总次数
- total_read：读I/O事件的总字节数
- avg_read：读I/O事件的平均字节数
- count_write：写I/O事件总次数
- total_written：写I/O事件的总字节数
- avg_write：写I/O事件的平均字节数
- total：读写I/O事件的总字节数
- write_pct：写I/O事件字节数占文件读写I/O事件的总字节数（读和写总字节数）的百分比


## io_global_by_file_by_latency,x$io_global_by_file_by_latency

按照文件路径+名称分组的全局I/O事件的时间开销统计信息，默认情况下按照文件总的I/O等待时间(读和写的I/O等待时间)进行降序排序。数据来源：performance_schema.file_summary_by_instance

下面我们看看使用该视图查询返回的结果。

```sql
# 不带x$前缀的视图
admin@localhost : sys 09:34:01> admin@localhost : sys 09:34:01> select * from io_global_by_file_by_latency limit 3;
+------------------------------------+-------+---------------+------------+--------------+-------------+---------------+------------+--------------+
| file                              | total | total_latency | count_read | read_latency | count_write | write_latency | count_misc | misc_latency |
+------------------------------------+-------+---------------+------------+--------------+-------------+---------------+------------+--------------+
| @@basedir/share/english/errmsg.sys |    5 | 268.13 ms    |          3 | 119.31 ms    |          0 | 0 ps          |          2 | 148.82 ms    |
| /data/mysqldata1/innodb_ts/ibtmp1  |    51 | 103.21 ms    |          0 | 0 ps        |          47 | 101.96 ms    |          4 | 1.26 ms      |
| /data/mysqldata1/undo/undo003      |  139 | 63.41 ms      |        132 | 60.72 ms    |          1 | 30.11 us      |          6 | 2.65 ms      |
+------------------------------------+-------+---------------+------------+--------------+-------------+---------------+------------+--------------+
3 rows in set (0.01 sec)

# 带x$前缀的视图
admin@localhost : sys 09:34:07> select * from x$io_global_by_file_by_latency limit 3;
+----------------------------------------------+-------+---------------+------------+--------------+-------------+---------------+------------+--------------+
| file                                        | total | total_latency | count_read | read_latency | count_write | write_latency | count_misc | misc_latency |
+----------------------------------------------+-------+---------------+------------+--------------+-------------+---------------+------------+--------------+
| /home/mysql/program/share/english/errmsg.sys |    5 |  268129329000 |          3 | 119307156000 |          0 |            0 |          2 | 148822173000 |
| /data/mysqldata1/innodb_ts/ibtmp1            |    51 |  103214655750 |          0 |            0 |          47 |  101957648625 |          4 |  1257007125 |
| /data/mysqldata1/undo/undo003                |  139 |  63405483000 |        132 |  60724181625 |          1 |      30110625 |          6 |  2651190750 |
+----------------------------------------------+-------+---------------+------------+--------------+-------------+---------------+------------+--------------+
3 rows in set (0.00 sec)
```

 视图字段含义如下：

- file：文件路径+名称
- total：I/O事件总次数
- total_latency：I/O事件的总延迟时间（执行时间）
- count_read：读I/O事件的总次数
- read_latency：读I/O事件的总延迟时间（执行时间）
- count_write：写I/O事件总次数
- write_latency：写I/O事件的总延迟时间（执行时间）
- count_misc：其他I/O事件总次数
- misc_latency：其他I/O事件的总延迟时间（执行时间）

## io_global_by_wait_by_bytes,x$io_global_by_wait_by_bytes

按照文件IO事件名称后缀进行分组的统计信息，默认情况下按照总I/O读写总字节数进行降序排序。数据来源：`performance_schema.file_summary_by_event_name`

下面我们看看使用该视图查询返回的结果。

```sql
# 不带x$前缀的视图
admin@localhost : sys 09:35:20> select * from io_global_by_wait_by_bytes limit 1\G
*************************** 1. row ***************************
event_name: innodb/innodb_data_file
      total: 843
total_latency: 439.19 ms
min_latency: 0 ps
avg_latency: 520.99 us
max_latency: 9.52 ms
count_read: 627
total_read: 13.64 MiB
  avg_read: 22.28 KiB
count_write: 60
total_written: 12.88 MiB
avg_written: 219.73 KiB
total_requested: 26.52 MiB
1 row in set (0.01 sec)

# 带x$前缀的视图
admin@localhost : sys 09:35:22> select * from x$io_global_by_wait_by_bytes limit 1\G;
*************************** 1. row ***************************
event_name: innodb/innodb_data_file
      total: 843
total_latency: 439194939750
min_latency: 0
avg_latency: 520990125
max_latency: 9521262750
count_read: 627
total_read: 14303232
  avg_read: 22812.1722
count_write: 60
total_written: 13500416
avg_written: 225006.9333
total_requested: 27803648
1 row in set (0.00 sec)
```


视图字段含义如下：

- EVENT_NAME：文件IO事件全称去掉了'wait/io/file/'前缀的名称字符串
- total：读写I/O事件发生的总次数
- total_latency：I/O事件的总延迟时间(执行时间)
- min_latency：I/O事件单次最短延迟时间（执行时间）
- avg_latency：I/O事件的平均延迟时间（执行时间）
- max_latency：I/O事件单次最大延迟时间（执行时间）
- count_read：读I/O事件的请求次数
- total_read：读I/O事件的总字节数
- avg_read：读I/O事件的平均字节数
- count_write：写I/O事件的请求次数
- total_written：写I/O事件的总字节数
- avg_written：写I/O事件的平均字节数
- total_requested：读与写I/O事件的总字节数

PS：该视图只统计文件IO等待事件信息("wait/io/file/%")

## io_global_by_wait_by_latency,x$io_global_by_wait_by_latency

按照事件名称后缀字符串分组、IO延迟时间排序的全局I/O等待时间统计信息，数据来源：performance_schema.file_summary_by_event_name

下面我们看看使用该视图查询返回的结果。

```sql
# 不带x$前缀的视图
admin@localhost : sys 09:35:52> select * from io_global_by_wait_by_latency limit 1\G
*************************** 1. row ***************************
event_name: innodb/innodb_data_file
    total: 843
total_latency: 439.19 ms
avg_latency: 520.99 us
max_latency: 9.52 ms
read_latency: 317.18 ms
write_latency: 105.05 ms
misc_latency: 16.96 ms
count_read: 627
total_read: 13.64 MiB
avg_read: 22.28 KiB
count_write: 60
total_written: 12.88 MiB
avg_written: 219.73 KiB
1 row in set (0.01 sec)

# 带x$前缀的视图
admin@localhost : sys 09:35:55> select * from x$io_global_by_wait_by_latency limit 1\G;
*************************** 1. row ***************************
event_name: innodb/innodb_data_file
    total: 843
total_latency: 439194939750
avg_latency: 520990125
max_latency: 9521262750
read_latency: 317177728125
write_latency: 105052561875
misc_latency: 16964649750
count_read: 627
total_read: 14303232
avg_read: 22812.1722
count_write: 60
total_written: 13500416
avg_written: 225006.9333
1 row in set (0.01 sec)
```

视图字段含义如下：

- EVENT_NAME：文件IO事件全称去掉了'wait/io/file/'前缀的名称字符串
- total：I/O事件的发生总次数
- total_latency：I/O事件的总延迟时间（执行时间）
- avg_latency：I/O事件的平均延迟时间（执行时间）
- max_latency：I/O事件单次最大延迟时间（执行时间）
- read_latency：读I/O事件的总延迟时间（执行时间）
- write_latency：写I/O事件的总延迟时间（执行时间）
- misc_latency：其他混杂I/O事件的总延迟时间（执行时间）
- count_read：读I/O事件的总请求次数
- total_read：读I/O事件的总字节数
- avg_read：读I/O事件的平均字节数
- count_write：写I/O事件的总请求次数
- total_written：写I/O事件的总字节数
- avg_written：写I/O事件的平均字节数

PS：该视图只统计文件IO等待事件信息("wait/io/file/%")

## latest_file_io,x$latest_file_io

按照文件名称和线程名称分组、文件IO操作开始起始排序的最新的已经执行完成的I/O等待事件信息，数据来源：performance_schema.events_waits_history_long、performance_schema.threads、information_schema.processlist

- 由于等待事件相关的instruments和consumers默认没有开启，所以该视图需要打开相关的配置之后才能查询到数据，语句如下： 

* 打开等待事件的instruments：update setup_instruments set enabled='yes',timed='yes' where name like '%wait/%'; 

* 打开等待事件的consumers：update setup_consumers set enabled='yes' where name like '%wait%';

下面我们看看使用该视图查询返回的结果。

```sql
# 不带x$前缀的视图
admin@localhost : sys 09:50:34> select * from latest_file_io limit 3;
+------------------------+-----------------------------------------+----------+-----------+-----------+
| thread                | file                                    | latency  | operation | requested |
+------------------------+-----------------------------------------+----------+-----------+-----------+
| admin@localhost:7      | /data/mysqldata1/slowlog/slow-query.log | 69.24 us | write    | 251 bytes |
| page_cleaner_thread:29 | /data/mysqldata1/innodb_ts/ibtmp1      | 93.30 us | write    | 16.00 KiB |
| page_cleaner_thread:29 | /data/mysqldata1/innodb_ts/ibtmp1      | 16.89 us | write    | 16.00 KiB |
+------------------------+-----------------------------------------+----------+-----------+-----------+
3 rows in set (0.02 sec)

# 带x$前缀的视图
admin@localhost : sys 09:50:36> select * from x$latest_file_io limit 3;
+------------------------+-----------------------------------------+----------+-----------+-----------+
| thread                | file                                    | latency  | operation | requested |
+------------------------+-----------------------------------------+----------+-----------+-----------+
| admin@localhost:7      | /data/mysqldata1/slowlog/slow-query.log | 69240000 | write    |      251 |
| page_cleaner_thread:29 | /data/mysqldata1/innodb_ts/ibtmp1      | 93297000 | write    |    16384 |
| page_cleaner_thread:29 | /data/mysqldata1/innodb_ts/ibtmp1      | 16891125 | write    |    16384 |
+------------------------+-----------------------------------------+----------+-----------+-----------+
3 rows in set (0.01 sec)
```

视图字段含义如下：

- thread：对于前台线程，显示与线程关联的帐户名和processlist id。对于后台线程，显示后台线程名称和内部thread ID
- file：文件路径+名称
- latency：I/O事件的延迟时间(执行时间)
- operation：I/O操作类型
- requested：I/O事件请求的数据字节数

PS：该视图只统计文件IO等待事件信息(`wait/io/file/%`)

内容参考链接如下：

https://dev.mysql.com/doc/refman/5.7/en/sys-latest-file-io.html

https://dev.mysql.com/doc/refman/5.7/en/sys-io-by-thread-by-latency.html

https://dev.mysql.com/doc/refman/5.7/en/sys-io-global-by-file-by-latency.html

https://dev.mysql.com/doc/refman/5.7/en/sys-io-global-by-wait-by-bytes.html

https://dev.mysql.com/doc/refman/5.7/en/sys-io-global-by-wait-by-latency.html

https://dev.mysql.com/doc/refman/5.7/en/sys-io-global-by-file-by-bytes.html


# 内存分配统计视图

## innodb_buffer_stats_by_schema,x$innodb_buffer_stats_by_schema

按照schema分组的 InnoDB buffer pool统计信息，默认按照分配的buffer size大小降序排序--allocated字段。数据来源：`information_schema.innodb_buffer_page`

视图select语句文本如下：

```sql
# 不带x$前缀的视图select语句文本
SELECT IF(LOCATE('.', ibp.table_name) = 0, 'InnoDB System', REPLACE(SUBSTRING_INDEX(ibp.table_name, '.', 1), '`', '')) AS object_schema,
  sys.format_bytes(SUM(IF(ibp.compressed_size = 0, 16384, compressed_size))) AS allocated,
  sys.format_bytes(SUM(ibp.data_size)) AS data,
  COUNT(ibp.page_number) AS pages,
  COUNT(IF(ibp.is_hashed = 'YES', 1, NULL)) AS pages_hashed,
  COUNT(IF(ibp.is_old = 'YES', 1, NULL)) AS pages_old,
  ROUND(SUM(ibp.number_records)/COUNT(DISTINCT ibp.index_name)) AS rows_cached
FROM information_schema.innodb_buffer_page ibp
WHERE table_name IS NOT NULL
GROUP BY object_schema
ORDER BY SUM(IF(ibp.compressed_size = 0, 16384, compressed_size)) DESC;

# 带x$前缀的视图select语句文本
SELECT IF(LOCATE('.', ibp.table_name) = 0, 'InnoDB System', REPLACE(SUBSTRING_INDEX(ibp.table_name, '.', 1), '`', '')) AS object_schema,
  SUM(IF(ibp.compressed_size = 0, 16384, compressed_size)) AS allocated,
  SUM(ibp.data_size) AS data,
  COUNT(ibp.page_number) AS pages,
  COUNT(IF(ibp.is_hashed = 'YES', 1, NULL)) AS pages_hashed,
  COUNT(IF(ibp.is_old = 'YES', 1, NULL)) AS pages_old,
  ROUND(IFNULL(SUM(ibp.number_records)/NULLIF(COUNT(DISTINCT ibp.index_name), 0), 0)) AS rows_cached
FROM information_schema.innodb_buffer_page ibp
WHERE table_name IS NOT NULL
GROUP BY object_schema
ORDER BY SUM(IF(ibp.compressed_size = 0, 16384, compressed_size)) DESC;
```


下面我们看看使用该视图查询返回的结果。

```sql
# 不带x$前缀的视图
admin@localhost : sys 06:15:41> select * from innodb_buffer_stats_by_schema;
+---------------+------------+-----------+-------+--------------+-----------+-------------+
| object_schema | allocated  | data      | pages | pages_hashed | pages_old | rows_cached |
+---------------+------------+-----------+-------+--------------+-----------+-------------+
| InnoDB System | 23.73 MiB  | 21.76 MiB |  1519 |            0 |        24 |      21474 |
| mysql        | 240.00 KiB | 14.57 KiB |    15 |            0 |        15 |        179 |
| xiaoboluo    | 128.00 KiB | 38.93 KiB |    8 |            0 |        5 |        982 |
| sys          | 16.00 KiB  | 354 bytes |    1 |            0 |        1 |          6 |
| 小萝卜        | 16.00 KiB  | 135 bytes |    1 |            0 |        1 |          3 |
+---------------+------------+-----------+-------+--------------+-----------+-------------+
5 rows in set (0.43 sec)

# 带x$前缀的视图
admin@localhost : sys 06:15:54> select * from x$innodb_buffer_stats_by_schema;
+---------------+-----------+----------+-------+--------------+-----------+-------------+
| object_schema | allocated | data    | pages | pages_hashed | pages_old | rows_cached |
+---------------+-----------+----------+-------+--------------+-----------+-------------+
| InnoDB System |  24887296 | 22809628 |  1519 |            0 |        24 |      21498 |
| mysql        |    245760 |    14917 |    15 |            0 |        15 |        179 |
| xiaoboluo    |    131072 |    39865 |    8 |            0 |        5 |        982 |
| sys          |    16384 |      354 |    1 |            0 |        1 |          6 |
| 小萝卜        |    16384 |      135 |    1 |            0 |        1 |          3 |
+---------------+-----------+----------+-------+--------------+-----------+-------------+
5 rows in set (0.42 sec)
```

 视图字段含义如下：

- object_schema：schema级别对象的名称，如果该表属于Innodb存储引擎，则该字段显示为InnoDB System，如果是其他引擎，则该字段显示为每个schema name.
- allocated：当前已分配给schema的总内存字节数
- data：当前已分配给schema的数据部分使用的内存字节总数
- pages：当前已分配给schema内存总页数
- pages_hashed：当前已分配给schema的自适应hash索引页总数
- pages_old：当前已分配给schema的旧页总数（位于LRU列表中的旧块子列表中的页数）
- rows_cached：buffer pool中为schema缓冲的总数据行数

## innodb_buffer_stats_by_table,x$innodb_buffer_stats_by_table

按照schema和表分组的 InnoDB buffer pool 统计信息，与sys.innodb_buffer_stats_by_schema视图类似，但是本视图是按照schema name和table name分组。数据来源：information_schema.innodb_buffer_page

视图select语句文本如下：

```sql
# 不带x$前缀的视图select语句文本
SELECT IF(LOCATE('.', ibp.table_name) = 0, 'InnoDB System', REPLACE(SUBSTRING_INDEX(ibp.table_name, '.', 1), '`', '')) AS object_schema,
  REPLACE(SUBSTRING_INDEX(ibp.table_name, '.', -1), '`', '') AS object_name,
  sys.format_bytes(SUM(IF(ibp.compressed_size = 0, 16384, compressed_size))) AS allocated,
  sys.format_bytes(SUM(ibp.data_size)) AS data,
  COUNT(ibp.page_number) AS pages,
  COUNT(IF(ibp.is_hashed = 'YES', 1, NULL)) AS pages_hashed,
  COUNT(IF(ibp.is_old = 'YES', 1, NULL)) AS pages_old,
  ROUND(SUM(ibp.number_records)/COUNT(DISTINCT ibp.index_name)) AS rows_cached
FROM information_schema.innodb_buffer_page ibp
WHERE table_name IS NOT NULL
GROUP BY object_schema, object_name
ORDER BY SUM(IF(ibp.compressed_size = 0, 16384, compressed_size)) DESC;

# 带x$前缀的视图select语句文本
SELECT IF(LOCATE('.', ibp.table_name) = 0, 'InnoDB System', REPLACE(SUBSTRING_INDEX(ibp.table_name, '.', 1), '`', '')) AS object_schema,
  REPLACE(SUBSTRING_INDEX(ibp.table_name, '.', -1), '`', '') AS object_name,
  SUM(IF(ibp.compressed_size = 0, 16384, compressed_size)) AS allocated,
  SUM(ibp.data_size) AS data,
  COUNT(ibp.page_number) AS pages,
  COUNT(IF(ibp.is_hashed = 'YES', 1, NULL)) AS pages_hashed,
  COUNT(IF(ibp.is_old = 'YES', 1, NULL)) AS pages_old,
  ROUND(IFNULL(SUM(ibp.number_records)/NULLIF(COUNT(DISTINCT ibp.index_name), 0), 0)) AS rows_cached
FROM information_schema.innodb_buffer_page ibp
WHERE table_name IS NOT NULL
GROUP BY object_schema, object_name
ORDER BY SUM(IF(ibp.compressed_size = 0, 16384, compressed_size)) DESC;
```



下面我们看看使用该视图查询返回的结果。

```sql
# 不带x$前缀的视图
root@localhost : sys 12:41:25> select * from innodb_buffer_stats_by_table limit 3;
+---------------+-------------+-----------+-----------+-------+--------------+-----------+-------------+
| object_schema | object_name | allocated | data      | pages | pages_hashed | pages_old | rows_cached |
+---------------+-------------+-----------+-----------+-------+--------------+-----------+-------------+
| InnoDB System | SYS_TABLES  | 11.58 MiB | 10.63 MiB |  741 |            0 |        3 |      36692 |
| luoxiaobo    | t_luoxiaobo | 80.00 KiB | 29.21 KiB |    5 |            0 |        0 |        1658 |
| InnoDB System | SYS_COLUMNS | 48.00 KiB | 16.03 KiB |    3 |            0 |        3 |        239 |
+---------------+-------------+-----------+-----------+-------+--------------+-----------+-------------+
3 rows in set (0.12 sec)

# 带x$前缀的视图
root@localhost : sys 12:41:41> select * from x$innodb_buffer_stats_by_table limit 3;
+---------------+-------------+-----------+----------+-------+--------------+-----------+-------------+
| object_schema | object_name | allocated | data    | pages | pages_hashed | pages_old | rows_cached |
+---------------+-------------+-----------+----------+-------+--------------+-----------+-------------+
| InnoDB System | SYS_TABLES  |  12140544 | 11154757 |  741 |            0 |        3 |      36702 |
| luoxiaobo    | t_luoxiaobo |    81920 |    29913 |    5 |            0 |        0 |        1658 |
| InnoDB System | SYS_COLUMNS |    49152 |    16412 |    3 |            0 |        3 |        239 |
+---------------+-------------+-----------+----------+-------+--------------+-----------+-------------+
3 rows in set (0.12 sec)
```

视图字段含义如下：

- object_name：表级别对象名称，通常是表名
- 其他字段含义与sys.innodb_buffer_stats_by_schema视图字段含义相同，详见 innodb_buffer_stats_by_schema,x$innodb_buffer_stats_by_schema视图解释部分。但这些字段是按照object_name表级别统计的

## memory_by_host_by_current_bytes,x$memory_by_host_by_current_bytes

按照客户端主机名分组的内存使用统计信息，默认情况下按照当前内存使用量降序排序，数据来源：performance_schema.memory_summary_by_host_by_event_name

- memory类型的事件默认情况下只启用了performance_schema自身的instruments，要监控用户访问，需要单独配置，如下： 
- 开启所有的memory类型的instruments：`update setup_instruments set enabled='yes' where name like '%memory/%';`

下面我们看看使用该视图查询返回的结果。

```sql
# 不带x$前缀的视图
admin@localhost : sys 10:01:37> select * from memory_by_host_by_current_bytes limit 3;
+-------------+--------------------+-------------------+-------------------+-------------------+-----------------+
| host        | current_count_used | current_allocated | current_avg_alloc | current_max_alloc | total_allocated |
+-------------+--------------------+-------------------+-------------------+-------------------+-----------------+
| 10.10.20.14 |              58256 | 35.83 MiB        | 645 bytes        | 14.20 MiB        | 2.36 GiB        |
| localhost  |                32 | 903.11 KiB        | 28.22 KiB        | 819.00 KiB        | 7.69 MiB        |
| background  |                  5 | 176 bytes        | 35 bytes          | 160 bytes        | 352.57 KiB      |
+-------------+--------------------+-------------------+-------------------+-------------------+-----------------+
3 rows in set (0.01 sec)

# 带x$前缀的视图
admin@localhost : sys 10:02:19> select * from x$memory_by_host_by_current_bytes limit 3;
+-------------+--------------------+-------------------+-------------------+-------------------+-----------------+
| host        | current_count_used | current_allocated | current_avg_alloc | current_max_alloc | total_allocated |
+-------------+--------------------+-------------------+-------------------+-------------------+-----------------+
| 10.10.20.14 |              58256 |          37569266 |          644.8995 |          14885584 |      2538394110 |
| localhost  |                18 |            891658 |        49536.5556 |            838656 |        9821551 |
| background  |                  5 |              176 |          35.2000 |              160 |          361068 |
+-------------+--------------------+-------------------+-------------------+-------------------+-----------------+
3 rows in set (0.00 sec)
```

视图字段含义如下：

- host：客户端连接的主机名或IP。在Performance Schema表中的HOST列为NULL的行在这里假定为后台线程，且在该视图host列显示为background
- current_count_used：当前已分配的且未释放的内存块对应的内存分配次数（内存事件调用次数，该字段是快捷值，来自：performance_schema.memory_summary_by_host_by_event_name表的内存总分配次数字段COUNT_ALLOC - 内存释放次数COUNT_FREE）
- current_allocated：当前已分配的且未释放的内存字节数
- current_avg_alloc：当前已分配的且未释放的内存块对应的平均每次内存分配的内存字节数(current_allocated/current_count_used)
- current_max_alloc：当前已分配的且未释放的单次最大内存分配字节数
- total_allocated：总的已分配内存字节数

## memory_by_thread_by_current_bytes,x$memory_by_thread_by_current_bytes

按照thread ID分组的内存使用统计信息（只统计前台线程），默认情况下按照当前内存使用量进行降序排序，数据来源：
* `performance_schema.memory_summary_by_thread_by_event_name`
* `performance_schema.threads`

下面我们看看使用该视图查询返回的结果。

```sql
# 不带x$前缀的视图
admin@localhost : sys 10:04:07> select * from memory_by_thread_by_current_bytes limit 3;
+-----------+--------------------------+--------------------+-------------------+-------------------+-------------------+-----------------+
| thread_id | user                    | current_count_used | current_allocated | current_avg_alloc | current_max_alloc | total_allocated |
+-----------+--------------------------+--------------------+-------------------+-------------------+-------------------+-----------------+
|        45 | admin@localhost          |                34 | 4.91 MiB          | 147.98 KiB        | 4.00 MiB          | 29.36 MiB      |
|        41 | innodb/dict_stats_thread |                  5 | 176 bytes        | 35 bytes          | 160 bytes        | 346.31 KiB      |
|        47 | admin@localhost          |                  3 | 112 bytes        | 37 bytes          | 80 bytes          | 8.17 KiB        |
+-----------+--------------------------+--------------------+-------------------+-------------------+-------------------+-----------------+
3 rows in set (0.13 sec)

# 带x$前缀的视图
admin@localhost : sys 10:04:58> select * from x$memory_by_thread_by_current_bytes limit 3;
+-----------+--------------------------+--------------------+-------------------+-------------------+-------------------+-----------------+
| thread_id | user                    | current_count_used | current_allocated | current_avg_alloc | current_max_alloc | total_allocated |
+-----------+--------------------------+--------------------+-------------------+-------------------+-------------------+-----------------+
|        45 | admin@localhost          |                19 |          5102538 |      268554.6316 |          4194304 |        44995979 |
|        41 | innodb/dict_stats_thread |                  5 |              176 |          35.2000 |              160 |          354620 |
|        47 | admin@localhost          |                  3 |              112 |          37.3333 |                80 |            8368 |
+-----------+--------------------------+--------------------+-------------------+-------------------+-------------------+-----------------+
3 rows in set (0.12 sec)
```



视图字段含义如下：

- thread_id：内部thread ID
- user：对于前台线程，该字段显示为account名称，对于后台线程，该字段显示后台线程名称
- 其他字段含义与sys.memory_by_host_by_current_bytes视图的字段含义相同，详见 memory_by_host_by_current_bytes,x$memory_by_host_by_current_bytes视图解释部分。但是与该视图不同的是本视图是按照线程分组统计的

## memory_by_user_by_current_bytes,x$memory_by_user_by_current_bytes

按照用户分组的内存使用统计信息，默认按照当前内存使用量进行降序排序，数据来源：performance_schema.memory_summary_by_user_by_event_name

下面我们看看使用该视图查询返回的结果。

```sql
# 不带x$前缀的视图
admin@localhost : sys 10:05:02> select * from memory_by_user_by_current_bytes limit 3;
+------------+--------------------+-------------------+-------------------+-------------------+-----------------+
| user      | current_count_used | current_allocated | current_avg_alloc | current_max_alloc | total_allocated |
+------------+--------------------+-------------------+-------------------+-------------------+-----------------+
| admin      |              58291 | 36.71 MiB        | 660 bytes        | 14.20 MiB        | 2.41 GiB        |
| background |                  5 | 176 bytes        | 35 bytes          | 160 bytes        | 358.17 KiB      |
| qfsys      |                  0 | 0 bytes          | 0 bytes          | 0 bytes          | 0 bytes        |
+------------+--------------------+-------------------+-------------------+-------------------+-----------------+
3 rows in set (0.01 sec)

# 带x$前缀的视图
admin@localhost : sys 10:05:21> select * from x$memory_by_user_by_current_bytes limit 3;
+------------+--------------------+-------------------+-------------------+-------------------+-----------------+
| user      | current_count_used | current_allocated | current_avg_alloc | current_max_alloc | total_allocated |
+------------+--------------------+-------------------+-------------------+-------------------+-----------------+
| admin      |              58278 |          38460932 |          659.9563 |          14885584 |      2586890836 |
| background |                  5 |              176 |          35.2000 |              160 |          366828 |
| qfsys      |                  0 |                0 |            0.0000 |                0 |              0 |
+------------+--------------------+-------------------+-------------------+-------------------+-----------------+
3 rows in set (0.01 sec)
```



视图字段含义如下：

- user：客户端用户名。对于后台线程，该字段显示为background，对于前台线程，该字段显示user名称(不是account，不包含host部分)
- 其他字段含义与sys.memory_by_host_by_current_bytes视图的字段含义相同，详见 memory_by_host_by_current_bytes,x$memory_by_host_by_current_bytes视图解释部分。但是与该视图不同的是这里是按照用户名分组统计的

## memory_global_by_current_bytes,x$memory_global_by_current_bytes

按照内存分配类型（事件类型）分组的内存使用统计信息，默认情况下按照当前内存使用量进行降序排序，数据来源：performance_schema.memory_summary_global_by_event_name

下面我们看看使用该视图查询返回的结果。

```
# 不带x$前缀的视图
admin@localhost : sys 10:05:24> select * from memory_global_by_current_bytes limit 3;
+-----------------------------------------------------------------+---------------+---------------+-------------------+------------+------------+----------------+
| event_name                                                      | current_count | current_alloc | current_avg_alloc | high_count | high_alloc | high_avg_alloc |
+-----------------------------------------------------------------+---------------+---------------+-------------------+------------+------------+----------------+
| memory/innodb/lock0lock                                        |          9166 | 14.20 MiB    | 1.59 KiB          |      9166 | 14.20 MiB  | 1.59 KiB      |
| memory/performance_schema/events_statements_history_long        |            1 | 13.66 MiB    | 13.66 MiB        |          1 | 13.66 MiB  | 13.66 MiB      |
| memory/performance_schema/events_statements_history_long.tokens |            1 | 9.77 MiB      | 9.77 MiB          |          1 | 9.77 MiB  | 9.77 MiB      |
+-----------------------------------------------------------------+---------------+---------------+-------------------+------------+------------+----------------+
3 rows in set (0.01 sec)

# 带x$前缀的视图
admin@localhost : sys 10:07:19> select * from x$memory_global_by_current_bytes limit 3;
+-----------------------------------------------------------------+---------------+---------------+-------------------+------------+------------+----------------+
| event_name                                                      | current_count | current_alloc | current_avg_alloc | high_count | high_alloc | high_avg_alloc |
+-----------------------------------------------------------------+---------------+---------------+-------------------+------------+------------+----------------+
| memory/innodb/lock0lock                                        |          9166 |      14885584 |        1624.0000 |      9166 |  14885584 |      1624.0000 |
| memory/performance_schema/events_statements_history_long        |            1 |      14320000 |    14320000.0000 |          1 |  14320000 |  14320000.0000 |
| memory/performance_schema/events_statements_history_long.tokens |            1 |      10240000 |    10240000.0000 |          1 |  10240000 |  10240000.0000 |
+-----------------------------------------------------------------+---------------+---------------+-------------------+------------+------------+----------------+
3 rows in set (0.00 sec)
```



视图字段含义如下：

- EVENT_NAME：内存事件名称
- CURRENT_COUNT：当前已分配内存且未释放的内存事件发生的总次数(内存分配次数)
- current_alloc：当前已分配内存且未释放的内存字节数
- current_avg_alloc：当前已分配内存且未释放的内存事件的平均内存字节数(平均每次内存分配的字节数)
- high_count：内存事件发生的历史最高位(高水位)次数（来自performance_schema.memory_summary_global_by_event_name表中的HIGH_COUNT_USED字段：如果CURRENT_COUNT_USED增加1是一个新的最高值，则该字段值相应增加 ）
- high_alloc：内存分配的历史最高位(高水位)字节数（来自performance_schema.memory_summary_global_by_event_name表中的HIGH_NUMBER_OF_BYTES_USED字段：如果CURRENT_NUMBER_OF_BYTES_USED增加N之后是一个新的最高值，则该字段值相应增加）
- high_avg_alloc：内存事件发生的历史最高位(高水位)次数对应的平均每次内存分配的字节数(high_number_of_bytes_used/high_count_used)

## memory_global_total,x$memory_global_total

当前总内存使用量统计（注意：只包含自memory类型的instruments启用以来被监控到的内存事件，在启用之前的无法监控，so..如果你不是在server启动之前就在配置文件中配置启动memory类型的instruments，那么此值可能并不可靠，当然如果你的server运行时间足够长，那么该值也具有一定参考价值），数据来源：performance_schema.memory_summary_global_by_event_name

下面我们看看使用该视图查询返回的结果。

```sql
# 不带x$前缀的视图
admin@localhost : sys 10:07:22> select * from memory_global_total limit 3;
+-----------------+
| total_allocated |
+-----------------+
| 168.91 MiB      |
+-----------------+
1 row in set (0.01 sec)

# 带x$前缀的视图
admin@localhost : sys 10:08:24> select * from x$memory_global_total limit 3;
+-----------------+
| total_allocated |
+-----------------+
|      177099388 |
+-----------------+
1 row in set (0.00 sec)
```

视图字段含义如下：

- total_allocated：在server中分配的内存总字节数


内容参考链接如下：

https://dev.mysql.com/doc/refman/5.7/en/sys-memory-global-total.html

https://dev.mysql.com/doc/refman/5.7/en/sys-innodb-buffer-stats-by-table.html

https://dev.mysql.com/doc/refman/5.7/en/sys-memory-by-host-by-current-bytes.html

https://dev.mysql.com/doc/refman/5.7/en/sys-memory-by-thread-by-current-bytes.html

https://dev.mysql.com/doc/refman/5.7/en/sys-memory-by-user-by-current-bytes.html

https://dev.mysql.com/doc/refman/5.7/en/sys-memory-global-by-current-bytes.html

https://dev.mysql.com/doc/refman/5.7/en/sys-innodb-buffer-stats-by-schema.html



# 等待事件统计视图 

## wait_classes_global_by_avg_latency,x$wait_classes_global_by_avg_latency

按照事件大类(等待事件名称层级中前三层组件组成的名称前缀)分组（如：wait/io/table、wait/io/file、wait/lock/table）的等待事件平均延迟时间（总IO延迟时间/总IOS）等统计信息，默认按照平均延迟时间(执行时间)降序排序。数据来源：events_waits_summary_global_by_event_name

- 该视图会忽略空闲等待事件(idle事件)信息

下面我们看看使用该视图查询返回的结果。

```sql
# 不带x$前缀的视图
admin@localhost : sys 12:58:11> select * from wait_classes_global_by_avg_latency limit 3;
+--------------------+-------+---------------+-------------+-------------+-------------+
| event_class        | total | total_latency | min_latency | avg_latency | max_latency |
+--------------------+-------+---------------+-------------+-------------+-------------+
| wait/lock/metadata |    2 | 56.57 m      | 12.94 m    | 28.28 m    | 43.63 m    |
| wait/synch/cond    |  7980 | 4.37 h        | 0 ps        | 1.97 s      | 5.01 s      |
| wait/io/socket    | 28988 | 21.02 s      | 0 ps        | 725.29 us  | 103.18 ms  |
+--------------------+-------+---------------+-------------+-------------+-------------+
3 rows in set (0.05 sec)

# 带x$前缀的视图
admin@localhost : sys 12:58:22> select * from x$wait_classes_global_by_avg_latency limit 3;
+--------------------+-------+-------------------+-----------------+-----------------------+------------------+
| event_class        | total | total_latency    | min_latency    | avg_latency          | max_latency      |
+--------------------+-------+-------------------+-----------------+-----------------------+------------------+
| wait/lock/metadata |    2 |  3393932470401750 | 776378395041375 | 1696966235200875.0000 | 2617554075360375 |
| wait/synch/cond    |  7980 | 15739342570225500 |              0 |    1972348693010.7143 |    5006888904375 |
| wait/io/socket    | 28990 |    21024710924250 |              0 |        725240114.6689 |    103181011500 |
+--------------------+-------+-------------------+-----------------+-----------------------+------------------+
3 rows in set (0.02 sec)
```



视图字段含义如下：

- event_class：事件类别，事件名称层级中前三层组件组成的名称前缀，如'wait/io/file/sql/slow_log'，截取后保留'wait/io/file' 字符串作为事件类别
- total：对应事件大类的事件总次数
- total_latency：对应事件大类的事件总延迟时间(执行时间)
- min_latency：对应事件大类的单次事件最小延迟时间(执行时间)
- avg_latency：对应事件大类中，每个事件的平均延迟时间(执行时间)
- max_latency：对应事件大类的单次事件在最大延迟时间(执行时间)

## wait_classes_global_by_latency,x$wait_classes_global_by_latency

按照事件大类(等待事件名称前三层前缀)分组（如：wait/io/table、wait/io/file、wait/lock/table）的等待事件平均延迟时间等统计信息，默认情况下按照总延迟时间(执行时间)降序排序。数据来源：events_waits_summary_global_by_event_name

下面我们看看使用该视图查询返回的结果。

```sql
# 不带x$前缀的视图
admin@localhost : sys 12:58:26> select * from wait_classes_global_by_latency limit 3;
+--------------------+----------+---------------+-------------+-------------+-------------+
| event_class        | total    | total_latency | min_latency | avg_latency | max_latency |
+--------------------+----------+---------------+-------------+-------------+-------------+
| wait/synch/cond    |    7983 | 4.38 h        | 0 ps        | 1.97 s      | 5.01 s      |
| wait/lock/metadata |        2 | 56.57 m      | 12.94 m    | 28.28 m    | 43.63 m    |
| wait/io/table      | 16096791 | 4.59 m        | 12.03 us    | 17.11 us    | 2.02 m      |
+--------------------+----------+---------------+-------------+-------------+-------------+
3 rows in set (0.02 sec)

# 带x$前缀的视图
admin@localhost : sys 12:58:40> select * from x$wait_classes_global_by_latency limit 3;
+--------------------+----------+-------------------+-----------------+-----------------------+------------------+
| event_class        | total    | total_latency    | min_latency    | avg_latency          | max_latency      |
+--------------------+----------+-------------------+-----------------+-----------------------+------------------+
| wait/synch/cond    |    7984 | 15759344050722375 |              0 |    1973865737815.9287 |    5006888904375 |
| wait/lock/metadata |        2 |  3393932470401750 | 776378395041375 | 1696966235200875.0000 | 2617554075360375 |
| wait/io/table      | 16096791 |  275441586767625 |        12026625 |        17111583.7168 |  121243803313125 |
+--------------------+----------+-------------------+-----------------+-----------------------+------------------+
3 rows in set (0.02 sec)
```



视图字段含义如下：

- 该视图字段含义和wait_classes_global_by_avg_latency,x$wait_classes_global_by_avg_latency 视图字段含义相同，只是排序字段不同而已


## waits_by_host_by_latency,x$waits_by_host_by_latency

按照主机和事件名称分组的等待事件统计信息，默认情况下按照主机名和总的等待事件延迟时间降序排序，数据来源：events_waits_summary_by_host_by_event_name

- 该视图忽略空闲等待事件（idle事件）信息

下面我们看看使用该视图查询返回的结果。

```sql
# 不带x$前缀的视图
admin@localhost : sys 12:58:43> select * from waits_by_host_by_latency limit 3;
+-------------+----------------------------------------------+-------+---------------+-------------+-------------+
| host        | event                                        | total | total_latency | avg_latency | max_latency |
+-------------+----------------------------------------------+-------+---------------+-------------+-------------+
| 10.10.20.14 | wait/io/socket/sql/client_connection        | 24568 | 20.53 s      | 835.48 us  | 70.46 ms    |
| 10.10.20.14 | wait/synch/mutex/innodb/trx_pool_mutex      |  2326 | 14.59 s      | 6.27 ms    | 215.63 ms  |
| 10.10.20.14 | wait/synch/cond/sql/MYSQL_BIN_LOG::COND_done |  1707 | 13.74 s      | 8.05 ms    | 43.33 ms    |
+-------------+----------------------------------------------+-------+---------------+-------------+-------------+
3 rows in set (0.00 sec)

# 带x$前缀的视图
admin@localhost : sys 12:59:04> select * from x$waits_by_host_by_latency limit 3;
+-------------+----------------------------------------------+-------+----------------+-------------+--------------+
| host        | event                                        | total | total_latency  | avg_latency | max_latency  |
+-------------+----------------------------------------------+-------+----------------+-------------+--------------+
| 10.10.20.14 | wait/io/socket/sql/client_connection        | 24568 | 20526083640375 |  835480125 |  70457480625 |
| 10.10.20.14 | wait/synch/mutex/innodb/trx_pool_mutex      |  2326 | 14586650782125 |  6271131000 | 215632752375 |
| 10.10.20.14 | wait/synch/cond/sql/MYSQL_BIN_LOG::COND_done |  1707 | 13737760876125 |  8047897125 |  43332152250 |
+-------------+----------------------------------------------+-------+----------------+-------------+--------------+
3 rows in set (0.01 sec)
```



视图字段含义如下：

- host：发起连接的主机名
- event：等待事件名称
- total：对应主机发生的等待事件总次数
- total_latency：对应主机的等待事件总延迟时间
- avg_latency：对应主机的等待事件的平均延迟时间
- max_latency：对应主机的单次等待事件的最大延迟时间


## waits_by_user_by_latency,x$waits_by_user_by_latency

按照用户和事件名称分组的等待事件统计信息，默认情况下按照用户名和总的等待事件延迟事件降序排序，数据来源：events_waits_summary_by_user_by_event_name

- 该视图忽略空闲等待事件（idle事件）信息

下面我们看看使用该视图查询返回的结果。

```sql
# 不带x$前缀的视图
admin@localhost : sys 12:59:07> select * from waits_by_user_by_latency limit 3;
+-------+---------------------------------------------------+----------+---------------+-------------+-------------+
| user  | event                                            | total    | total_latency | avg_latency | max_latency |
+-------+---------------------------------------------------+----------+---------------+-------------+-------------+
| admin | wait/lock/metadata/sql/mdl                        |        2 | 56.57 m      | 28.28 m    | 43.63 m    |
| admin | wait/synch/cond/sql/MDL_context::COND_wait_status |    3395 | 56.56 m      | 999.66 ms  | 1.00 s      |
| admin | wait/io/table/sql/handler                        | 16096791 | 4.59 m        | 17.11 us    | 2.02 m      |
+-------+---------------------------------------------------+----------+---------------+-------------+-------------+
3 rows in set (0.01 sec)

# 带x$前缀的视图
admin@localhost : sys 12:59:22> select * from x$waits_by_user_by_latency limit 3;
+-------+---------------------------------------------------+----------+------------------+------------------+------------------+
| user  | event                                            | total    | total_latency    | avg_latency      | max_latency      |
+-------+---------------------------------------------------+----------+------------------+------------------+------------------+
| admin | wait/lock/metadata/sql/mdl                        |        2 | 3393932470401750 | 1696966235200875 | 2617554075360375 |
| admin | wait/synch/cond/sql/MDL_context::COND_wait_status |    3395 | 3393839154564375 |    999658071750 |    1004173431750 |
| admin | wait/io/table/sql/handler                        | 16096791 |  275441586767625 |        17111250 |  121243803313125 |
+-------+---------------------------------------------------+----------+------------------+------------------+------------------+
3 rows in set (0.01 sec)
```



视图字段含义如下：

- user：与该连接关联的用户名
- 其他字段与waits_by_host_by_latency,x$waits_by_host_by_latency 视图字段含义相同，不同的是waits_by_user_by_latency,x$waits_by_user_by_latency视图是按照用户名和事件名称分组



## waits_global_by_latency,x$waits_global_by_latency

按照事件名称分组的等待事件统计信息，默认按照等待事件总延迟时间降序排序。数据来源：events_waits_summary_global_by_event_name

- 该视图忽略空闲等待事件（idle事件）信息

下面我们看看使用该视图查询返回的结果。

```sql
# 不带x$前缀的视图
admin@localhost : sys 12:59:25> select * from waits_global_by_latency limit 3;
+---------------------------------------------------+-------+---------------+-------------+-------------+
| events                                            | total | total_latency | avg_latency | max_latency |
+---------------------------------------------------+-------+---------------+-------------+-------------+
| wait/synch/cond/sql/MYSQL_BIN_LOG::update_cond    |  2891 | 3.45 h        | 4.29 s      | 5.01 s      |
| wait/lock/metadata/sql/mdl                        |    2 | 56.57 m      | 28.28 m    | 43.63 m    |
| wait/synch/cond/sql/MDL_context::COND_wait_status |  3395 | 56.56 m      | 999.66 ms  | 1.00 s      |
+---------------------------------------------------+-------+---------------+-------------+-------------+
3 rows in set (0.02 sec)

# 带x$前缀的视图
admin@localhost : sys 12:59:40> select * from x$waits_global_by_latency limit 3;
+---------------------------------------------------+-------+-------------------+------------------+------------------+
| events                                            | total | total_latency    | avg_latency      | max_latency      |
+---------------------------------------------------+-------+-------------------+------------------+------------------+
| wait/synch/cond/sql/MYSQL_BIN_LOG::update_cond    |  2892 | 12411771548807250 |    4291760563125 |    5006888904375 |
| wait/lock/metadata/sql/mdl                        |    2 |  3393932470401750 | 1696966235200875 | 2617554075360375 |
| wait/synch/cond/sql/MDL_context::COND_wait_status |  3395 |  3393839154564375 |    999658071750 |    1004173431750 |
+---------------------------------------------------+-------+-------------------+------------------+------------------+
3 rows in set (0.02 sec)
```


视图字段含义如下：

- events：等待事件名称
- 其他字段含义和`waits_by_host_by_latency`,`x$waits_by_host_by_latency` 视图字段含义相同，不同的是`waits_global_by_latency`,`x$waits_global_by_latency`视图只按照事件名称分组

内容参考链接如下：

https://dev.mysql.com/doc/refman/5.7/en/sys-waits-global-by-latency.html

https://dev.mysql.com/doc/refman/5.7/en/sys-wait-classes-global-by-latency.html

https://dev.mysql.com/doc/refman/5.7/en/sys-wait-classes-global-by-avg-latency.html

https://dev.mysql.com/doc/refman/5.7/en/sys-waits-by-host-by-latency.html

https://dev.mysql.com/doc/refman/5.7/en/sys-waits-by-user-by-latency.html


# 思维导图

<head>
    <meta charset="utf-8"/>
    <meta content="IE=edge" http-equiv="X-UA-Compatible"/>
    <title></title>
    <style>
        body{
            margin: 0;
        }
        #content-info{
            width: auto;
            margin: 0 auto;
            text-align: center;
        }
        #author-info{
            white-space: nowrap;
            text-overflow: ellipsis;
            overflow: hidden;
        }
        #title{
            text-overflow: ellipsis;
            white-space: nowrap;
            overflow: hidden;
            padding-top: 10px;
            margin-bottom: 2px;
            font-size: 34px;
            color: #505050;
        }
        .text{
            white-space:nowrap;
            text-overflow: ellipsis;
            display: inline-block;
            margin-right: 20px;
            margin-bottom: 2px;
            font-size: 20px;
            color: #8c8c8c;
        }
        #navBar{
            width: auto;
            height: auto;
            position: fixed;
            right:0;
            bottom: 0;
            background-color: #f0f3f4;
            overflow-y: auto;
            text-align: center;
        }
        #svg-container{
            width: 100%;
            overflow-x: scroll;
            min-width: 0px;
            margin: 0 10px;
        }
        #nav-thumbs{
            overflow-y: scroll;
            padding: 0 5px;
        }
        .nav-thumb{
            position: relative;
            margin: 10px auto;
        }
        .nav-thumb >p{
            text-align: center;
            font-size: 12px;
            margin: 4px 0 0 0;
        }
        .nav-thumb >div{
            position: relative;
            display: inline-block;
            border: 1px solid #c6cfd5;
        }
        .nav-thumb img{
            display: block;
        }
        #main-content{
            bottom: 0;
            left: 0;
            right: 0;
            background-color: #d0cfd8;
            display: flex;
            height: auto;
            flex-flow: row wrap;
            text-align:center;
        }
        #svg-container >svg{
            display: block;
            margin:10px auto;
            margin-bottom: 0;
        }
        #copyright{
            bottom: 0;
            left: 50%;
            margin: 5px auto;
            font-size: 16px;
            color: #515151;
        }
        #copyright >a{
            text-decoration: none;
            color: #77C;
        }
        .number{
            position: absolute;
            top:0;
            left:0;
            border-top:22px solid #08a1ef;
            border-right: 22px solid transparent;
        }
        .pagenum{
            font-size: 12px;
            color: #fff;
            position: absolute;
            top: -23px;
            left: 2px;
        }
            #navBar::-webkit-scrollbar{
            width: 8px;
            background-color: #f5f5f5;
        }
            #navBar::-webkit-scrollbar-track{
            -webkit-box-shadow: inset 0 0 4px rgba(0,0,0,.3);
            border-radius: 8px;
            background-color: #fff;
        }
            #navBar::-webkit-scrollbar-thumb{
            border-radius: 8px;
            -webkit-box-shadow: inset 0 0 4px rgba(0,0,0,.3);
            background-color: #6b6b70;
        }
        #navBar::-webkit-scrollbar-thumb:hover{
            background-color: #4a4a4f;
        }    
    </style>
</head>
<body>
<div id="main-area">
      <div id="content-info">
        <div id="content-info">
          <div id="title">MySQLSYS库</div>
        </div>
        <div id="author-info">
          <div class="text" id="author-name">大宝和聪聪</div>
          <div class="text" id="share-time">2020-11-06</div>
        </div>
      </div>
      <div id="main-content">
        <div id="svg-container" style=""><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1416 1863" width="1416" ed:vspacing="20" height="1863" xmlns:ed="https://www.edrawsoft.cn/xml/2017/SVGExtensions/" xmlns:xlink="http://www.w3.org/1999/xlink" ed:name="页面-1" id="page0" ed:hspacing="50" preserveaspectradio="xMinYMin meet" xmlns:ev="http://www.w3.org/2001/xml-events">
    <style type="text/css">
g[ed\:togtopicid],g[ed\:hyperlink],g[ed\:comment],g[ed\:note] {cursor:pointer;}
g[id] {-moz-user-select: none;-ms-user-select: none;user-select: none;}
svg text::selection,svg tspan::selection{background-color: #4285f4;color: #ffffff;fill: #ffffff;}
.st15 {fill:#444444;font-family:Helvetica Neue,Helvetica,Arial,sans-serif}
.st3 {fill:#454545;font-family:;font-size:11.25pt}
.st4 {fill:#454545;font-family:;font-size:9pt}
.st6 {fill:#454545;font-family:Apple LiSung Light;font-size:9pt}
.st10 {fill:#4d4d4c;font-family:Source Code Pro,Monaco,Menlo,Consolas,monospace}
.st12 {fill:#4d4d4c;font-family:inherit}
.st8 {fill:#555555;font-family:Open Sans,Arial,Helvetica,sans-serif;font-size:8.04pt}
.st13 {fill:#718c00;font-family:inherit}
.st11 {fill:#8959a8;font-family:inherit}
.st9 {fill:#8e908c;font-family:inherit}
.st14 {fill:#f5871f;font-family:inherit}
.st1 {fill:#ffffff;font-family:;font-size:14.25pt}
.st7 {font-family:.SF NS Text;font-size:8.04pt}
.st2 {font-family:.SF NS Text}
.st5 {font-family:Apple LiSung Light;font-size:8.57pt}
</style>
    <defs></defs>
    <rect y="0" fill="#ffffff" width="1416" height="1863" x="0"></rect>
    <path fill="none" stroke="#696969" ed:parentid="101" stroke-linejoin="round" stroke-width="1.33333" d="M-2.5,272.4L38.1,272.4L38.1,-266.4C38.1,-269.7,40.8,-272.4,44.1,-272.4L102.5,-272.4" stroke-linecap="round" transform="matrix(1,0,0,1,223.81,656.9)" id="103" ed:tosuperid="102"></path>
    <path fill="none" stroke="#696969" ed:parentid="101" stroke-linejoin="round" stroke-width="1.33333" d="M-2.5,-12.9L38.1,-12.9L38.1,6.9C38.1,10.2,40.8,12.9,44.1,12.9L102.5,12.9" stroke-linecap="round" transform="matrix(1,0,0,1,223.81,942.15)" id="105" ed:tosuperid="104"></path>
    <path fill="none" stroke="#696969" ed:parentid="915" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,-89.4L2.5,83.4C2.5,86.7,5.2,89.4,8.5,89.4L16.5,89.4" stroke-linecap="round" transform="matrix(1,0,0,1,485.78,1589.19)" id="109" ed:tosuperid="108"></path>
    <path fill="none" stroke="#696969" ed:parentid="915" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,54.9L2.5,-48.9C2.5,-52.2,5.2,-54.9,8.5,-54.9L16.5,-54.9" stroke-linecap="round" transform="matrix(1,0,0,1,485.78,1444.94)" id="115" ed:tosuperid="114"></path>
    <path fill="none" stroke="#696969" ed:parentid="915" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,116.7L2.5,116.7L2.5,-110.7C2.5,-114,5.2,-116.7,8.5,-116.7L16.5,-116.7" stroke-linecap="round" transform="matrix(1,0,0,1,485.78,1383.08)" id="117" ed:tosuperid="116"></path>
    <path fill="none" stroke="#696969" ed:parentid="393" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,-83.5L2.5,77.5C2.5,80.8,5.2,83.5,8.5,83.5L16.5,83.5" stroke-linecap="round" transform="matrix(1,0,0,1,591.78,544.23)" id="123" ed:tosuperid="122"></path>
    <path fill="none" stroke="#696969" ed:parentid="393" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,132.7L2.5,132.7L2.5,-126.7C2.5,-130,5.2,-132.7,8.5,-132.7L16.5,-132.7" stroke-linecap="round" transform="matrix(1,0,0,1,591.78,328.08)" id="127" ed:tosuperid="126"></path>
    <path fill="none" stroke="#696969" ed:parentid="393" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,48.7L2.5,-42.7C2.5,-46,5.2,-48.7,8.5,-48.7L16.5,-48.7" stroke-linecap="round" transform="matrix(1,0,0,1,591.78,412.06)" id="129" ed:tosuperid="128"></path>
    <path fill="none" stroke="#696969" ed:parentid="104" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,8.1L2.5,8.1L2.5,-2.1C2.5,-5.5,5.2,-8.1,8.5,-8.1L16.5,-8.1" stroke-linecap="round" transform="matrix(1,0,0,1,485.78,946.85)" id="177" ed:tosuperid="156"></path>
    <path fill="none" stroke="#696969" ed:parentid="156" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,-25.6L2.5,19.6C2.5,22.9,5.2,25.6,8.5,25.6L16.5,25.6" stroke-linecap="round" transform="matrix(1,0,0,1,635.69,964.29)" id="178" ed:tosuperid="164"></path>
    <path fill="none" stroke="#696969" ed:parentid="102" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,159L2.5,159L2.5,-153C2.5,-156.3,5.2,-159,8.5,-159L16.5,-159" stroke-linecap="round" transform="matrix(1,0,0,1,485.78,225.5)" id="390" ed:tosuperid="389"></path>
    <path fill="none" stroke="#696969" ed:parentid="102" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,126.5L2.5,-120.5C2.5,-123.8,5.2,-126.5,8.5,-126.5L16.5,-126.5" stroke-linecap="round" transform="matrix(1,0,0,1,485.78,257.98)" id="392" ed:tosuperid="391"></path>
    <path fill="none" stroke="#696969" ed:parentid="102" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,-38.1L2.5,32.1C2.5,35.4,5.2,38.1,8.5,38.1L16.5,38.1" stroke-linecap="round" transform="matrix(1,0,0,1,485.78,422.62)" id="394" ed:tosuperid="393"></path>
    <path fill="none" stroke="#696969" ed:parentid="389" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,0.7L2.5,0.7C2.5,-0.1,5.2,-0.7,8.5,-0.7L16.5,-0.7" stroke-linecap="round" transform="matrix(1,0,0,1,591.78,65.75)" id="396" ed:tosuperid="395"></path>
    <path fill="none" stroke="#696969" ed:parentid="395" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,-8L2.5,-8L2.5,2C2.5,5.3,5.2,8,8.5,8L16.5,8" stroke-linecap="round" transform="matrix(1,0,0,1,696.78,73)" id="432" ed:tosuperid="411"></path>
    <path fill="none" stroke="#696969" ed:parentid="476" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,6.9L2.5,6.9L2.5,-0.9C2.5,-4.2,5.2,-6.9,8.5,-6.9L16.5,-6.9" stroke-linecap="round" transform="matrix(1,0,0,1,765.78,175.73)" id="473" ed:tosuperid="445"></path>
    <path fill="none" stroke="#696969" ed:parentid="476" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,-5.9L2.5,-0.1C2.5,3.2,5.2,5.9,8.5,5.9L16.5,5.9" stroke-linecap="round" transform="matrix(1,0,0,1,765.78,188.52)" id="475" ed:tosuperid="474"></path>
    <path fill="none" stroke="#696969" ed:parentid="126" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,6.4L2.5,6.4L2.5,-0.4C2.5,-3.7,5.2,-6.4,8.5,-6.4L16.5,-6.4" stroke-linecap="round" transform="matrix(1,0,0,1,690.78,189.02)" id="477" ed:tosuperid="476"></path>
    <path fill="none" stroke="#696969" ed:parentid="445" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,-0.5L2.5,-0.5C2.5,0.1,5.2,0.5,8.5,0.5L16.5,0.5" stroke-linecap="round" transform="matrix(1,0,0,1,854.8,169.33)" id="479" ed:tosuperid="478"></path>
    <path fill="none" stroke="#696969" ed:parentid="474" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,-0.5L2.5,-0.5C2.5,0.1,5.2,0.5,8.5,0.5L16.5,0.5" stroke-linecap="round" transform="matrix(1,0,0,1,854.8,194.92)" id="481" ed:tosuperid="480"></path>
    <path fill="none" stroke="#696969" ed:parentid="126" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,-12.8L2.5,6.8C2.5,10.1,5.2,12.8,8.5,12.8L16.5,12.8" stroke-linecap="round" transform="matrix(1,0,0,1,690.78,208.21)" id="483" ed:tosuperid="482"></path>
    <path fill="none" stroke="#696969" ed:parentid="482" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,0.5L2.5,0.5C2.5,-0.1,5.2,-0.5,8.5,-0.5L16.5,-0.5" stroke-linecap="round" transform="matrix(1,0,0,1,765.78,220.5)" id="485" ed:tosuperid="484"></path>
    <path fill="none" stroke="#696969" ed:parentid="128" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,7.6L2.5,7.6L2.5,-1.6C2.5,-5,5.2,-7.6,8.5,-7.6L16.5,-7.6" stroke-linecap="round" transform="matrix(1,0,0,1,702.78,355.73)" id="487" ed:tosuperid="486"></path>
    <path fill="none" stroke="#696969" ed:parentid="486" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,-50.8L2.5,-50.8L2.5,44.8C2.5,48.1,5.2,50.8,8.5,50.8L16.5,50.8" stroke-linecap="round" transform="matrix(1,0,0,1,777.78,398.83)" id="489" ed:tosuperid="488"></path>
    <path fill="none" stroke="#696969" ed:parentid="128" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,-57.1L2.5,51.1C2.5,54.5,5.2,57.1,8.5,57.1L16.5,57.1" stroke-linecap="round" transform="matrix(1,0,0,1,702.78,420.52)" id="491" ed:tosuperid="490"></path>
    <path fill="none" stroke="#696969" ed:parentid="490" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,-1.3L2.5,-1.3C2.5,0.1,5.2,1.3,8.5,1.3L16.5,1.3" stroke-linecap="round" transform="matrix(1,0,0,1,777.78,478.92)" id="493" ed:tosuperid="492"></path>
    <path fill="none" stroke="#696969" ed:parentid="915" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,-12.8L2.5,6.8C2.5,10.1,5.2,12.8,8.5,12.8L16.5,12.8" stroke-linecap="round" transform="matrix(1,0,0,1,485.78,1512.54)" id="900" ed:tosuperid="106"></path>
    <path fill="none" stroke="#696969" ed:parentid="101" stroke-linejoin="round" stroke-width="1.33333" d="M-2.5,-285.2L38.1,-285.2L38.1,279.2C38.1,282.6,40.8,285.2,44.1,285.2L102.5,285.2" stroke-linecap="round" transform="matrix(1,0,0,1,223.81,1214.54)" id="939" ed:tosuperid="915"></path>
    <path fill="none" stroke="#696969" ed:parentid="411" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,20.5L2.5,20.5L2.5,-14.5C2.5,-17.9,5.2,-20.5,8.5,-20.5L16.5,-20.5" stroke-linecap="round" transform="matrix(1,0,0,1,999.27,60.46)" id="1073" ed:tosuperid="1070"></path>
    <path fill="none" stroke="#696969" ed:parentid="411" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,7.8L2.5,-1.8C2.5,-5.1,5.2,-7.8,8.5,-7.8L16.5,-7.8" stroke-linecap="round" transform="matrix(1,0,0,1,999.27,73.25)" id="1074" ed:tosuperid="1071"></path>
    <path fill="none" stroke="#696969" ed:parentid="411" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,-5L2.5,-1C2.5,2.4,5.2,5,8.5,5L16.5,5" stroke-linecap="round" transform="matrix(1,0,0,1,999.27,86.04)" id="1075" ed:tosuperid="1072"></path>
    <path fill="none" stroke="#696969" ed:parentid="122" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,59.7L2.5,59.7L2.5,-53.7C2.5,-57,5.2,-59.7,8.5,-59.7L16.5,-59.7" stroke-linecap="round" transform="matrix(1,0,0,1,690.78,567.98)" id="1077" ed:tosuperid="1076"></path>
    <path fill="none" stroke="#696969" ed:parentid="122" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,45.7L2.5,-39.7C2.5,-43,5.2,-45.7,8.5,-45.7L16.5,-45.7" stroke-linecap="round" transform="matrix(1,0,0,1,690.78,582.02)" id="1079" ed:tosuperid="1078"></path>
    <path fill="none" stroke="#696969" ed:parentid="122" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,32.9L2.5,-26.9C2.5,-30.2,5.2,-32.9,8.5,-32.9L16.5,-32.9" stroke-linecap="round" transform="matrix(1,0,0,1,690.78,594.81)" id="1081" ed:tosuperid="1080"></path>
    <path fill="none" stroke="#696969" ed:parentid="122" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,4.7L2.5,1.3C2.5,-2,5.2,-4.7,8.5,-4.7L16.5,-4.7" stroke-linecap="round" transform="matrix(1,0,0,1,690.78,623)" id="1083" ed:tosuperid="1082"></path>
    <path fill="none" stroke="#696969" ed:parentid="1076" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,-1.3L2.5,-1.3C2.5,0.1,5.2,1.3,8.5,1.3L16.5,1.3" stroke-linecap="round" transform="matrix(1,0,0,1,864.3,509.5)" id="1089" ed:tosuperid="1088"></path>
    <path fill="none" stroke="#696969" ed:parentid="391" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,0.2L2.5,0.2C2.5,-0,5.2,-0.2,8.5,-0.2L16.5,-0.2" stroke-linecap="round" transform="matrix(1,0,0,1,591.78,131.21)" id="1091" ed:tosuperid="1090"></path>
    <path fill="none" stroke="#696969" ed:parentid="1090" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,7.4L2.5,7.4L2.5,-1.4C2.5,-4.7,5.2,-7.4,8.5,-7.4L16.5,-7.4" stroke-linecap="round" transform="matrix(1,0,0,1,666.78,123.56)" id="1095" ed:tosuperid="1094"></path>
    <path fill="none" stroke="#696969" ed:parentid="1090" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,-5.9L2.5,-0.1C2.5,3.2,5.2,5.9,8.5,5.9L16.5,5.9" stroke-linecap="round" transform="matrix(1,0,0,1,666.78,136.85)" id="1097" ed:tosuperid="1096"></path>
    <path fill="none" stroke="#696969" ed:parentid="1094" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,-0.8L2.5,-0.8C2.5,0.1,5.2,0.8,8.5,0.8L16.5,0.8" stroke-linecap="round" transform="matrix(1,0,0,1,739.86,116.92)" id="1099" ed:tosuperid="1098"></path>
    <path fill="none" stroke="#696969" ed:parentid="1096" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,-0.8L2.5,-0.8C2.5,0.1,5.2,0.8,8.5,0.8L16.5,0.8" stroke-linecap="round" transform="matrix(1,0,0,1,754.8,143.5)" id="1101" ed:tosuperid="1100"></path>
    <path fill="none" stroke="#696969" ed:parentid="122" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,-42.5L2.5,36.5C2.5,39.8,5.2,42.5,8.5,42.5L16.5,42.5" stroke-linecap="round" transform="matrix(1,0,0,1,690.78,670.19)" id="1103" ed:tosuperid="1102"></path>
    <path fill="none" stroke="#696969" ed:parentid="1102" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,-18.5L2.5,-18.5L2.5,12.5C2.5,15.8,5.2,18.5,8.5,18.5L16.5,18.5" stroke-linecap="round" transform="matrix(1,0,0,1,873.11,731.17)" id="1105" ed:tosuperid="1104"></path>
    <path fill="none" stroke="#696969" ed:parentid="1082" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,1.9L2.5,1.9C2.5,-0.2,5.2,-1.9,8.5,-1.9L16.5,-1.9" stroke-linecap="round" transform="matrix(1,0,0,1,860.67,616.4)" id="1107" ed:tosuperid="1106"></path>
    <path fill="none" stroke="#696969" ed:parentid="1082" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,-15.4L2.5,9.4C2.5,12.7,5.2,15.4,8.5,15.4L16.5,15.4" stroke-linecap="round" transform="matrix(1,0,0,1,860.67,633.69)" id="1109" ed:tosuperid="1108"></path>
    <path fill="none" stroke="#696969" ed:parentid="164" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,-5.9L2.5,-0.1C2.5,3.2,5.2,5.9,8.5,5.9L16.5,5.9" stroke-linecap="round" transform="matrix(1,0,0,1,753.69,995.77)" id="1111" ed:tosuperid="1110"></path>
    <path fill="none" stroke="#696969" ed:parentid="1110" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,-40.8L2.5,-40.8L2.5,34.8C2.5,38.1,5.2,40.8,8.5,40.8L16.5,40.8" stroke-linecap="round" transform="matrix(1,0,0,1,852.69,1042.42)" id="1113" ed:tosuperid="1112"></path>
    <path fill="none" stroke="#696969" ed:parentid="1148" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,19.7L2.5,19.7L2.5,-13.7C2.5,-17,5.2,-19.7,8.5,-19.7L16.5,-19.7" stroke-linecap="round" transform="matrix(1,0,0,1,741.69,811.94)" id="1115" ed:tosuperid="1114"></path>
    <path fill="none" stroke="#696969" ed:parentid="1148" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,-18.7L2.5,12.7C2.5,16,5.2,18.7,8.5,18.7L16.5,18.7" stroke-linecap="round" transform="matrix(1,0,0,1,741.69,850.31)" id="1117" ed:tosuperid="1116"></path>
    <path fill="none" stroke="#696969" ed:parentid="1148" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,6.9L2.5,-0.9C2.5,-4.2,5.2,-6.9,8.5,-6.9L16.5,-6.9" stroke-linecap="round" transform="matrix(1,0,0,1,741.69,824.73)" id="1119" ed:tosuperid="1118"></path>
    <path fill="none" stroke="#696969" ed:parentid="1148" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,-5.9L2.5,-0.1C2.5,3.2,5.2,5.9,8.5,5.9L16.5,5.9" stroke-linecap="round" transform="matrix(1,0,0,1,741.69,837.52)" id="1121" ed:tosuperid="1120"></path>
    <path fill="none" stroke="#696969" ed:parentid="164" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,47.6L2.5,47.6L2.5,-41.6C2.5,-45,5.2,-47.6,8.5,-47.6L16.5,-47.6" stroke-linecap="round" transform="matrix(1,0,0,1,753.69,942.23)" id="1137" ed:tosuperid="1136"></path>
    <path fill="none" stroke="#696969" ed:parentid="1136" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,0.5L2.5,0.5C2.5,-0.1,5.2,-0.5,8.5,-0.5L16.5,-0.5" stroke-linecap="round" transform="matrix(1,0,0,1,852.69,894.08)" id="1139" ed:tosuperid="1138"></path>
    <path fill="none" stroke="#696969" ed:parentid="156" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,53.5L2.5,53.5L2.5,-47.5C2.5,-50.9,5.2,-53.5,8.5,-53.5L16.5,-53.5" stroke-linecap="round" transform="matrix(1,0,0,1,635.69,885.17)" id="1154" ed:tosuperid="1148"></path>
    <path fill="none" stroke="#696969" ed:parentid="104" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,-77.9L2.5,71.9C2.5,75.2,5.2,77.9,8.5,77.9L16.5,77.9" stroke-linecap="round" transform="matrix(1,0,0,1,485.78,1032.87)" id="1156" ed:tosuperid="1155"></path>
    <path fill="none" stroke="#696969" ed:parentid="104" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,-91.7L2.5,85.7C2.5,89,5.2,91.7,8.5,91.7L16.5,91.7" stroke-linecap="round" transform="matrix(1,0,0,1,485.78,1046.67)" id="1158" ed:tosuperid="1157"></path>
    <path fill="none" stroke="#696969" ed:parentid="116" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,43.7L2.5,43.7L2.5,-37.7C2.5,-41,5.2,-43.7,8.5,-43.7L16.5,-43.7" stroke-linecap="round" transform="matrix(1,0,0,1,653.77,1222.65)" id="1162" ed:tosuperid="1161"></path>
    <path fill="none" stroke="#696969" ed:parentid="116" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,31.9L2.5,-25.9C2.5,-29.2,5.2,-31.9,8.5,-31.9L16.5,-31.9" stroke-linecap="round" transform="matrix(1,0,0,1,653.77,1234.44)" id="1164" ed:tosuperid="1163"></path>
    <path fill="none" stroke="#696969" ed:parentid="116" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,20.1L2.5,-14.1C2.5,-17.5,5.2,-20.1,8.5,-20.1L16.5,-20.1" stroke-linecap="round" transform="matrix(1,0,0,1,653.77,1246.23)" id="1166" ed:tosuperid="1165"></path>
    <path fill="none" stroke="#696969" ed:parentid="116" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,8.4L2.5,-2.4C2.5,-5.7,5.2,-8.4,8.5,-8.4L16.5,-8.4" stroke-linecap="round" transform="matrix(1,0,0,1,653.77,1258.02)" id="1168" ed:tosuperid="1167"></path>
    <path fill="none" stroke="#696969" ed:parentid="116" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,-3.4C2.5,0.7,5.2,3.4,8.5,3.4L16.5,3.4" stroke-linecap="round" transform="matrix(1,0,0,1,653.77,1269.81)" id="1170" ed:tosuperid="1169"></path>
    <path fill="none" stroke="#696969" ed:parentid="116" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,-15.2L2.5,9.2C2.5,12.5,5.2,15.2,8.5,15.2L16.5,15.2" stroke-linecap="round" transform="matrix(1,0,0,1,653.77,1281.6)" id="1172" ed:tosuperid="1171"></path>
    <path fill="none" stroke="#696969" ed:parentid="114" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,34.8L2.5,34.8L2.5,-28.8C2.5,-32.1,5.2,-34.8,8.5,-34.8L16.5,-34.8" stroke-linecap="round" transform="matrix(1,0,0,1,653.5,1355.25)" id="1174" ed:tosuperid="1173"></path>
    <path fill="none" stroke="#696969" ed:parentid="114" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,23L2.5,-17C2.5,-20.4,5.2,-23,8.5,-23L16.5,-23" stroke-linecap="round" transform="matrix(1,0,0,1,653.5,1367.04)" id="1176" ed:tosuperid="1175"></path>
    <path fill="none" stroke="#696969" ed:parentid="114" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,11.3L2.5,-5.3C2.5,-8.6,5.2,-11.3,8.5,-11.3L16.5,-11.3" stroke-linecap="round" transform="matrix(1,0,0,1,653.5,1378.83)" id="1178" ed:tosuperid="1177"></path>
    <path fill="none" stroke="#696969" ed:parentid="114" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,-0.5C2.5,0.1,5.2,0.5,8.5,0.5L16.5,0.5" stroke-linecap="round" transform="matrix(1,0,0,1,653.5,1390.62)" id="1180" ed:tosuperid="1179"></path>
    <path fill="none" stroke="#696969" ed:parentid="114" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,-12.3L2.5,6.3C2.5,9.6,5.2,12.3,8.5,12.3L16.5,12.3" stroke-linecap="round" transform="matrix(1,0,0,1,653.5,1402.42)" id="1182" ed:tosuperid="1181"></path>
    <path fill="none" stroke="#696969" ed:parentid="106" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,43.5L2.5,43.5L2.5,-37.5C2.5,-40.8,5.2,-43.5,8.5,-43.5L16.5,-43.5" stroke-linecap="round" transform="matrix(1,0,0,1,637.09,1481.81)" id="1184" ed:tosuperid="1183"></path>
    <path fill="none" stroke="#696969" ed:parentid="106" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,31.7L2.5,-25.7C2.5,-29,5.2,-31.7,8.5,-31.7L16.5,-31.7" stroke-linecap="round" transform="matrix(1,0,0,1,637.09,1493.6)" id="1186" ed:tosuperid="1185"></path>
    <path fill="none" stroke="#696969" ed:parentid="106" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,19.9L2.5,-13.9C2.5,-17.2,5.2,-19.9,8.5,-19.9L16.5,-19.9" stroke-linecap="round" transform="matrix(1,0,0,1,637.09,1505.4)" id="1188" ed:tosuperid="1187"></path>
    <path fill="none" stroke="#696969" ed:parentid="106" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,8.1L2.5,-2.1C2.5,-5.4,5.2,-8.1,8.5,-8.1L16.5,-8.1" stroke-linecap="round" transform="matrix(1,0,0,1,637.09,1517.19)" id="1190" ed:tosuperid="1189"></path>
    <path fill="none" stroke="#696969" ed:parentid="106" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,-3.7L2.5,-2.3C2.5,1,5.2,3.7,8.5,3.7L16.5,3.7" stroke-linecap="round" transform="matrix(1,0,0,1,637.09,1528.98)" id="1192" ed:tosuperid="1191"></path>
    <path fill="none" stroke="#696969" ed:parentid="106" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,-15.5L2.5,9.5C2.5,12.8,5.2,15.5,8.5,15.5L16.5,15.5" stroke-linecap="round" transform="matrix(1,0,0,1,637.09,1540.77)" id="1194" ed:tosuperid="1193"></path>
    <path fill="none" stroke="#696969" ed:parentid="108" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,49.4L2.5,49.4L2.5,-43.4C2.5,-46.7,5.2,-49.4,8.5,-49.4L16.5,-49.4" stroke-linecap="round" transform="matrix(1,0,0,1,624.78,1629.21)" id="1196" ed:tosuperid="1195"></path>
    <path fill="none" stroke="#696969" ed:parentid="108" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,37.6L2.5,-31.6C2.5,-34.9,5.2,-37.6,8.5,-37.6L16.5,-37.6" stroke-linecap="round" transform="matrix(1,0,0,1,624.78,1641)" id="1198" ed:tosuperid="1197"></path>
    <path fill="none" stroke="#696969" ed:parentid="108" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,25.8L2.5,-19.8C2.5,-23.1,5.2,-25.8,8.5,-25.8L16.5,-25.8" stroke-linecap="round" transform="matrix(1,0,0,1,624.78,1652.79)" id="1200" ed:tosuperid="1199"></path>
    <path fill="none" stroke="#696969" ed:parentid="108" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,14L2.5,-8C2.5,-11.3,5.2,-14,8.5,-14L16.5,-14" stroke-linecap="round" transform="matrix(1,0,0,1,624.78,1664.58)" id="1202" ed:tosuperid="1201"></path>
    <path fill="none" stroke="#696969" ed:parentid="108" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,2.2C2.5,-0.2,5.2,-2.2,8.5,-2.2L16.5,-2.2" stroke-linecap="round" transform="matrix(1,0,0,1,624.78,1676.37)" id="1204" ed:tosuperid="1203"></path>
    <path fill="none" stroke="#696969" ed:parentid="108" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,-9.6L2.5,3.6C2.5,6.9,5.2,9.6,8.5,9.6L16.5,9.6" stroke-linecap="round" transform="matrix(1,0,0,1,624.78,1688.17)" id="1206" ed:tosuperid="1205"></path>
    <path fill="none" stroke="#696969" ed:parentid="108" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,-21.4L2.5,15.4C2.5,18.7,5.2,21.4,8.5,21.4L16.5,21.4" stroke-linecap="round" transform="matrix(1,0,0,1,624.78,1699.96)" id="1208" ed:tosuperid="1207"></path>
    <path fill="none" stroke="#696969" ed:parentid="915" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,-160.4L2.5,154.4C2.5,157.7,5.2,160.4,8.5,160.4L16.5,160.4" stroke-linecap="round" transform="matrix(1,0,0,1,485.78,1660.19)" id="1229" ed:tosuperid="1216"></path>
    <path fill="none" stroke="#696969" ed:parentid="1216" stroke-linejoin="round" stroke-width="1.33333" d="M-16.5,37.8L2.5,37.8L2.5,-31.8C2.5,-35.1,5.2,-37.8,8.5,-37.8L16.5,-37.8" stroke-linecap="round" transform="matrix(1,0,0,1,632.78,1782.75)" id="1231" ed:tosuperid="1230"></path>
    <path fill="none" stroke="#696969" ed:parentid="1216" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,26L2.5,-20C2.5,-23.4,5.2,-26,8.5,-26L16.5,-26" stroke-linecap="round" transform="matrix(1,0,0,1,632.78,1794.54)" id="1233" ed:tosuperid="1232"></path>
    <path fill="none" stroke="#696969" ed:parentid="1216" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,14.3L2.5,-8.3C2.5,-11.6,5.2,-14.3,8.5,-14.3L16.5,-14.3" stroke-linecap="round" transform="matrix(1,0,0,1,632.78,1806.33)" id="1235" ed:tosuperid="1234"></path>
    <path fill="none" stroke="#696969" ed:parentid="1216" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,2.5C2.5,-0.3,5.2,-2.5,8.5,-2.5L16.5,-2.5" stroke-linecap="round" transform="matrix(1,0,0,1,632.78,1818.12)" id="1237" ed:tosuperid="1236"></path>
    <path fill="none" stroke="#696969" ed:parentid="1216" stroke-linejoin="round" stroke-width="1.33333" d="M2.5,-9.3L2.5,3.3C2.5,6.6,5.2,9.3,8.5,9.3L16.5,9.3" stroke-linecap="round" transform="matrix(1,0,0,1,632.78,1829.92)" id="1239" ed:tosuperid="1238"></path>
    <g ed:width="199.974" ed:topictype="mainidea" ed:layout="map" transform="matrix(1,0,0,1,21.33,858.07)" id="101" ed:height="142.443">
        <path fill="#365b88" stroke="#365b88" stroke-linejoin="round" stroke-width="1.33333" d="M4,0L196,0C198.7,0,200,1.3,200,4L200,138.4C200,141.1,198.7,142.4,196,142.4L4,142.4C1.3,142.4,0,141.1,0,138.4L0,4C0,1.3,1.3,0,4,0z"></path>
        <g transform="translate(55.66,15.27)">
            <path fill="#ffffff" d="M87.3,5L48.3,5L48.3,2.3L48.3,0L46.3,0L43.6,0L41.3,0L41.3,2.3L41.3,5L2.3,5L0,5L0,7.3L0,10L0,12.3L2.3,12.3L7.3,12.3L7.3,56C7.3,60.3,11,63.9,15.3,63.9L41.3,63.9L41.3,72.5L18.5,72.5L16.2,72.5L16.2,74.8L16.2,77.8L16.2,80.1L18.5,80.1L71.9,80.1L74.2,80.1L74.2,77.8L74.2,74.8L74.2,72.5L71.9,72.5L48.6,72.5L48.6,63.9L74.4,63.9C78.7,63.9,82.3,60.3,82.3,56L82.3,12.3L87.3,12.3L89.6,12.3L89.6,10L89.6,7.3L89.6,5L87.3,5z" transform="matrix(1,0,0,1,0,0)" id="407"></path>
            <path fill="#4c4c4c" d="M85,5L43.7,5L43.7,0L41,0L41,5L0,5L0,7.7L7.3,7.7L7.3,53.5C7.3,56.7,9.8,59.2,13,59.2L41,59.2L41,72.5L16.1,72.5L16.1,75.2L69.5,75.2L69.5,72.5L43.7,72.5L43.7,59.2L71.8,59.2C74.9,59.2,77.4,56.6,77.4,53.5L77.4,7.7L85,7.7L85,5zM75.4,53.5C75.4,55.3,73.8,57,72,57L12.9,57C11,57,9.4,55.4,9.4,53.5L9.4,7.8L75.4,7.8L75.4,53.5z" transform="matrix(1,0,0,1,2.59,2.3)" id="408"></path>
            <path fill="#5ac7d5" d="M19.8,3.4C17.8,1.4,15,0.3,12.3,0L11.6,12.9L0,16.1C0.7,17.6,0.9,18.4,1.5,19.1C2.2,19.9,3.2,20.9,4,21.6C6.1,23.2,8.6,23.9,11.1,23.9C12.8,23.9,14.3,23.7,15.7,23C17,22.3,18.4,21.4,19.5,20.5C20.7,19.4,21.5,18,22,16.7C22.7,15,22.9,13.5,22.9,11.9C23.4,8.6,22.1,5.7,19.8,3.4z" transform="matrix(1,0,0,1,38.63,26.83)" id="409"></path>
            <path fill="#fcd486" d="M12.7,0C10,0,8.2,0.5,6.6,1.1C4.9,1.8,3.6,2.8,2.7,4.1C0.9,6.4,-0.2,9.1,0,12.1C0,13.4,0.2,15,0.7,16.4L12.7,14.1L12.7,0z" transform="matrix(1,0,0,1,34.07,22.52)" id="410"></path>
        </g>
        <text class="st1">
            <tspan style="white-space:pre" y="123.3" class="st2" x="15">MySQL SYS系统库</tspan>
        </text>
    </g>
    <g ed:width="142.9739550352097" ed:parentid="101" ed:layout="rightmap" transform="matrix(1,0,0,1,326.31,339.33)" id="102" ed:height="90.33333003520966">
        <path fill="#f6f6f6" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M4,0L139,0C141.7,0,143,1.3,143,4L143,86.3C143,89,141.7,90.3,139,90.3L4,90.3C1.3,90.3,0,89,0,86.3L0,4C0,1.3,1.3,0,4,0z"></path>
        <g transform="translate(49.16,8.27)">
            <path fill="#ffffff" d="M44.1,12L43.8,10.5L42.3,10.8L40.9,11.1L39.5,11.4L39.8,12.9C42.1,26.7,42.1,42.1,38.6,43.1C37.9,43.2,37.3,43.4,36.8,43.4C36.2,43.4,35.6,43.3,35.3,43C34.3,42.2,34.2,40.2,34.2,39.3C34.2,39,34.6,22,34.7,16.7C34.9,10.3,33.6,6,31,3.3C28.9,0.8,26.4,0,23.6,0C23.4,0,23.3,0,23.1,0C19.8,0.1,17.1,1.1,15.3,3.1C14.3,4.2,13.7,5.4,13.4,6.5L8.5,6.5C3.9,6.6,0,10.6,0,15.2L0,33.2C0,41.4,6.6,48,14.7,48L15.7,48C22.6,48,28.4,43.1,29.9,36.6C29.9,37,29.7,38.8,29.7,38.9C29.7,39.4,29.4,43.9,32.3,46.3C33.5,47.1,35.1,47.6,36.6,47.6C37.6,47.6,38.8,47.5,39.9,47.2C48.6,44.6,45.2,19.7,44.1,12z" transform="matrix(1,0,0,1,0,0)" id="399"></path>
            <path fill="#4c4c4c" d="M25.6,44.8C24.6,45.1,23.2,45.2,22.4,45.1C16.1,44.4,16.9,37.5,16.9,37.3C16.9,37.3,17.3,20.2,17.4,14.9C17.6,9.3,16.6,5.5,14.5,3.5C13.3,2.2,11.2,1.6,9,1.5C6.9,1.4,4.3,2.3,3.1,3.8C1.3,6,1.8,6.7,1.7,7.1L0,7.1C0.1,6.4,0.5,4.3,2.1,2.7C3.7,1.1,6.2,0,9.5,0C12,0,14.1,1,15.5,2.6C17.8,4.9,19,9.1,18.8,15C18.7,20.4,18.5,37.5,18.5,37.5C18.5,37.5,17.3,42.9,22.6,43.8C23.3,44,24.3,43.8,25.1,43.5C30.9,41.8,28.2,19,26.8,10.7L28.2,10.4C28.8,13.6,33.5,42.4,25.6,44.8z" transform="matrix(1,0,0,1,14.02,1.13)" id="400"></path>
            <path fill="#4c4c4c" d="M13.3,38.7C6,38.7,0,32.6,0,25.3L0,7.3C0,3.4,3.2,0,7.1,0L20.4,0C24.3,0.1,27.5,3.4,27.5,7.3L27.5,25.3C27.5,32.6,21.5,38.7,14.3,38.7L13.3,38.7z" transform="matrix(1,0,0,1,1.18,8.34)" id="401"></path>
            <path fill="#69d9e2" d="M5.9,0L18.8,0C22,0,24.8,2.7,24.8,6.1L24.8,24C24.8,30.7,19.5,36,12.9,36L11.9,36C5.3,36,0,30.7,0,24L0,6.1C0.1,2.7,2.8,0,5.9,0z" transform="matrix(1,0,0,1,2.65,9.65)" id="402"></path>
            <path fill="#fffdfd" d="M0,0L13.1,0L13.1,10.3C13.1,13.8,10.4,16.7,6.8,16.7C2.8,16.7,0,13.9,0,10.3L0,0z" transform="matrix(1,0,0,1,8.48,9.65)" id="403"></path>
            <path fill="#4c4c4c" d="M1,7.9C0.4,7.9,0,7.4,0,7L0,0.8C0,0.4,0.4,0,0.7,0C1.3,0,1.6,0.4,1.6,0.8L1.6,7C1.7,7.4,1.4,7.9,1,7.9z" transform="matrix(1,0,0,1,14.08,7.74)" id="404"></path>
        </g>
        <text class="st3">
            <tspan style="white-space:pre" y="79.2" class="st2" x="17">SYS系统库简介</tspan>
        </text>
    </g>
    <g ed:width="142.9739550352097" ed:parentid="101" ed:layout="rightmap" transform="matrix(1,0,0,1,326.31,909.83)" id="104" ed:height="90.33333003520966">
        <path fill="#f6f6f6" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M4,0L139,0C141.7,0,143,1.3,143,4L143,86.3C143,89,141.7,90.3,139,90.3L4,90.3C1.3,90.3,0,89,0,86.3L0,4C0,1.3,1.3,0,4,0z"></path>
        <g transform="translate(50.33,8.27)">
            <path fill="#ffffff" d="M38,0.5L38,3L32.5,3L32.5,30.2C35.1,31,37,33.4,37,36.3C37,39.7,34,42.6,30.5,42.6C27.5,42.6,24.9,40.5,24.2,37.7L0,37.7L0,35.3L3.4,35.3C2.4,35.1,1.8,34.4,1.8,33.3L1.8,27.4C1.8,26.4,2.5,25.6,3.5,25.3C2.5,25,1.8,24.3,1.8,23.2L1.8,17.4C1.8,16.4,2.5,15.6,3.5,15.3C2.5,15.2,1.8,14.3,1.8,13.2L1.8,7.4C1.8,6.3,2.8,5.3,3.9,5.3L27.9,5.3C28.9,5.3,29.7,6,29.8,6.8L29.8,2.5L29.8,0L38,0" transform="matrix(1,0,0,1,2.68,2.69)" id="433"></path>
            <path fill="#ffffff" d="M40.7,0L32.8,0L30,0L30,2.7L30,5.2L6.8,5.2C4.2,5.2,1.9,7.5,1.9,10.1L1.9,15.9C1.9,16.6,2.1,17.3,2.4,18C2.1,18.7,1.9,19.2,1.9,20.1L1.9,25.9C1.9,26.6,2.1,27.3,2.4,28C2.1,28.5,1.9,29.2,1.9,30.1L1.9,35.2L0,35.2L0,38L0,40.3L0,43L2.8,43L25,43C26.5,46.1,29.6,47.9,33.1,48C39.5,48.2,42.3,43.6,42.4,38.8C42.4,35.5,40.5,32.6,37.9,31L37.9,8.3L40.5,8.3L43.3,8.3L43.3,5.6L43.3,3.1L43.3,0L40.7,0z" transform="matrix(1,0,0,1,0,0)" id="434"></path>
            <path fill="#4c4c4c" d="M40.5,0L29.8,0L29.8,5.2C29.6,5.2,5.3,5.2,5.3,5.2C3.4,5.2,1.8,6.8,1.8,8.7L1.8,14.5C1.8,14.9,2.2,16.4,2.4,16.8C2.1,17.4,1.8,18.1,1.8,18.7L1.8,24.5C1.8,24.9,2.2,26.4,2.4,26.8C2.1,27.4,1.8,28.1,1.8,28.7L1.8,34.5C1.8,34.8,1.8,35.1,1.8,35.2L0,35.2L0,40.9L1.4,40.9L24.6,40.9C25.8,43.8,28.7,45.6,31.9,45.6C36.2,45.6,39.7,42.5,39.7,38.1C39.7,37.5,40.2,33.1,35.2,30.2L35.2,6.2L39.3,6.2L40.5,6.2L40.5,0z" transform="matrix(1,0,0,1,1.38,1.2)" id="435"></path>
            <path fill="#dddddd" d="M37.9,2.5L37.9,0L29.9,0L29.9,2.5L29.9,34.8L0,34.8L0,37.2L32.5,37.2L32.5,34.8L32.3,2.5L37.9,2.5z" transform="matrix(1,0,0,1,2.95,3.2)" id="436"></path>
            <path fill="#ffe48f" d="M26,10L2.1,10C1,10,0,9,0,7.9L0,2.1C0,1,1,0,2.1,0L26,0C27.2,0,28.2,1,28.2,2.1L28.2,7.9C28.2,9.2,27.3,10,26,10z" transform="matrix(1,0,0,1,4.71,28.01)" id="437"></path>
            <path fill="#304352" d="M26,10L2.1,10C1,10,0,9,0,7.9L0,2.1C0,1,1,0,2.1,0L26,0C27.2,0,28.2,1,28.2,2.1L28.2,7.9C28.2,9.1,27.3,10,26,10z" transform="matrix(1,0,0,1,4.71,18.17)" id="438"></path>
            <path fill="#69d9e2" d="M26,10L2.1,10C1,10,0,9,0,7.9L0,2.1C0,1,1,0,2.1,0L26,0C27.2,0,28.2,1,28.2,2.1L28.2,7.9C28.2,9,27.3,10,26,10z" transform="matrix(1,0,0,1,4.71,8.38)" id="439"></path>
            <path fill="#132c2d" d="M12.8,6.3C12.8,9.9,10,12.7,6.4,12.7C2.9,12.7,0,9.9,0,6.3C0,2.8,2.9,0,6.4,0C10,0,12.8,2.8,12.8,6.3z" transform="matrix(1,0,0,1,26.82,32.49)" id="440"></path>
            <path fill="#dddddd" d="M6.7,3.3C6.7,5.2,5.2,6.6,3.4,6.6C1.5,6.6,0,5.2,0,3.3C0,1.5,1.5,0,3.4,0C5.2,0,6.7,1.5,6.7,3.3z" transform="matrix(1,0,0,1,29.88,35.52)" id="441"></path>
        </g>
        <text class="st3">
            <tspan style="white-space:pre" y="79.2" class="st2" x="17">SYS系统库配置</tspan>
        </text>
    </g>
    <g ed:width="118.3125" ed:parentid="915" ed:layout="rightmap" transform="matrix(1,0,0,1,502.28,1450.71)" id="106" ed:height="74.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,74.6L118.3,74.6"></path>
        <g transform="translate(38.47,1.83)">
            <path fill="#ffffff" d="M40.6,39.6L33.3,30.2C35.9,26.9,37.4,23,37.4,18.6C37.2,8.3,28.8,0,18.6,0C8.3,0,0,8.3,0,18.5C0,28.7,8.3,37,18.6,37C20.8,37,23.2,36.6,25.3,35.8L31.8,45.5C32.4,46.3,34.2,48,36.6,48C37.7,48,38.7,47.6,39.7,47C42.3,44.9,41.5,41.3,40.6,39.6z" transform="matrix(1,0,0,1,0,0)" id="443"></path>
            <path fill="#4c4c4c" d="M38.1,38.8L30.2,28.7C33.1,25.6,34.7,21.5,34.7,17.2C34.6,7.6,26.8,0,17.3,0C7.8,0,0,7.7,0,17.2C0,26.6,7.8,34.3,17.3,34.3C19.8,34.3,22.2,33.8,24.4,32.8L31.4,43.4C31.7,43.7,33.3,45.3,35.3,45.3C36.1,45.3,36.8,45,37.4,44.5C39.4,43,38.7,40.2,38.1,38.8z" transform="matrix(1,0,0,1,1.37,1.35)" id="444"></path>
            <path fill="#69d9e2" d="M0,2.8L10.3,18.1C10.3,18.1,12.5,20.3,14.2,18.9C15.9,17.5,14.5,14.7,14.5,14.7L2.8,0L0,2.8z" transform="matrix(1,0,0,1,23.89,25.93)" id="446"></path>
            <path fill="#69d9e2" d="M0,15.8C0,7.1,7.1,0,15.9,0C24.8,0,31.9,7.1,31.9,15.8C31.9,24.6,24.8,31.7,15.9,31.7C7.1,31.7,0,24.6,0,15.8z" transform="matrix(1,0,0,1,2.94,2.76)" id="447"></path>
            <path fill="#ffffff" d="M0,12.4C0,5.6,5.7,0,12.5,0C19.5,0,25.2,5.6,25.2,12.4C25.2,19.4,19.5,25,12.5,25C5.7,24.9,0,19.3,0,12.4z" transform="matrix(1,0,0,1,6.22,6.07)" id="448"></path>
        </g>
        <text class="st4">
            <tspan style="white-space:pre" y="68.8" class="st5" x="8">按 file 分组统计视图</tspan>
        </text>
    </g>
    <g ed:width="106" ed:parentid="915" ed:layout="rightmap" transform="matrix(1,0,0,1,502.28,1604)" id="108" ed:height="74.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,74.6L106,74.6"></path>
        <g transform="translate(29.04,1.83)">
            <path fill="#ffffff" d="M24,0C10.8,0,0,10.8,0,24C0,37.2,10.8,48,24,48C37.2,48,48,37.2,48,24C48,10.8,37.2,0,24,0z" transform="matrix(1,0,0,1,0,0)" id="413"></path>
            <path fill="#4c4c4c" d="M23,1C35.2,1,44.9,10.9,44.9,23C44.9,35.2,35,44.9,23,44.9C10.9,44.9,1,35,1,23C1,10.9,10.8,1,23,1zM23,0C10.3,0,0,10.3,0,23C0,35.7,10.3,46,23,46C35.7,46,46,35.7,46,23C46,10.3,35.7,0,23,0z" transform="matrix(1,0,0,1,1.02,1.02)" id="414"></path>
            <path fill="#ffe48f" d="M18.4,0C8.2,0,0,8.3,0,18.4C0,28.6,8.3,36.9,18.4,36.9C28.6,36.9,36.9,28.6,36.9,18.4C36.9,8.2,28.6,0,18.4,0z" transform="matrix(1,0,0,1,5.56,5.56)" id="415"></path>
            <path fill="#304352" d="M13.6,0L10.1,5.4C9.1,7.1,8.7,7.6,8.2,8.5L6.4,5.4L2.9,0L0,0L5.9,9.1L2,9.1L2,11.1L6.9,11.1L2,11.9L2,13.9L6.9,13.9L6.9,18.2L9.3,18.2L9.3,13.9L14.1,13.9L14.1,11.9L9.3,11.9L14.1,11.1L14.1,9.1L10.2,9.1L16.1,0L13.6,0z" transform="matrix(1,0,0,1,15.12,16.35)" id="416"></path>
        </g>
        <text class="st4">
            <tspan style="white-space:pre" y="68.8" class="st5" x="8">等待事件统计视图</tspan>
        </text>
    </g>
    <g ed:width="134.71875" ed:parentid="915" ed:layout="rightmap" transform="matrix(1,0,0,1,502.28,1326.5)" id="114" ed:height="63.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,63.6L134.7,63.6"></path>
        <g transform="translate(43.4,1.83)">
            <path fill="#ffffff" d="M43.6,19C42.8,18.4,42,17.9,41.1,17.5C43,16.2,44.3,14,44.4,11.5C44.5,7.3,41.3,3.7,37,3.6C36.9,3.6,36.7,3.6,36.7,3.6C35.3,3.6,34,3.9,32.8,4.6C31.2,1.9,28.3,0.2,25.2,0.1C24.8,0,24.7,0,24.5,0C20.8,0,17.5,2.2,16.1,5.5C14.9,4.6,13.5,4.1,11.9,4.1C11.8,4.1,11.7,4.1,11.7,4.1C7.5,4.1,4.1,7.3,4,11.5C4,14.1,5.2,16.4,7.2,17.8C6.3,18.3,5.5,18.7,4.7,19.3C1.7,21.2,0,24,0,27.1C0,27.3,0,27.3,0,27.4L0,32.5L0,33.7L1.2,33.7L10.5,33.7L10.5,34.8L10.5,36L11.7,36L37.3,36L38.5,36L38.5,34.8L38.5,33.2L46.8,33.2L48,33.2L48,32L48,26.9C48,23.7,46.3,20.8,43.6,19zM29.6,16.8C30,16.6,30.5,16.3,30.8,16C31.2,16.5,31.7,16.9,32.2,17.3C31.9,17.4,31.6,17.5,31.3,17.7C30.8,17.4,30.2,17.1,29.6,16.8zM16.2,17.8C17.1,17.2,17.8,16.5,18.3,15.6C18.7,16,19.2,16.4,19.7,16.7C18.7,17.1,17.7,17.6,16.8,18.2C16.6,18.1,16.4,17.9,16.2,17.8z" transform="matrix(1,0,0,1,0,0)" id="418"></path>
            <path fill="#f7d77c" d="M6.8,11.6C6.7,11.6,6.6,11.6,6.5,11.6C6.2,11.6,6.1,11.6,5.9,11.6C2.1,11.2,0,8.6,0,5.6C0.1,2.5,2.6,0,5.9,0C7.7,0,9.1,0.7,10.2,1.9C11.3,3.1,11.9,4.5,11.8,6.1C11.6,8.7,9.6,11.2,6.8,11.6z" transform="matrix(1,0,0,1,30.57,5.68)" id="419"></path>
            <path fill="#4c4c4c" d="M6.5,1.2C6.6,1.2,6.6,1.2,6.7,1.2C8.1,1.2,9.5,1.8,10.5,2.9C11.4,3.9,11.9,5.2,11.9,6.6C11.8,9.1,10,11.1,7.6,11.6C7.5,11.6,7.3,11.6,7.2,11.6C7,11.6,6.9,11.6,6.6,11.6C3.2,11.2,1.3,8.8,1.3,6.4C1.3,3.4,3.6,1.2,6.5,1.2zM6.5,0C3,0,0.1,2.8,0,6.2C0,9.4,2.3,12.2,5.5,12.8C5.9,12.8,6.1,12.8,6.5,12.8C6.8,12.8,7.2,12.8,7.4,12.8C10.6,12.3,12.8,9.8,12.9,6.7C13,3.1,10.3,0.1,6.7,0C6.6,0,6.6,0,6.5,0z" transform="matrix(1,0,0,1,29.96,5.13)" id="420"></path>
            <path fill="#f7d77c" d="M6.8,11.6C6.7,11.6,6.6,11.6,6.5,11.6C6.2,11.6,6.1,11.6,5.9,11.6C2.1,11.2,0,8.6,0,5.6C0.1,2.5,2.6,0,5.9,0C9.3,0.1,11.9,2.9,11.8,6.1C11.6,8.8,9.6,11.1,6.8,11.6z" transform="matrix(1,0,0,1,5.86,6.18)" id="421"></path>
            <path fill="#4c4c4c" d="M6.5,1.2C6.6,1.2,6.6,1.2,6.7,1.2C8.1,1.2,9.5,1.8,10.5,2.8C11.4,3.9,11.9,5.2,11.9,6.5C11.8,9.1,10,11.1,7.6,11.6C7.5,11.6,7.3,11.6,7.2,11.6C7,11.6,6.9,11.6,6.6,11.6C3.1,11.1,1.2,8.8,1.2,6.3C1.3,3.4,3.6,1.2,6.5,1.2zM6.5,0C3,0,0.1,2.8,0,6.2C0,9.4,2.3,12.2,5.5,12.8C5.9,12.8,6.1,12.8,6.5,12.8C6.8,12.8,7.2,12.8,7.4,12.8C10.6,12.3,12.8,9.8,12.9,6.7C13,3.1,10.3,0.1,6.7,0C6.6,0,6.6,0,6.5,0z" transform="matrix(1,0,0,1,5.25,5.63)" id="422"></path>
            <path fill="#f7d77c" d="M8.7,14.8C8.6,14.8,8.3,14.8,8.2,14.8C8,14.8,7.8,14.8,7.5,14.8L6.3,14.8C2.7,14.2,0,10.9,0,7.2C0.1,3.2,3.4,0,7.5,0C7.5,0,11.6,0.9,13.1,2.3C14.5,3.8,15.2,5.7,15.1,7.7C14.9,11.3,12.2,14.3,8.7,14.8z" transform="matrix(1,0,0,1,17.13,2.11)" id="423"></path>
            <path fill="#4c4c4c" d="M8.1,1.4C8.2,1.4,8.2,1.4,8.4,1.4C10.3,1.5,12,2.3,13.2,3.6C14.5,5,15.2,6.7,15.1,8.5C15,11.9,12.5,14.5,9.3,15C9.2,15,8.9,15,8.8,15C8.6,15,8.4,15,8.1,15L7,15C3.7,14.4,1.2,11.5,1.2,8C1.4,4.4,4.4,1.4,8.1,1.4zM0,8C0,12,3,16.1,8.1,16.2C12.9,16.2,16.2,12.4,16.3,8.4C16.4,3.9,12.9,0.3,8.4,0C3.6,-0.1,0.2,3.6,0,8z" transform="matrix(1,0,0,1,16.42,1.21)" id="424"></path>
            <path fill="#69d9e2" d="M40.8,0L35,7.1C34.7,4.4,32.9,1.8,30.3,0L29.4,1.3L24.9,6.7L23,9L19.3,4.9L15.7,1C12.3,2,10.8,4.5,10.6,7.2L3.5,0.8C1.3,2.4,0,4.5,0,6.9L0,11.6L10.6,11.6L10.6,13.9L35,13.8L35,11L44.4,11L44.4,6.2C44.4,3.8,43.1,1.6,40.8,0z" transform="matrix(1,0,0,1,1.87,21.37)" id="425"></path>
            <path fill="#4c4c4c" d="M41.6,2.8C39.9,1.3,37.6,0.5,35.1,0.5C33.2,0.5,31.4,1,29.9,1.9C28,0.7,25.7,0,23.2,0C20.4,0,17.8,0.9,15.7,2.5C14.1,1.6,12.3,1.1,10.4,1.1C8.1,1.1,6,1.8,4.2,3.1C1.7,4.6,0,7.1,0,9.9L0,15.3L10.6,15.3L10.6,17.6L36.2,17.6L36.2,14.8L45.6,14.8L45.6,9.7C45.6,9.6,45.6,9.4,45.6,9.4C45.6,6.7,44,4.3,41.6,2.8zM35.1,1.6C37.1,1.6,38.9,2.2,40.4,3.3L35.9,8.8C35.2,6.3,33.5,4.1,31.1,2.6C32.3,2,33.7,1.6,35.1,1.6zM16.8,3.2C18.6,1.9,20.9,1,23.2,1.1C26.8,1.4,29.7,3,29.9,3.2C29.9,3.3,23.5,11.2,23.5,11.2L20.5,7.7L16.5,3.3C16.6,3.3,16.7,3.3,16.8,3.2zM10.7,10L5.1,4C4.7,4.2,4.4,4.5,4.1,4.8L10.6,11.9L10.6,14.1L1.2,14.1C1.2,14.1,0.3,6.6,4,4.6C4.4,4.5,4.7,4.2,5.1,4C6.6,2.9,8.4,2.2,10.4,2.2C11.9,2.2,13.4,2.6,14.7,3.3C14.7,3.3,14.6,3.4,14.5,3.4C12.4,5.2,11,7.4,10.7,10zM44.3,13.5L34.9,13.5L34.9,16.5L11.8,16.5L11.8,11.4C11.8,11.4,11.7,8.9,13.9,6C13.9,6,15.4,4.3,15.5,4.1L23.5,13.1L30.9,4C32.7,5.3,34,6.9,34.5,8.7C34.6,9,34.7,9.3,34.7,9.5C34.8,9.9,34.9,13.5,34.9,13.5L36.2,13.5C36.2,13.5,36.2,10.7,36.1,10.5C36.1,10.5,36.1,10.5,36.1,10.5L36.8,9.5L41.3,4C42.8,5.4,43.8,7.3,44.1,9.5L44.3,10.5L44.3,13.5z" transform="matrix(1,0,0,1,1.31,17.5)" id="430"></path>
        </g>
        <text class="st6">
            <tspan style="white-space:pre" y="56.8" x="8">按 user 分组统计视图</tspan>
        </text>
    </g>
    <g ed:width="134.984375" ed:parentid="915" ed:layout="rightmap" transform="matrix(1,0,0,1,502.28,1190.79)" id="116" ed:height="75.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,75.6L135,75.6"></path>
        <g transform="translate(44.04,1.83)">
            <path fill="#fffdfd" d="M45.7,17.8L25.7,1C25.3,0.4,24.4,0,23.6,0C22.7,0,22,0.3,21.4,0.9L1.3,17.7C0.1,18.6,-0.3,20.1,0.2,21.6C0.7,23,2.1,23.9,3.5,23.9L4.6,23.9L4.6,43.7C4.6,46.1,6.5,48,8.8,48L38.2,48C40.6,48,42.4,46,42.4,43.7L42.4,24L43.5,24C44.9,24,46.3,23,46.8,21.7C47.2,20.2,46.9,18.7,45.7,17.8z" transform="matrix(1,0,0,1,0,0)" id="465"></path>
            <path fill="#4c4c4c" d="M22.3,1.2C22.6,1.2,22.8,1.3,23,1.5L43.1,18.3C43.8,18.9,43.4,20.2,42.4,20.2L38.8,20.2L38.8,42.5C38.8,43.6,38,44.4,37,44.4L7.6,44.4C6.5,44.4,5.8,43.5,5.8,42.5L5.8,20.3L2.2,20.3C1.3,20.3,0.8,19.1,1.5,18.4L21.6,1.6C21.8,1.3,22.1,1.2,22.3,1.2zM22.3,0C21.8,0,21.2,0.3,20.9,0.5L0.8,17.2C0.1,17.9,-0.2,18.8,0.1,19.8C0.5,20.8,1.3,21.4,2.3,21.4L4.6,21.4L4.6,42.5C4.6,44.2,5.9,45.5,7.6,45.5L37,45.5C38.7,45.5,40,44.2,40,42.5L40,21.5L42.3,21.5C43.3,21.5,44.1,20.9,44.5,19.9C44.9,18.9,44.5,18,43.8,17.3L23.7,0.6C23.4,0.3,22.9,0,22.3,0z" transform="matrix(1,0,0,1,1.25,1.24)" id="466"></path>
            <path fill="#304352" d="M1.1,10L6.9,10C7.5,10,8,10.4,8,11L8,16.9C8,17.3,7.5,17.9,6.9,17.9L1.1,17.9C0.5,17.9,0,17.4,0,16.9L0,11C0.2,10.5,0.6,10,1.1,10zM5.2,1.1L5.2,7C5.2,7.6,5.7,8.1,6.3,8.1L12.1,8.1C12.7,8.1,13.2,7.6,13.2,7L13.2,1.1C13.2,0.5,12.7,0,12.1,0L6.3,0C5.7,0.1,5.2,0.6,5.2,1.1zM10.4,11L10.4,16.9C10.4,17.3,10.8,17.9,11.4,17.9L17.2,17.9C17.7,17.9,18.2,17.4,18.2,16.9L18.2,11C18.2,10.4,17.8,10,17.2,10L11.4,10C10.8,10,10.4,10.5,10.4,11z" transform="matrix(1,0,0,1,14.45,27.72)" id="467"></path>
            <path fill="#69d9e2" d="M20.4,0.3L0.4,17.3C-0.3,17.9,0,19.3,1.1,19.3L41.3,19.3C42.3,19.3,42.7,18,42,17.3L22,0.3C21.4,-0.1,20.8,-0.1,20.4,0.3z" transform="matrix(1,0,0,1,2.41,2.47)" id="468"></path>
        </g>
        <text class="st6">
            <tspan style="white-space:pre" y="68.8" x="8">按 host 分组统计视图</tspan>
        </text>
    </g>
    <g ed:width="66" ed:parentid="393" ed:layout="rightmap" transform="matrix(1,0,0,1,608.28,607.12)" id="122" ed:height="20.5833">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,20.6L66,20.6"></path>
        <text class="st4">
            <tspan style="white-space:pre" y="14.6" class="st2" x="8">入门使用</tspan>
        </text>
    </g>
    <g ed:width="66" ed:parentid="393" ed:layout="rightmap" transform="matrix(1,0,0,1,608.28,174.83)" id="126" ed:height="20.5833">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,20.6L66,20.6"></path>
        <text class="st4">
            <tspan style="white-space:pre" y="14.6" class="st2" x="8">开启情况</tspan>
        </text>
    </g>
    <g ed:width="78" ed:parentid="393" ed:layout="rightmap" transform="matrix(1,0,0,1,608.28,342.79)" id="128" ed:height="20.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,20.6L78,20.6"></path>
        <text class="st4">
            <tspan style="white-space:pre" y="14.6" class="st2" x="8">启用和关闭</tspan>
        </text>
    </g>
    <g ed:width="116.90625" ed:parentid="104" ed:layout="rightmap" transform="matrix(1,0,0,1,502.28,916.12)" id="156" ed:height="22.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,22.6L116.9,22.6"></path>
        <g transform="translate(7.04,2.21)">
            <use xlink:href="#imgstar4" transform="translate(0,0)"></use>
        </g>
        <text class="st4">
            <tspan style="white-space:pre" y="16.1" class="st5" x="27">SYS_CONFIG 表</tspan>
        </text>
    </g>
    <symbol id="imgstar4">
        <image xlink:href="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAYAAACqaXHeAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMTM4IDc5LjE1OTgyNCwgMjAxNi8wOS8xNC0wMTowOTowMSAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTcgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOkQ1Nzk1RkJFNEZFMjExRTc4NkFDQTJCRDQ3QzQ1MDI0IiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOkQ1Nzk1RkJGNEZFMjExRTc4NkFDQTJCRDQ3QzQ1MDI0Ij4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6RDU3OTVGQkM0RkUyMTFFNzg2QUNBMkJENDdDNDUwMjQiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6RDU3OTVGQkQ0RkUyMTFFNzg2QUNBMkJENDdDNDUwMjQiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz4/zZk4AAAEf0lEQVR42uSbW0gUURjHj2ZeSi27WKkZhZcsMystoqgoKoKgGyH0UCRUKj0EQUVQQiWVBgVd8El6iO5R9BBF1oNUBEqBRVBUSPerlZqZpdv/Y76lRdZ1ztmZ2ZnZD36UuzuX89853/nO/5yNKKotFyGO2eBOqC4eFcKG07UfgQHgFigOxU1EhlCA2yALpIO14G44CbACZPtcvx/IAxvCRYBDILnHa/GgIhwEWAiSenkvFqxxuwB7AwiQAA66WYB87vuBgkRY7lYB9oBBfXyG3q90owCpYD6I0PFZSpCL3CYAlZzROj9LT0GVmwSIAetBf4ljxnCZ7AoBdoJOyWPoKdjnFgG2cc0vG1N45HC0AGXgj+KxieCA0wUo57FdNebyCOJIAVZJZH4RoDyucqoANLkZbMB5igwQ0nIB5viZ8QVzn5VOE6AiwKRHJUrNuEmzLLEJbHAYGb+FZqFdAw9BI3gSSgESuFpLZ8aCHP43VbLq03u9XBa3jV8jE6WJBbnHohCfjBAglRvmbST5d5n8/xFCs7E6QDeXurE6JzpGdNtEn7/HMUvBL74XqjuegvugwUeY7kACpHO/XQyGgXY+USQ3zl8Wjhb2iRiGIg4UggLwkxtOT9ALUAsug5vA4xVgIr+R7JMYBwrnRwR3E29kMOvAezJnorjRV8BIET4Rx92mkb7tFoOKFSfGl0hOZEdBa5g1/juV6t7+Tl4drcx0hUnj34IF4LNvJbiEM6bb4xvYCB74K4XTFJwbJ0Uz2MzVpN+5AOWBWdw/3PjNkzdxuq/JUAMXED9clvAOg2N6Z4NUMW0CX13QeBrma4S2JCc1HT4nND+v2cGNpy59CWxV9QPOgxKHitDO5X1xsIbIBe4OHxzU+A6eCa7UM7XUExeFtrLjhNGBhvHHXOgIowSguC60Bcs2Gze+iw2SQhlzQSbqwSShvtBhZnRzN82WOUjFFCWFh4C/Nhzu0mQPUnWFqRuQ5/fGRpMbJQc6GFt8OBhqEwHII8y0WoDxQrOq7RAezk2WC2AXU5T8y3yrBZgs1Nb8zQiy6GdaLcBUm40CuVYLkGEzASgpx1glQDTXAnaKNpVEqCoAJUC7+YdRVgqQY8NSmEaCAqsEoKW0eBuKMMMqAaaJ0P7apLfICucu4M0DyVYIMNqg6Sut51dzBvcYcM5O2USoIgBtmOgI8kbJWSK/jnaTlHJVSev1wVrxtOqbZ7YA9PirriG2sJ+wTGgbMT7y6y/5b9pOXx+EENGyJbGKAFQDxEoeQ7NGMlC287de18vnaL1uutDMzGeKQowyW4AUiZLTw/27hg2Uap3H0W8Kydqi3xO+5idHb2SaLUCSRD+v45qhTPGRviq0vUsl3F1adXYDUwVo1fH+O7AazAOvDMjuZ4S2hWeL0BY524xqk4oAz3n48jcE0SO/W2hb7GpNGOdreBK2g0XwJ0ST2QKc6DEMUoIjm/w4n++IBQUPXYu2ve0S//cqCk60UtdX3SmawomKjr8B9gttLc7qoMaeEtoeJ6olzoKTMif4J8AArIjH4LQYI48AAAAASUVORK5CYII=" width="16" height="16"></image>
    </symbol>
    <g ed:width="85" ed:parentid="156" ed:layout="rightmap" transform="matrix(1,0,0,1,652.19,967.29)" id="164" ed:height="22.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,22.6L85,22.6"></path>
        <g transform="translate(7.04,2.21)">
            <use xlink:href="#imgstar5" transform="translate(0,0)"></use>
        </g>
        <text class="st6">
            <tspan style="white-space:pre" y="15.6" x="27">配置方法</tspan>
        </text>
    </g>
    <symbol id="imgstar5">
        <image xlink:href="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAYAAACqaXHeAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMTM4IDc5LjE1OTgyNCwgMjAxNi8wOS8xNC0wMTowOTowMSAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTcgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOkUyRkUxQjZENEZFMjExRTc4Q0QxRDdFODI5QjA5NkQ2IiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOkUyRkUxQjZFNEZFMjExRTc4Q0QxRDdFODI5QjA5NkQ2Ij4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6RTJGRTFCNkI0RkUyMTFFNzhDRDFEN0U4MjlCMDk2RDYiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6RTJGRTFCNkM0RkUyMTFFNzhDRDFEN0U4MjlCMDk2RDYiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz7nEaD4AAAEiElEQVR42uSbaUhUURTHr+OupW1aSotim6VFmRUVbfSpxU9FSEn0sY0SS7Issc0SpCjCPiWBERUURkREUQZB+KmsoGyBIjJazFLLLe1/eMcYhnFm3n3LvPfmwA9R77x373/OO/eec+8Lm3PutQiyzQUNwbq5K4gDTwNt4DLYG6xORARRgCYQCYaAA2AEKA4VDzgDwtx+jwNbwPhQECAVbPbifdGgPBQEOM6D9TR6HPJBopMFGAnW+Yg9f8FBJwtQ4eeeFAsKPeKDYwSgaL8JRPlp1wpKnCjAUdAfQLvhoNRpAtC3vm2Q4OfNfnF7xwhwGPSoaD8alDlJAFrkxKj8TCcocIIAeyTvM9YMLzBDAFrfx0t8jqbCWJBnZwHyNCZcY8AhOwtA09kwjf1LAUvtKEAmmKLDdZLAETsKUCL57HuLBdPALDsJQJndBhCu0/XoMTpmREeNqggV8jwep9P1yAtmgrvgPngKGsGHYAlA32wamMA/09lNM3j+7tZx8AOWwiwGHdx3gqq6DUwj81urANE+BpjKLtnO+Xskz9dmrSojPWaWbCafhae48wM8B4/AExbljT8B5oF9POXEs8p9fMM4Lzn6UGEti3PzumSwHCzhcYSxKF/BLXCTPea/AGvABU5FByxB2N/C3cZBXxgVXXNAEceS1S5W6bTH4J1u5CmrwB0SoIurNaFmFL9ekQCPwUv+QyhZM9g5ELkp8H0OocG/BeMoyLvc3IE2Kf+EiAATvS2FP4FcNYsIGxqtExJ95QIvwEqHegKtByYJpeDqMxmqF0r93kki0KAXeMsdBlu+XgXbWTW7G60A83g5rCodPs9L4zYbD75FKDvR9bL1AFoh0n7eT5sOfjeo01oQqWAV22zm9lRGq/HXMNAU9rpQ9vZabDB42lw9C04G0lhNDn9CKHv33y0e7WuFcuZI6C2AYGV3WNQTqEBzg/snjBKA7JJQCp6tFho8rVkeCIm9RNky1m2wEXyziAAPhVLUEWYJQEaVlhiLCLBQ9oNaBMgS+ld+tVia2QLkiOAetXW3Pk7nTRVgqoW+fSp4LjJbgHEWEoDK3kvNFCDDgpligpkCZPJzZyVLkvFKl4bnP9ZiAvTJxCVZAWgGiLKYADQlZ5slQLawntFY5pslQLpFs8EsMwRIFiae5jZ6apYRgAJNlw6d7eD8/Z6OmSWda0g0WgCaAiM1dJKOzvQK5fwfdXYFWCuUl6i01h671M4EMgLMEHKnv3p4qjrFAla6/Y+8gI7U0X7ERyFff4wwQ4DZKtv3s6tXcfrs62WIOn6Ot3KtoV3lvWLUZoUuSZUDtS9COaOTywMP9Mh8La/sijlWqNmvnGy0AIFMNS082CL2mCZJl64WyuGNcr5eZwCfMTwIRvtZjlLV+AqvFGt1iu6VfL0q/r3bR9soIwWg5ea7Qf5HUxmdNFkmlBckjLBS7nM1i9DrpU2zkQLQs3jR41nu5iBH+4jTwTODFzsUVHeBUeCax9RJ/VL13qHMSdEyjs4FHORqWBSzjabK9Rxj6J0kOvWxH7xXc5F/AgwAlB7Kc/hRXl8AAAAASUVORK5CYII=" width="16" height="16"></image>
    </symbol>
    <g ed:width="73" ed:parentid="102" ed:layout="rightmap" transform="matrix(1,0,0,1,502.28,43.92)" id="389" ed:height="22.5833">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,22.6L73,22.6"></path>
        <g transform="translate(7.04,2.21)">
            <use xlink:href="#imgstar1" transform="translate(0,0)"></use>
        </g>
        <text class="st4">
            <tspan style="white-space:pre" y="15.6" class="st2" x="27">是什么</tspan>
        </text>
    </g>
    <symbol id="imgstar1">
        <image xlink:href="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAYAAACqaXHeAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMTM4IDc5LjE1OTgyNCwgMjAxNi8wOS8xNC0wMTowOTowMSAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTcgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOkI2OEQ4MUJENEZFMjExRTdCNjg2REUzMzYwNzdFOTQ4IiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOkI2OEQ4MUJFNEZFMjExRTdCNjg2REUzMzYwNzdFOTQ4Ij4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6QjY4RDgxQkI0RkUyMTFFN0I2ODZERTMzNjA3N0U5NDgiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6QjY4RDgxQkM0RkUyMTFFN0I2ODZERTMzNjA3N0U5NDgiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz5uTGucAAAEXklEQVR42uSbW2xMURSG9ww1qo1WhaJKI7RNVbWCSF1fCJoiRIRW3BOEBA8ekKCIeOLNG0880JCgGveKVJrQEBJ1beJBhJbqTUcvU+vPWZM0TWd6zj73Myv5Mg8zZ87Z/6y991prr/HVxacLG62QeG7nA/htvPdN4gbxxE4Bhtp035PEUiKBSCU+EZlEbyx4AAa8mQcftinsDTExBcqI9AGeYxkxwesCxBE7I0y9YcRprwtwjOiMsh6VEoleFuAwMTzK+728QHpSgD1E9yCfwTQ46FUBTqh07yBPFU8JsI4IqPxsPHHEawJgdU/S8PkQsc8rAiwixmm8JsGqxdAKAc4QyZJh+ia3C5BD5EleO5I463YBynQGNlg3itwqwFhipc57QIBzbhXguEHpdhqx0KyH9JlUEfJz1Ocz6PuqiQVu8oCjRIeB35erYzG1xQPa+hU89BoCo/fEM+Il8YYJ2iXACGIyMYlfUdHJ5tc09qxkk7yrjacXdpdG4q1QCquvWZR6IwQY02dwYCqRRWQIpaQVYBcPcQYXb+B8l7EeFsbPz/aFPaWmj7e0RBMgiUPP5fwrQuF/PKjAIDm8k62DxwGP/UjUEeVEBdEeFmA0vdYS4/nX9LqhIvWHWAJB4DKP2M1jYfDhoguCtPv40f08f2LRMO7vEOAA0RRjg8fusSUcCGFvPc/zIhasiVP0p30jwVNCOZlp8fjgW4krxIWBQuEdQjmwbPbo4LHt3SL2R8sFthK3PegJiAdwCl2qJhnCweUddhcvGAKhF0SxlmywhKMlt4vQzdHfYpl0eCNR6WIRkKd8JQr01AM2EPdcKALOGBs4idNdEFlPPORsyy2GZ1V1FqG2IrSWeOwSEbqEUlIXRgoAW01UhdNIB897TZUorTXBYg4hnShCkLO8LjMFgBVx/tDusDk/nfil9ULZqvAKriM4wZp5n6+XuVhPWXyeQwRA41W+7MWyAqAImuIQAVDvm2O1ALkO2xLn2iHAEAcJkGm1AJhziQ4SAD9GqpUCwOV8DhIApe4ZVgqQ5bAgCAvhTKsESBLOK5vFyW7LMgJME+YdfOqxAisF8DtQgAyrBMgUxp79G2V/ZdYmGQHyHRYDhA1VoDwrBMg26IFRer/MObwR/xVKlMkJZATQ21ODHeSVUI6ntwul/+CqiPxHCi1jKTRbgBQd7o/C6jehtL/OYhFgKF3jwAIdKZU66ww5ZguAHUBrY1Ing1PoicTdCJ/7IZTGSkSZ6PlpkIwIE80UAA1QWvoJ2niew80vqbzmHTGfWMVe0qzRQ0eZKUBApcJY4Gp5Udot6c41PFXWCKUTTI0QPmFyUbRnkCQoyHN4FzFbKN1aeq2KtzecT3wQ0fsYQmZPAQzud4Q9GIHIRX6Aaybs8w94Cy5hYX9G2GECZgpQ0e/GIZ7n1zlCPGRBwINFFEde23jhbO23CFZr+TKZTlGIVs4DRjMi/g/02cYIcC+DznQ0VDdqufi/AAMAwrzVVV0VhtwAAAAASUVORK5CYII=" width="16" height="16"></image>
    </symbol>
    <g ed:width="73" ed:parentid="102" ed:layout="rightmap" transform="matrix(1,0,0,1,502.28,108.88)" id="391" ed:height="22.5833">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,22.6L73,22.6"></path>
        <g transform="translate(7.04,2.21)">
            <use xlink:href="#imgstar2" transform="translate(0,0)"></use>
        </g>
        <text class="st4">
            <tspan style="white-space:pre" y="15.6" class="st2" x="27">有什么</tspan>
        </text>
    </g>
    <symbol id="imgstar2">
        <image xlink:href="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAYAAACqaXHeAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMTM4IDc5LjE1OTgyNCwgMjAxNi8wOS8xNC0wMTowOTowMSAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTcgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOkJFNjYxREY2NEZFMjExRTc5MERBQURFNUJBQTdGRTI4IiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOkJFNjYxREY3NEZFMjExRTc5MERBQURFNUJBQTdGRTI4Ij4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6QkU2NjFERjQ0RkUyMTFFNzkwREFBREU1QkFBN0ZFMjgiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6QkU2NjFERjU0RkUyMTFFNzkwREFBREU1QkFBN0ZFMjgiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz5VTLd+AAAEaklEQVR42uSbWUhUURjHz0zqmFpa2aK2qaXti7agtNFKRrSAUdBDBUUv4UM+VJKRlJER9BYVFT0EPbVSURJFJfVU2h5BEbRnZmZqlk7fn/tJwzAz3jl3mXvvfPB7cLxz7/n+9zvnfOc7Z1ytJcNFBG0s8TSSDXBH8NllRA3xKpICxETouQuJUiKZ8RKJREu0REAlkeL32eto6QKriZwAn3uIldEgAN5+rwCfIyL2OV2AeURqiP/jf/OdLEBVkLffZX05QhwpwEQiV8V12cRUJwpwgKe67qwPscdpAmQQM1Ve6yKmqYwW2wiwn4gP4/okosIpAsQRayQy1CIizQkCVHJYy6Tpu4xunMuE1eBvjgIZ+8MJUotdI6BE4/chQLmdI6Ce6KfxHp1EDztGQHGYI38wayW22zECUOgYodO9fvHUaJsImEP01/F+KJhsNqKhRlWEkMom63i/JO4Gs4j7xCOmIVICoHgxzIdMYjQvZDIMEnUos5yn1ngeH54T94gHLMoTPQRI9nMQjo1iR9PE//pdBxFLJEgmOzLWkxEsQiFRQDRzV8Fy+y3xkIWpY2E+hRoE4UQpp5/5PDa08Q09Pg+0m7Xzi0IihprjZ+ICcZ146SvAC2IIv0mnWyd3od1YpKELXBFKkdIlosPcHNF7iXT88VVEp2H8qnXz/NoRZc5jfDtFnHTzVILB72OUON/KY94m30ywmthJfHO48+3s/ORAqfBxzuCcKsJfzg/yQq0FDjENDnMeY9wXEWBLzh0kjz9GfHfQvN8cLEUPthrcRpwmGm3uvJffforMcngLcZ5osvnbT9RSD1hPXOOChN0MbR4klLqipoLIKqFsa7XYyHl03QlCqUkKrQIIXjggEn7awHlM43OFyhMn4ZTEcHrjIvHDws5jXVPMtQChtwCwtcQZi84O9byuuRnu0jBcw0NOWCxPQNjvIM7KrI1lbCtxiacZK8z1FZy8CbME6Eov3RYQAIWcQi3VEVnLsVAXyIuEANkWEiDTbAHwvYEWK3KMNFOALIvlAxgIx5spQDYXGKxi2DqbZHYEeCwkAPwoMFOAAcKg7WoNNs5MAYqE9TZSsB0fZ5YAucJ61sxLYMMFSBXyp76MtBiZmUBGAKjcZkEBUPqaYoYAUFmPw0/II3Dao0nHKXW6GQLgIVrOC6CqhG24dTx14TBGFSczXo0C5JghQL5k41BTRKESBzHShVJx7rIybgt2p9o1CBErwjycJSNAlsSyGSFezrnD0RDXbmRxcIrjg4QQ6E5jjBQAzr8Ls59f5TA/qPI7qO6sEEphs5qdUitEktER4FH5AIT6e2IpI1NSxxmeRVzsuKFSCAzOCUYKEKsy3FGiGkzc0WFkx2+LFxAziFvdrEJdQt3PcqQFCHWkBpXiy0RvHtX1tsfcLeYQt0XgAx2oC3QaKUCtUGruXr9+XsOhvowbYaShDbOJxUI5A9jolw0eMXoWwAbJYeIZcY5YwuF51+TMr45rABv4BbyRWQv8E2AA5NLbjbxUGAEAAAAASUVORK5CYII=" width="16" height="16"></image>
    </symbol>
    <g ed:width="73" ed:parentid="102" ed:layout="rightmap" transform="matrix(1,0,0,1,502.28,438.17)" id="393" ed:height="22.5833">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,22.6L73,22.6"></path>
        <g transform="translate(7.04,2.21)">
            <use xlink:href="#imgstar3" transform="translate(0,0)"></use>
        </g>
        <text class="st4">
            <tspan style="white-space:pre" y="15.6" class="st2" x="27">怎么用</tspan>
        </text>
    </g>
    <symbol id="imgstar3">
        <image xlink:href="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAYAAACqaXHeAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMTM4IDc5LjE1OTgyNCwgMjAxNi8wOS8xNC0wMTowOTowMSAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTcgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOkNEMzM2QTM3NEZFMjExRTc4RDY1RDY3NTVGMDlFMjk2IiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOkNEMzM2QTM4NEZFMjExRTc4RDY1RDY3NTVGMDlFMjk2Ij4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6Q0QzMzZBMzU0RkUyMTFFNzhENjVENjc1NUYwOUUyOTYiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6Q0QzMzZBMzY0RkUyMTFFNzhENjVENjc1NUYwOUUyOTYiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz6U7HANAAAESklEQVR42uSbWUgVYRTHv3s1NZebhZYtYqWGWhaR2ErRSiUtVERED9KD9BQUlRAtBGZFEQSRPfRQ0YNEVBQ9FEkREREI7dpGBS22mes119v/MOfhYnmd+WYfD/wYdO7Mne8/5ztzzpnv+kJXhJ2WC2rsvAC/jd9dCh6CdyDKrouItul757MAAaaTty0DxQMOgaFhf/vAh4EyBdaBnD68cYPVF+OzIQi+Btl97HsPxnvZA+aCtAj7h4BlXhbgCEiKsH8YKPeqAHkgX8Xn0sFsLwpAdz9BxefIC8q8JkAqWKw2MIPJKr3FNQLQ3Y/R8HmKEwe89BgMSRzTxJ7w0e0ecBD0SBxHHrPfCx4QBIMlj20Do8Fvt3rAFp2VXshsLzDbA+rACJ3n6OHp0O02D1gJEg04D02DvW70gOdgokHn6gCxbvKAGZzSGmXtYJubPKAKLDD4nJ/AfaG00Z4yv+wSgNwxI4xxQmlwZvJji6J3ikne9YeJ420Ni1LNojwzQoBArwHSwHK4WTGSi5ogR+ZBIJ5zeLuMBG/hbYCzx8fgAXjCwnyNJAANYjsoAgX87G7jE8bqSGTstg4eB42Puk3fwDVwE9SGC1DLQSteeN96OKhS46WMGpHXwQSbXdhK87NHU7WZ5udsbSBaF8UGmgI+yWrNzUZToBIU+znQzesrSnrQ6NFJrfni8EzwHtgF6j0+eHoF95YbLf+kwhfAHg+LQDnLF9Gr19i7FqgAhz0oQjePaayaYugoOGlmF8aGLJHm/XAt1SB1Yc6CRo+IkChTDlNqfFEo3Vm3GqXCSXr6ASXgBmh24eCbuUpt1dsQ2ciBMeiiwVPAK+SoL/QKILhwcIsn/BTKK/ZatYWBWlsPLjk8MP4Am8AjLZWRFtsMzoMGh975rVzrC7MEEPwlFQ7LE+r50V0pUxvL2G6eDl0OEYBWnZ2SbQ7oKSyiHSKA9IoSPQLkOGgKTLFDgCwHCZBhhwBjHCQAtcTzrBQgQ9iwrjeC+YTkmiJZATK50HBStTfVSgHoDVGMwzxglpUCpPZXZtpguVYKUCSc9yIlIHNT/FaqbbK1yQRCGQFoRXeCAwWIsUqAfFbbaUbv+6ZbJYAR63Wor1DNW6OKqgIrBCgU+tYLUAJVx70FuuBkrubo/WRIpwDZVggwTfLigsxOoawyuRy2b59QFmWc4QRLVgh6AZJutgBZEhfVyQ0LCp6nI3y2hMW5Cj5LCNGqtSbQKsAo8F3jPL8llB9BHFN5DHV31oCFfGyTBiEoD0gxU4A4zgLV3Ama56vBcsnC6RVYCmaC2yqFiNP6iNYqQKwKd6c5XM6ufNeAyP4SLOFcv6ofISg7jTdTgKgIU6CBXTZFmPPLrxdC+dnNHHBH/H9BR7vWKlVmoSS9L1wbJl4ju2upQXdcrU0C57gyTeb/dWqtUmVXip4Ai8AbcFwoK0zsslVgh1BWqK5gT1FtfwUYAN0E6f+FyjlHAAAAAElFTkSuQmCC" width="16" height="16"></image>
    </symbol>
    <g ed:width="72" ed:parentid="389" ed:layout="rightmap" transform="matrix(1,0,0,1,608.28,45.42)" id="395" ed:height="19.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,19.6L72,19.6"></path>
        <text class="st4">
            <tspan style="white-space:pre" y="14.6" class="st7" x="8">SYS系统库</tspan>
        </text>
    </g>
    <g ed:width="269.484375" ed:parentid="395" ed:layout="rightmap" transform="matrix(1,0,0,1,713.28,29.42)" id="411" ed:height="51.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,51.6L269.5,51.6"></path>
        <text class="st4">
            <tspan style="white-space:pre" y="14.6" class="st8" x="8">帮助DBA和开发人员解释由Performance Schema收</tspan>
            <tspan style="white-space:pre" y="30.6" class="st8" x="8">集的数据。SYS库中的对象可用于典型的调整和诊断</tspan>
            <tspan style="white-space:pre" y="46.6" class="st8" x="8">用例。</tspan>
        </text>
    </g>
    <g ed:width="56.0156" ed:parentid="476" ed:layout="rightmap" transform="matrix(1,0,0,1,782.28,150.25)" id="445" ed:height="18.5833">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L56,18.6"></path>
        <text class="st4">
            <tspan style="white-space:pre" y="12.6" class="st2" x="10">&lt;=5.6</tspan>
        </text>
    </g>
    <g ed:width="56.0156" ed:parentid="476" ed:layout="rightmap" transform="matrix(1,0,0,1,782.28,175.83)" id="474" ed:height="18.5833">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L56,18.6"></path>
        <text class="st4">
            <tspan style="white-space:pre" y="12.6" class="st2" x="10">&gt;=5.7</tspan>
        </text>
    </g>
    <g ed:width="42" ed:parentid="126" ed:layout="rightmap" transform="matrix(1,0,0,1,707.28,162.04)" id="476" ed:height="20.5833">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,20.6L42,20.6"></path>
        <text class="st4">
            <tspan style="white-space:pre" y="14.6" class="st2" x="8">版本</tspan>
        </text>
    </g>
    <g ed:width="42" ed:parentid="445" ed:layout="rightmap" transform="matrix(1,0,0,1,871.3,149.25)" id="478" ed:height="20.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,20.6L42,20.6"></path>
        <text class="st4">
            <tspan style="white-space:pre" y="14.6" x="8">没有</tspan>
        </text>
    </g>
    <g ed:width="90" ed:parentid="474" ed:layout="rightmap" transform="matrix(1,0,0,1,871.3,174.83)" id="480" ed:height="20.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,20.6L90,20.6"></path>
        <text class="st4">
            <tspan style="white-space:pre" y="14.6" x="8">开始有该功能</tspan>
        </text>
    </g>
    <g ed:width="42" ed:parentid="126" ed:layout="rightmap" transform="matrix(1,0,0,1,707.28,200.42)" id="482" ed:height="20.5833">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,20.6L42,20.6"></path>
        <text class="st4">
            <tspan style="white-space:pre" y="14.6" class="st2" x="8">查看</tspan>
        </text>
    </g>
    <g ed:width="101" ed:parentid="482" ed:layout="rightmap" transform="matrix(1,0,0,1,782.28,201.42)" id="484" ed:height="18.5833">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L101,18.6"></path>
        <text class="st4">
            <tspan style="white-space:pre" y="12.6" class="st2" x="9">show engines;</tspan>
        </text>
    </g>
    <g ed:width="42" ed:parentid="128" ed:layout="rightmap" transform="matrix(1,0,0,1,719.28,327.5)" id="486" ed:height="20.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,20.6L42,20.6"></path>
        <text class="st4">
            <tspan style="white-space:pre" y="14.6" x="8">启用</tspan>
        </text>
    </g>
    <g ed:width="321.828125" ed:parentid="486" ed:layout="rightmap" transform="matrix(1,0,0,1,794.28,226)" id="488" ed:height="223.5833325088024">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,223.6L321.8,223.6"></path>
        <text class="st4">
            <tspan style="white-space:pre" y="19.6" class="st9" x="8"># 启用所有`wait instruments`</tspan>
            <tspan style="white-space:pre" class="st10" x="179"> </tspan>
            <tspan style="white-space:pre" y="41.6" class="st11" x="8">CALL</tspan>
            <tspan style="white-space:pre" class="st12" x="37"> sys.ps_setup_enable_instrument(</tspan>
            <tspan style="white-space:pre" class="st13" x="233">'wait'</tspan>
            <tspan style="white-space:pre" class="st12" x="262">);</tspan>
            <tspan style="white-space:pre" class="st10" x="270"> </tspan>
            <tspan style="white-space:pre" y="63.6" class="st9" x="8"># 启用所有`stage instruments`</tspan>
            <tspan style="white-space:pre" class="st10" x="187"> </tspan>
            <tspan style="white-space:pre" y="85.6" class="st11" x="8">CALL</tspan>
            <tspan style="white-space:pre" class="st12" x="37"> sys.ps_setup_enable_instrument(</tspan>
            <tspan style="white-space:pre" class="st13" x="233">'stage'</tspan>
            <tspan style="white-space:pre" class="st12" x="270">);</tspan>
            <tspan style="white-space:pre" class="st10" x="278"> </tspan>
            <tspan style="white-space:pre" y="107.6" class="st9" x="8"># 启用所有`statement instruments`</tspan>
            <tspan style="white-space:pre" class="st10" x="214"> </tspan>
            <tspan style="white-space:pre" y="129.6" class="st11" x="8">CALL</tspan>
            <tspan style="white-space:pre" class="st12" x="37"> sys.ps_setup_enable_instrument(</tspan>
            <tspan style="white-space:pre" class="st13" x="233">'statement'</tspan>
            <tspan style="white-space:pre" class="st12" x="297">);</tspan>
            <tspan style="white-space:pre" class="st10" x="305"> </tspan>
            <tspan style="white-space:pre" y="151.6" class="st9" x="8"># 启用所有事件类型的`current`表</tspan>
            <tspan style="white-space:pre" class="st10" x="196"> </tspan>
            <tspan style="white-space:pre" y="173.6" class="st11" x="8">CALL</tspan>
            <tspan style="white-space:pre" class="st12" x="37"> sys.ps_setup_enable_consumer(</tspan>
            <tspan style="white-space:pre" class="st13" x="227">'current'</tspan>
            <tspan style="white-space:pre" class="st12" x="274">);</tspan>
            <tspan style="white-space:pre" class="st10" x="282"> </tspan>
            <tspan style="white-space:pre" y="195.6" class="st9" x="8"># 启用所有事件类型的`history_long`表</tspan>
            <tspan style="white-space:pre" class="st10" x="226"> </tspan>
            <tspan style="white-space:pre" y="217.6" class="st11" x="8">CALL</tspan>
            <tspan style="white-space:pre" class="st12" x="37"> sys.ps_setup_enable_consumer(</tspan>
            <tspan style="white-space:pre" class="st13" x="227">'history_long'</tspan>
            <tspan style="white-space:pre" class="st12" x="304">);</tspan>
        </text>
    </g>
    <g ed:width="42" ed:parentid="128" ed:layout="rightmap" transform="matrix(1,0,0,1,719.28,457.08)" id="490" ed:height="20.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,20.6L42,20.6"></path>
        <text class="st4">
            <tspan style="white-space:pre" y="14.6" x="8">关闭</tspan>
        </text>
    </g>
    <g ed:width="266.203125" ed:parentid="490" ed:layout="rightmap" transform="matrix(1,0,0,1,794.28,454.58)" id="492" ed:height="25.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,25.6L266.2,25.6"></path>
        <text class="st4">
            <tspan style="white-space:pre" y="19.6" class="st11" x="8">CALL</tspan>
            <tspan style="white-space:pre" class="st12" x="37"> sys.ps_setup_reset_to_default(</tspan>
            <tspan style="white-space:pre" class="st14" x="218">TRUE</tspan>
            <tspan style="white-space:pre" class="st12" x="248">);</tspan>
        </text>
    </g>
    <g ed:width="142.9739550352097" ed:parentid="101" ed:layout="rightmap" transform="matrix(1,0,0,1,326.31,1454.62)" id="915" ed:height="90.33333003520966">
        <path fill="#f6f6f6" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M4,0L139,0C141.7,0,143,1.3,143,4L143,86.3C143,89,141.7,90.3,139,90.3L4,90.3C1.3,90.3,0,89,0,86.3L0,4C0,1.3,1.3,0,4,0z"></path>
        <g transform="translate(47.99,8.27)">
            <path fill="#ededed" d="M1.5,0C1.5,0,2,0.7,1.8,0.9C1.5,1,1.5,1.8,1.5,1.3C1.5,1.3,1.4,2.3,1.1,2.9C0.8,3.5,1.1,3.9,0.5,4C-0.1,4.1,0,3.6,0,3.4C0,3,0.4,2.4,0.4,2C0.3,1.6,0.4,1.3,0.8,1C1.1,0.7,1.1,0.2,1.5,0z" transform="matrix(1,0,0,1,37.67,33.11)" id="917"></path>
            <path fill="#ffffff" d="M24,0C10.7,0,0,10.7,0,24C0,37.3,10.7,48,24,48C37.3,48,48,37.3,48,24C48,10.7,37.3,0,24,0z" transform="matrix(1,0,0,1,0,0)" id="918"></path>
            <path fill="#4c4c4c" d="M22.7,1.3C34.6,1.3,44.2,10.9,44.2,22.7C44.2,34.6,34.6,44.2,22.7,44.2C10.9,44.2,1.3,34.6,1.3,22.7C1.3,10.9,10.9,1.3,22.7,1.3zM22.7,0C10.3,0,0,10.3,0,22.7C0,35.2,10.3,45.5,22.7,45.5C35.2,45.5,45.5,35.2,45.5,22.7C45.5,10.3,35.3,0,22.7,0z" transform="matrix(1,0,0,1,1.26,1.26)" id="919"></path>
            <path fill="#ededed" d="M0.8,0.3C0.7,0.3,0.8,0.2,0.4,0.4C0.2,0.6,0.1,0.6,0.1,0.7C0,0.8,0.2,1,0.1,1.1C0,1.2,0,1.5,0.1,1.5C0.2,1.5,0.4,1.6,0.4,1.7C0.4,1.8,0.8,2,0.8,2.1C0.8,2.2,1.1,2.4,1.1,2.4C1.1,2.4,1.1,2.4,1.1,2.5C1.1,2.6,1.4,2.8,1.4,2.8C1.4,3.2,1.8,3.6,1.9,3.6C2.3,3.6,2.6,3.6,2.7,3.6C2.8,3.6,2.8,3.6,2.9,3.3C2.9,3.2,2.8,3,2.6,2.7C2.4,2.6,2.2,2.3,2.2,2.3C2.4,2,2.2,1.6,2.2,1.6C1.6,1.5,1.2,1.2,1.1,1.2C1,1.2,1,0.8,1,0.8C1.5,0.2,1.3,0.2,1.2,0.1C1.2,-0.1,1.1,0,0.8,0.3z" transform="matrix(1,0,0,1,29.92,12.62)" id="920"></path>
            <path fill="#69d9e2" d="M30.9,19.9C30.9,19.9,30.5,18.3,30.9,18.6C31.1,19,31.2,20.2,31.2,19.9C31.2,19.6,31.3,19.2,31.3,18.6C31.3,18.1,30.7,15.8,30.7,15.6C30.7,15.4,31.1,15.9,31,15.5C30.7,14,30.2,12.8,28.9,10.4C27.7,8.3,26,6,23.2,3.9C21.7,2.9,19.9,1.8,18.7,1.3C15.1,-0.2,12.5,-0.2,12.8,0.2C12.8,0.2,13.2,0.3,13.3,0.2C13.4,0.1,13.3,0.5,13.7,0.6C14.3,0.6,14.4,1.1,13.9,0.9C13.4,0.8,12.8,0.7,12.9,1C13.2,1.2,13.9,1.4,13.9,1.4C13.9,1.4,11.4,1.5,11.5,0.4C11.5,0,10.8,0,11,0.4C11.1,0.6,11.5,1.2,12.1,1.5C12,1.7,12,1.8,11.7,2.2C11,3.2,10.7,2.6,10,2.3C9.4,2,7.9,2.9,7,3.3C6.2,3.7,6,4.3,5.9,4.6C5.7,5.1,4.5,6.4,4.2,6.7C4,7.3,4.2,7.4,4.6,7.7C5.1,7.9,5.5,6.7,5.6,6.8C5.7,7,5.9,7.2,6.1,7.4C6,7.6,6.1,7.8,6.2,7.9C6.2,8,6.1,8.2,6.1,8.3C6,8.3,6,8.3,6,8.4C5.9,8.3,5.8,7.9,5.5,8C4.5,8.3,5.4,9.3,4.7,9.6C3.9,10,4.4,10.2,3.8,10.5C3.3,10.9,3.1,11.6,2.4,11.9C1.7,12,1.5,12.6,2,12.6C2.5,12.6,2.5,12.7,2.9,13.1C3.1,13.5,1.8,14,1.7,14.1C1.5,14.2,0.7,14.8,1,15.6C1.1,16.2,0.2,16.1,1.4,16.6C1.8,16.7,2.1,16.8,2.3,16.8C2.3,16.8,2.3,16.9,2.3,16.9C2.3,17.6,1.4,18.1,1.3,18.6C1.2,19.1,1.7,19.1,1.3,19.5C0.8,19.7,0.7,20.4,0.3,21.1C0,21.9,-0.1,22.4,0.2,23C0.7,23.6,0.6,24.5,0.6,24.9C0.6,25.2,0.2,25.4,1.2,25.9C2.5,26.3,1.9,26.8,2.9,27.2C3.9,27.5,3.5,27.9,4.5,27.5C5.5,26.9,5.8,27.4,6.1,27.2C6.4,26.9,7.8,25.7,8.1,26.2C8.4,26.6,8.4,26.6,8.8,26.3C9.3,26,9.7,26.9,9.8,27.3C9.8,27.5,9.8,28.1,10.2,28.4C10.4,28.6,10.9,29,11.3,29.4C11.7,30,12,30.6,12,31C11.9,32,12.1,32.9,12.4,33.3C12.5,33.5,12.6,33.5,12.9,33.9C13.1,34.1,13.1,34.3,13.2,34.4C13.2,34.5,13.4,35.1,13.7,35.5C14,35.8,13.9,34.8,14.3,35.8C14.7,36.9,15,36.8,15,36.8C15,36.8,15.5,36.4,15.9,36.4C16.2,36.4,16.8,36.5,17.2,35.4C17.7,34.3,17.7,33.9,17.7,33.9C18.3,33.2,18.1,32.4,18.1,32.2C18.1,32,18.5,31.5,19,31.2C19.4,30.8,19.3,30.4,19.3,29.8C19.3,29.3,19.3,29.2,19,28.8C18.6,28.5,18.7,28.2,18.7,27.5C18.7,26.9,19.4,26.2,19.7,25.8C20,25.4,20.3,24.9,20.7,24.4C21,23.8,21.1,23.7,21.3,23.4C21.6,23.2,21.4,22.6,21.7,21.9C22,21.3,21.7,21,21.7,21C20.8,21.3,20.5,21.5,20.2,21.8C19.3,22.2,19.3,21.8,19.4,21.6C19.7,21.4,20.1,21.2,20.3,21C20.7,20.8,20.7,20.4,21.6,19.5C22.4,18.6,22.7,17.4,22,17.2C21.3,17,21.7,16.4,21.3,16.8C20.7,17.4,19.8,17,19.4,16.9C18.8,16.6,18.5,15.9,18.5,15.9C18.9,15.3,19.5,16.1,19.9,16.4C20.3,16.6,20.5,16.5,20.8,16.4C20.9,16.1,21,16.1,21.2,16.1C21.4,16.1,21.2,16.3,21.9,16.6C22.5,16.8,23.9,15.8,23.9,16.2C23.9,16.6,24.3,16.6,24.3,16.7C24.4,16.7,24.4,16.8,24.8,17C25.3,17.4,25.4,17.3,25.7,17.3C25.8,17.3,25.9,17.4,25.9,17.8C25.9,18.2,26.9,20,26.9,20C27.3,20.3,27.2,21.4,27.7,21C28.2,20.8,27.5,19.4,27.9,18.4C28,17.8,28.3,16.9,28.4,16C28.7,15,29,15.7,29,15.7C28.9,15.7,29.1,15.9,29.5,16.7C29.9,17.4,30,17.3,30.2,17.7C30.4,18,30.2,18.7,30.3,19.7C30.3,19.9,30.7,21.3,30.7,21.7C30.7,22,30,21,30.3,21.7C31,23,30.4,25.4,30.7,25.3C31.1,25.3,31.1,23,31.1,23C31.1,23.9,31.2,22.7,31.2,22.3C31.2,21.9,30.9,19.9,30.9,19.9zM18.1,20.2C18,20.1,17.9,20,17.9,19.9C18,19.9,18,20,18.1,20.2zM12.9,15.8C12.7,16.1,12.3,15.9,11.8,15.8C11.3,15.7,11,15.7,10.7,16.5C10.6,17.1,9.3,16.5,8.9,16.5C8.5,16.5,7.8,16.2,7.4,16.1C7.1,16,7.4,15.7,7.4,15.6C7.4,15.6,7.3,15.2,7.1,15.2C6.7,15.2,6.1,15.4,5.7,15.5C5.2,15.5,4.8,15.5,4.4,16C4.1,16.5,3.4,16.5,3,16.9C2.8,17.1,2.5,16.8,2.4,16.7C3.1,16.7,3.4,16.3,3.5,16C3.6,15.8,3.4,15.4,3.9,15C4.3,14.7,4.2,13.7,4.6,13.7C4.9,13.7,5.5,13.5,5.5,13.4C5.5,13.3,6,12.7,6.4,13.2C6.9,13.7,5.5,13.5,7.4,13.8C9.4,14,8.4,14.5,8,14.8C7.7,14.9,6.2,14.8,7.7,15.3C9,15.8,8.2,14.9,8.1,14.9C7.9,14.9,8.2,14.9,8.6,14.5C9,14.2,8.2,13.8,8.6,13.8C9,13.8,9,13.6,8.9,13.5C8.8,13.4,8.4,13.3,7.8,12.9C7.2,12.6,7.2,12.2,7.3,11.9C7.4,11.6,7.8,11.9,7.8,12.3C7.8,12.5,8.6,12.5,9.2,12.9C9.6,13.1,9.3,13.6,9.7,13.4C10.1,13.3,9.8,13.9,10.7,14.4C11.5,14.8,10.5,13.6,10.4,13.5C10.3,13.4,10.8,12.7,11.4,13C12.2,13.1,11.7,13.6,12.4,14C13.2,14.4,13.3,13.5,13.8,13.7C14.3,13.8,14.1,13.7,14.4,13.4C14.7,13.1,15.1,13.1,15.1,13.5C15.1,13.7,15.3,15.5,14.6,15.5C14,15.5,13.3,15.6,12.9,15.8zM17.8,33.9C17.8,33.9,17.8,33.9,17.8,33.9C17.8,33.9,17.8,33.9,17.8,33.9zM17.7,34C17.7,34,17.7,34,17.7,34z" transform="matrix(1,0,0,1,13.79,2.95)" id="921"></path>
            <path fill="#69d9e2" d="M8.6,9.4C8.6,8.8,8.7,8.2,8,7.5C7.4,6.7,7.3,6.1,7.3,6.1C7.3,4.1,5.4,3.8,5,3.7C4.7,3.6,4.5,4.1,4.3,3.7C4.1,3.6,3,3.3,3,3.3C2,1.6,1.5,1.4,1.4,1.3C1.2,1.1,0.6,0.5,0.5,0.4C0,0,0.3,0,0,0C4.1,10.6,13.3,12.9,13.7,13.1C13.6,13.1,12.3,12.4,11.3,11.8C10.3,11.1,8.6,9.7,8.6,9.4z" transform="matrix(1,0,0,1,3.9,30.95)" id="922"></path>
            <path fill="#69d9e2" d="M2.9,1.7C2.9,1.7,2.7,1.4,2.6,1.2C2.3,0.9,2.5,1.2,2.3,0.9C2.2,0.5,1.8,0.1,1.9,0.1C1.9,0.1,1.8,0.1,1.8,0.1C1.8,0.1,1.6,-0.1,1.5,0.1C1.2,0.4,1.1,0.2,1.1,0.5C1.1,0.9,1.1,0.9,1.1,1C1.1,1.1,0.7,1.3,0.7,1.3C0.6,1.4,0.4,1.7,0.4,1.7C0.4,1.7,0,1.8,0,2.1C0.3,2.3,0.4,2.3,0.4,2.3C0.5,2.3,0,2.8,0,3C0.2,3.1,0.4,3.6,0.4,3.4C1.3,3.4,1.4,3.1,1.4,3C1.4,2.7,1.4,2.6,1.6,2.4C1.9,2.1,2,2,2,2C2,2,2.3,2.1,2,2.5C1.7,2.9,1.6,3,1.6,3C2.1,3.1,2.5,2.8,2.5,3C2.5,3.1,2.8,2.7,2.8,2.7C2.9,2.7,3.2,2.3,3.2,2.3C3.2,2.3,3.1,2.2,3.2,2.1C3.3,2,2.9,1.7,2.9,1.7z" transform="matrix(1,0,0,1,13.26,10.94)" id="923"></path>
            <path fill="#69d9e2" d="M0.3,9.1C0.6,8.8,0.9,7.4,1.9,6.7C2.7,6.1,2.6,5.9,3.4,5.4C3.8,5.2,4,5.1,4.9,4.4C5.7,3.9,5.6,3.8,5.6,3.7C5.6,3.6,5.7,3.3,6,3.3C6.3,3.3,6.9,2.8,7,2.6C7.1,2.5,7.1,2.3,7.4,2.2C7.7,2.1,7.8,2.1,8.1,1.7C8.2,1.5,8.3,1.4,8.7,1.4C9.1,1.4,9.1,1.2,9.4,0.9C9.7,0.6,11.4,0,11.4,0C9.3,0.2,4.9,2.5,4.9,2.6C4.8,2.9,2.5,4.8,2.2,5.2C1.6,5.7,0.6,7.1,0.3,7.7C-0.1,8.6,-0.1,9.4,0.3,9.1z" transform="matrix(1,0,0,1,7.72,3.05)" id="927"></path>
            <path fill="#69d9e2" d="M0.9,5.2C0.8,5.6,0.6,6.2,0.6,6.5C0.6,6.8,1.1,6.6,1.1,6.1C1.1,5.7,1.4,4.8,1.4,4.8C1.4,4.8,1.5,4.5,1.7,4.1C1.9,3.7,1.7,2.9,1.8,2.6C2.1,2,2.5,1.3,2.5,1.3C2.5,1.3,3.2,-0.1,3,0C0.4,3.6,0,7.3,0,7.3C0,7.3,0.2,7.7,0.3,7.6L0.6,5.9C0.6,5.8,0.8,5.5,0.9,5.2z" transform="matrix(1,0,0,1,3.32,11.52)" id="930"></path>
        </g>
        <text class="st3">
            <tspan style="white-space:pre" y="79.2" class="st2" x="17">SYS系统库的表</tspan>
        </text>
    </g>
    <g ed:width="282" ed:parentid="411" ed:layout="rightmap" transform="matrix(1,0,0,1,1015.77,19.33)" id="1070" ed:height="20.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,20.6L282,20.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="14.6" x="8">将性能模式数据汇总为更易于理解的形式的视图。</tspan>
        </text>
    </g>
    <g ed:width="363.53125" ed:parentid="411" ed:layout="rightmap" transform="matrix(1,0,0,1,1015.77,44.92)" id="1071" ed:height="20.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,20.6L363.5,20.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="14.6" x="8">执行诸如“性能模式”配置和生成诊断报告之类的操作的存储过程。</tspan>
        </text>
    </g>
    <g ed:width="270" ed:parentid="411" ed:layout="rightmap" transform="matrix(1,0,0,1,1015.77,70.5)" id="1072" ed:height="20.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,20.6L270,20.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="14.6" x="8">查询性能架构配置并提供格式服务的存储函数。</tspan>
        </text>
    </g>
    <g ed:width="140.515625" ed:parentid="122" ed:layout="rightmap" transform="matrix(1,0,0,1,707.28,487.67)" id="1076" ed:height="20.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,20.6L140.5,20.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="14.6" x="8">实践1-查看数据库版本</tspan>
        </text>
    </g>
    <g ed:width="164.515625" ed:parentid="122" ed:layout="rightmap" transform="matrix(1,0,0,1,707.28,515.75)" id="1078" ed:height="20.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,20.6L164.5,20.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="14.6" x="8">实践2-视图数值展示的区别</tspan>
        </text>
    </g>
    <g ed:width="173.25" ed:parentid="122" ed:layout="rightmap" transform="matrix(1,0,0,1,707.28,541.33)" id="1080" ed:height="20.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,20.6L173.3,20.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="14.6" x="8">实践3-查看SYS库的对象DDL</tspan>
        </text>
    </g>
    <g ed:width="136.890625" ed:parentid="122" ed:layout="rightmap" transform="matrix(1,0,0,1,707.28,597.71)" id="1082" ed:height="20.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,20.6L136.9,20.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="14.6" x="8">实践4-导出导入SYS库</tspan>
        </text>
    </g>
    <g ed:width="178.765625" ed:parentid="1076" ed:layout="rightmap" transform="matrix(1,0,0,1,880.8,485.17)" id="1088" ed:height="25.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,25.6L178.8,25.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="19.6" class="st12" x="8">SELECT * FROM sys.version;</tspan>
        </text>
    </g>
    <g ed:width="42" ed:parentid="391" ed:layout="rightmap" transform="matrix(1,0,0,1,608.28,109.37)" id="1090" ed:height="21.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,21.6L42,21.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="15.6" class="st15" x="8">视图</tspan>
        </text>
    </g>
    <g ed:width="40.078125" ed:parentid="1090" ed:layout="rightmap" transform="matrix(1,0,0,1,683.28,97.58)" id="1094" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L40.1,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">xxx</tspan>
        </text>
    </g>
    <g ed:width="55.015625" ed:parentid="1090" ed:layout="rightmap" transform="matrix(1,0,0,1,683.28,124.17)" id="1096" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L55,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">x$xxx</tspan>
        </text>
    </g>
    <g ed:width="378" ed:parentid="1094" ed:layout="rightmap" transform="matrix(1,0,0,1,756.36,96.08)" id="1098" ed:height="21.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,21.6L378,21.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="15.6" class="st15" x="8">相关数值数据转化为人性化显示，例如毫秒、秒、分钟、小时、天等</tspan>
        </text>
    </g>
    <g ed:width="180.1875" ed:parentid="1096" ed:layout="rightmap" transform="matrix(1,0,0,1,771.3,122.67)" id="1100" ed:height="21.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,21.6L180.2,21.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="15.6" class="st15" x="8">相关数值保持原始的数据(皮秒)</tspan>
        </text>
    </g>
    <g ed:width="149.328125" ed:parentid="122" ed:layout="rightmap" transform="matrix(1,0,0,1,707.28,691.08)" id="1102" ed:height="21.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,21.6L149.3,21.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="15.6" class="st15" x="8">实</tspan>
            <tspan style="white-space:pre" class="st15" x="20">践5-查看语句进度信息</tspan>
        </text>
    </g>
    <g ed:width="505.3125" ed:parentid="1102" ed:layout="rightmap" transform="matrix(1,0,0,1,889.61,654.08)" id="1104" ed:height="95.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,95.6L505.3,95.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="21.6" class="st9" x="10"># 查看当前正在执行的语句进度信息</tspan>
            <tspan style="white-space:pre" class="st10" x="201"> </tspan>
            <tspan style="white-space:pre" y="43.6" class="st11" x="10">select</tspan>
            <tspan style="white-space:pre" class="st12" x="44"> * </tspan>
            <tspan style="white-space:pre" class="st11" x="57">from</tspan>
            <tspan style="white-space:pre" class="st12" x="85"> </tspan>
            <tspan style="white-space:pre" class="st11" x="89">session</tspan>
            <tspan style="white-space:pre" class="st12" x="132"> </tspan>
            <tspan style="white-space:pre" class="st11" x="136">where</tspan>
            <tspan style="white-space:pre" class="st12" x="171"> conn_id!=connection_id() </tspan>
            <tspan style="white-space:pre" class="st11" x="326">and</tspan>
            <tspan style="white-space:pre" class="st12" x="348"> trx_state=</tspan>
            <tspan style="white-space:pre" class="st13" x="412">'ACTIVE'</tspan>
            <tspan style="white-space:pre" class="st12" x="459">;</tspan>
            <tspan style="white-space:pre" class="st10" x="463"> </tspan>
            <tspan style="white-space:pre" y="65.6" class="st9" x="10"># 查看已经执行完的语句相关统计信息</tspan>
            <tspan style="white-space:pre" class="st10" x="213"> </tspan>
            <tspan style="white-space:pre" y="87.6" class="st11" x="10">select</tspan>
            <tspan style="white-space:pre" class="st12" x="44"> * </tspan>
            <tspan style="white-space:pre" class="st11" x="57">from</tspan>
            <tspan style="white-space:pre" class="st12" x="85"> </tspan>
            <tspan style="white-space:pre" class="st11" x="89">session</tspan>
            <tspan style="white-space:pre" class="st12" x="132"> </tspan>
            <tspan style="white-space:pre" class="st11" x="136">where</tspan>
            <tspan style="white-space:pre" class="st12" x="171"> conn_id!=connection_id() </tspan>
            <tspan style="white-space:pre" class="st11" x="326">and</tspan>
            <tspan style="white-space:pre" class="st12" x="348"> trx_state=</tspan>
            <tspan style="white-space:pre" class="st13" x="412">'COMMITTED'</tspan>
            <tspan style="white-space:pre" class="st12" x="490">;</tspan>
        </text>
    </g>
    <g ed:width="342.640625" ed:parentid="1082" ed:layout="rightmap" transform="matrix(1,0,0,1,877.17,566.92)" id="1106" ed:height="47.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,47.6L342.6,47.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="19.6" class="st12" x="8">mysqldump --databases --routines sys&gt; sys_dump.sql</tspan>
            <tspan style="white-space:pre" class="st10" x="333"> </tspan>
            <tspan style="white-space:pre" y="41.6" class="st12" x="8">mysqlpump sys&gt; sys_dump.sql</tspan>
        </text>
    </g>
    <g ed:width="152.859375" ed:parentid="1082" ed:layout="rightmap" transform="matrix(1,0,0,1,877.17,619.5)" id="1108" ed:height="29.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,29.6L152.9,29.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="21.6" class="st12" x="10">mysql &lt; sys_dump.sql</tspan>
        </text>
    </g>
    <g ed:width="66" ed:parentid="164" ed:layout="rightmap" transform="matrix(1,0,0,1,770.19,981.08)" id="1110" ed:height="20.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,20.6L66,20.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="14.6" x="8">修改配置</tspan>
        </text>
    </g>
    <g ed:width="514" ed:parentid="1110" ed:layout="rightmap" transform="matrix(1,0,0,1,869.19,899.58)" id="1112" ed:height="183.5833325088024">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,183.6L514,183.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="21.6" x="10"># 如果所有会话都需要使用，则可以将DEBUG选项INSERT到SYS_CONFIG表中 </tspan>
            <tspan style="white-space:pre" y="43.6" x="10">MYSQL&gt; INSERT INTO SYS_CONFIG (VARIABLE, VALUE) VALUES('DEBUG', 'ON'); </tspan>
            <tspan style="white-space:pre" y="65.6" x="10"># 要更改表中的调试配置选项值，可以使用UPDATE语句更新该配置选项值 </tspan>
            <tspan style="white-space:pre" y="87.6" x="10">## 首先，修改表中的值： </tspan>
            <tspan style="white-space:pre" y="109.6" x="10">MYSQL&gt; UPDATE SYS_CONFIG SET VALUE = 'OFF' WHERE VARIABLE = 'DEBUG'; </tspan>
            <tspan style="white-space:pre" y="131.6" x="10">## 然后，为了确保当前会话中的存储过程调用时使用表中的更改后的值，需要将相应的用户</tspan>
            <tspan style="white-space:pre" y="153.6" x="10">定义的变量设置为NULL </tspan>
            <tspan style="white-space:pre" y="175.6" x="10">MYSQL&gt; SET @SYS.DEBUG = NULL;</tspan>
        </text>
    </g>
    <g ed:width="157.1875" ed:parentid="1148" ed:layout="rightmap" transform="matrix(1,0,0,1,758.19,771.67)" id="1114" ed:height="20.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,20.6L157.2,20.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="14.6" x="8">VARIABLE：配置选项名称</tspan>
        </text>
    </g>
    <g ed:width="127.3125" ed:parentid="1148" ed:layout="rightmap" transform="matrix(1,0,0,1,758.19,848.42)" id="1116" ed:height="20.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,20.6L127.3,20.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="14.6" x="8">VALUE：配置选项值</tspan>
        </text>
    </g>
    <g ed:width="204.390625" ed:parentid="1148" ed:layout="rightmap" transform="matrix(1,0,0,1,758.19,797.25)" id="1118" ed:height="20.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,20.6L204.4,20.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="14.6" x="8">SET_TIME：该行配置最近修改时间</tspan>
        </text>
    </g>
    <g ed:width="286.890625" ed:parentid="1148" ed:layout="rightmap" transform="matrix(1,0,0,1,758.19,822.83)" id="1120" ed:height="20.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,20.6L286.9,20.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="14.6" x="8">SET_BY：最近一次对改行配置进行修改的帐户名。</tspan>
        </text>
    </g>
    <g ed:width="66" ed:parentid="164" ed:layout="rightmap" transform="matrix(1,0,0,1,770.19,874)" id="1136" ed:height="20.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,20.6L66,20.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="14.6" x="8">查看配置</tspan>
        </text>
    </g>
    <g ed:width="161.015625" ed:parentid="1136" ed:layout="rightmap" transform="matrix(1,0,0,1,869.19,875)" id="1138" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L161,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">select * from sys_config;</tspan>
        </text>
    </g>
    <g ed:width="73" ed:parentid="156" ed:layout="rightmap" transform="matrix(1,0,0,1,652.19,809.04)" id="1148" ed:height="22.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,22.6L73,22.6"></path>
        <g transform="translate(7.04,2.21)">
            <use xlink:href="#imgstar5" transform="translate(0,0)"></use>
        </g>
        <text class="st6">
            <tspan style="white-space:pre" y="15.6" x="27">表结构</tspan>
        </text>
    </g>
    <g ed:width="252.1875" ed:parentid="104" ed:layout="rightmap" transform="matrix(1,0,0,1,502.28,1088.17)" id="1155" ed:height="22.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,22.6L252.2,22.6"></path>
        <g transform="translate(7.04,2.21)">
            <use xlink:href="#imgstar4" transform="translate(0,0)"></use>
        </g>
        <text class="st6">
            <tspan style="white-space:pre" y="15.6" x="27">SYS_CONFIG_INSERT_SET_USER触发器</tspan>
        </text>
    </g>
    <g ed:width="258.03125" ed:parentid="104" ed:layout="rightmap" transform="matrix(1,0,0,1,502.28,1115.75)" id="1157" ed:height="22.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,22.6L258,22.6"></path>
        <g transform="translate(7.04,2.21)">
            <use xlink:href="#imgstar4" transform="translate(0,0)"></use>
        </g>
        <text class="st6">
            <tspan style="white-space:pre" y="15.6" x="27">SYS_CONFIG_UPDATE_SET_USER触发器</tspan>
        </text>
    </g>
    <g ed:width="328.046875" ed:parentid="116" ed:layout="rightmap" transform="matrix(1,0,0,1,670.27,1160.33)" id="1161" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L328,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">host_summary_by_file_io,x$host_summary_by_file_io</tspan>
        </text>
    </g>
    <g ed:width="210.8125" ed:parentid="116" ed:layout="rightmap" transform="matrix(1,0,0,1,670.27,1183.92)" id="1163" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L210.8,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">host_summary,x$ host_summary</tspan>
        </text>
    </g>
    <g ed:width="389.984375" ed:parentid="116" ed:layout="rightmap" transform="matrix(1,0,0,1,670.27,1207.5)" id="1165" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L390,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">host_summary_by_file_io_type,x$host_summary_by_file_io_type</tspan>
        </text>
    </g>
    <g ed:width="333.578125" ed:parentid="116" ed:layout="rightmap" transform="matrix(1,0,0,1,670.27,1231.08)" id="1167" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L333.6,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">host_summary_by_stages,x$host_summary_by_stages</tspan>
        </text>
    </g>
    <g ed:width="469.171875" ed:parentid="116" ed:layout="rightmap" transform="matrix(1,0,0,1,670.27,1254.67)" id="1169" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L469.2,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">host_summary_by_statement_latency,x$host_summary_by_statement_latency</tspan>
        </text>
    </g>
    <g ed:width="436.921875" ed:parentid="116" ed:layout="rightmap" transform="matrix(1,0,0,1,670.27,1278.25)" id="1171" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L436.9,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">host_summary_by_statement_type,x$host_summary_by_statement_type</tspan>
        </text>
    </g>
    <g ed:width="327.515625" ed:parentid="114" ed:layout="rightmap" transform="matrix(1,0,0,1,670,1301.83)" id="1173" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L327.5,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">user_summary_by_file_io,x$user_summary_by_file_io</tspan>
        </text>
    </g>
    <g ed:width="389.453125" ed:parentid="114" ed:layout="rightmap" transform="matrix(1,0,0,1,670,1325.42)" id="1175" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L389.5,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">user_summary_by_file_io_type,x$user_summary_by_file_io_type</tspan>
        </text>
    </g>
    <g ed:width="333.046875" ed:parentid="114" ed:layout="rightmap" transform="matrix(1,0,0,1,670,1349)" id="1177" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L333,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">user_summary_by_stages,x$user_summary_by_stages</tspan>
        </text>
    </g>
    <g ed:width="468.640625" ed:parentid="114" ed:layout="rightmap" transform="matrix(1,0,0,1,670,1372.58)" id="1179" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L468.6,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">user_summary_by_statement_latency,x$user_summary_by_statement_latency</tspan>
        </text>
    </g>
    <g ed:width="436.390625" ed:parentid="114" ed:layout="rightmap" transform="matrix(1,0,0,1,670,1396.17)" id="1181" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L436.4,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">user_summary_by_statement_type,x$user_summary_by_statement_type</tspan>
        </text>
    </g>
    <g ed:width="319.171875" ed:parentid="106" ed:layout="rightmap" transform="matrix(1,0,0,1,653.59,1419.75)" id="1183" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L319.2,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">io_by_thread_by_latency,x$io_by_thread_by_latency</tspan>
        </text>
    </g>
    <g ed:width="343.421875" ed:parentid="106" ed:layout="rightmap" transform="matrix(1,0,0,1,653.59,1443.33)" id="1185" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L343.4,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">io_global_by_file_by_bytes,x$io_global_by_file_by_bytes</tspan>
        </text>
    </g>
    <g ed:width="363.453125" ed:parentid="106" ed:layout="rightmap" transform="matrix(1,0,0,1,653.59,1466.92)" id="1187" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L363.5,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">io_global_by_file_by_latency,x$io_global_by_file_by_latency</tspan>
        </text>
    </g>
    <g ed:width="355.296875" ed:parentid="106" ed:layout="rightmap" transform="matrix(1,0,0,1,653.59,1490.5)" id="1189" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L355.3,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">io_global_by_wait_by_bytes,x$io_global_by_wait_by_bytes</tspan>
        </text>
    </g>
    <g ed:width="375.328125" ed:parentid="106" ed:layout="rightmap" transform="matrix(1,0,0,1,653.59,1514.08)" id="1191" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L375.3,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">io_global_by_wait_by_latency,x$io_global_by_wait_by_latency</tspan>
        </text>
    </g>
    <g ed:width="181.828125" ed:parentid="106" ed:layout="rightmap" transform="matrix(1,0,0,1,653.59,1537.67)" id="1193" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L181.8,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">latest_file_io,x$latest_file_io</tspan>
        </text>
    </g>
    <g ed:width="407.984375" ed:parentid="108" ed:layout="rightmap" transform="matrix(1,0,0,1,641.28,1561.25)" id="1195" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L408,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">innodb_buffer_stats_by_schema,x$innodb_buffer_stats_by_schema</tspan>
        </text>
    </g>
    <g ed:width="377.203125" ed:parentid="108" ed:layout="rightmap" transform="matrix(1,0,0,1,641.28,1584.83)" id="1197" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L377.2,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">innodb_buffer_stats_by_table,x$innodb_buffer_stats_by_table</tspan>
        </text>
    </g>
    <g ed:width="444.109375" ed:parentid="108" ed:layout="rightmap" transform="matrix(1,0,0,1,641.28,1608.42)" id="1199" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L444.1,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">memory_by_host_by_current_bytes,x$memory_by_host_by_current_bytes</tspan>
        </text>
    </g>
    <g ed:width="468.671875" ed:parentid="108" ed:layout="rightmap" transform="matrix(1,0,0,1,641.28,1632)" id="1201" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L468.7,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">memory_by_thread_by_current_bytes,x$memory_by_thread_by_current_bytes</tspan>
        </text>
    </g>
    <g ed:width="443.578125" ed:parentid="108" ed:layout="rightmap" transform="matrix(1,0,0,1,641.28,1655.58)" id="1203" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L443.6,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">memory_by_user_by_current_bytes,x$memory_by_user_by_current_bytes</tspan>
        </text>
    </g>
    <g ed:width="425.578125" ed:parentid="108" ed:layout="rightmap" transform="matrix(1,0,0,1,641.28,1679.17)" id="1205" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L425.6,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">memory_global_by_current_bytes,x$memory_global_by_current_bytes</tspan>
        </text>
    </g>
    <g ed:width="280.671875" ed:parentid="108" ed:layout="rightmap" transform="matrix(1,0,0,1,641.28,1702.75)" id="1207" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L280.7,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">memory_global_total,x$memory_global_total</tspan>
        </text>
    </g>
    <g ed:width="114" ed:parentid="915" ed:layout="rightmap" transform="matrix(1,0,0,1,502.28,1745)" id="1216" ed:height="75.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,75.6L114,75.6"></path>
        <g transform="translate(33.56,1.83)">
            <path fill="#ffffff" d="M46.7,24.6C45.5,13.6,36.9,5.1,25.8,4.2L25.8,1.2L25.8,0L24.6,0L23.2,0L22,0L22,1.2L22,4.3C11,5,1.9,13.4,0.6,24.3C0.3,24.7,0,25.2,0,25.8C0,26.9,1,27.8,2.1,27.8C3,27.8,3.7,27.4,4.1,26.6L13.3,26.6C13.6,27.2,14.4,27.7,15.2,27.7C15.9,27.7,16.5,27.3,16.9,26.7L21.9,26.7L21.9,42.3C21.9,43.3,22.1,44.4,22.6,45.3C23.6,47,25.5,48,27.6,48C28.2,48,28.8,47.9,29.4,47.7L31.8,47.1L33,46.8L32.7,45.5L32.3,44.3L31.9,43L30.7,43.4L28.2,44.1C28,44.2,27.8,44.2,27.5,44.2C26.8,44.2,26,43.8,25.8,43.3C25.5,43,25.5,42.6,25.5,42.2L25.5,26.4L30.9,26.4C31.1,27.3,31.9,27.9,32.8,27.9C33.8,27.9,34.6,27.3,34.8,26.4L42.9,26.4C43.2,27.2,44,27.7,44.9,27.7C46,27.7,47,26.7,47,25.6C47,25.2,46.9,24.9,46.7,24.6z" transform="matrix(1,0,0,1,-0,0)" id="1241"></path>
            <path fill="#69d9e2" d="M0,18.9C0.1,15.9,0.9,3.1,8.7,0C10,0.7,16.9,5.2,17.7,18.8L0,18.9z" transform="matrix(1,0,0,1,15.01,5.92)" id="1242"></path>
            <path fill="#4c4c4c" d="M44.3,23.7C43.2,12.6,33.9,4.1,22.6,4.1L23.4,0L22,0L22,4.1C10.9,4.1,1.6,12.5,0.5,23.6C0.2,23.7,0,24,0,24.4C0,24.8,0.4,25.2,0.9,25.2C1.4,25.2,1.8,24.8,1.8,24.4C1.8,24.3,1.8,24.2,1.7,24.2L12.9,24.2C12.9,24.2,12.9,24.2,12.9,24.3C12.9,24.7,13.3,25.1,13.8,25.1C14.3,25.1,14.7,24.7,14.7,24.3C14.7,24.2,14.7,24.2,14.7,24.2L21.9,24.2L21.9,41.2C21.9,42.1,22.1,42.8,22.5,43.5C23.4,44.9,24.8,45.6,26.5,45.6C26.8,45.6,27.3,45.5,27.9,45.5L30.4,44.8L30,43.5L27.6,44.3C25.9,44.8,24.3,44.2,23.6,42.9C23.2,42.4,23.1,41.8,23.1,41.2L23.1,24.2L31,24.2C30.9,24.3,30.8,24.5,30.8,24.6C30.8,25.1,31.2,25.5,31.7,25.5C32.2,25.5,32.6,25.1,32.6,24.6C32.6,24.5,32.5,24.3,32.4,24.2L42.8,24.2C42.8,24.2,42.8,24.3,42.8,24.4C42.8,24.8,43.2,25.2,43.7,25.2C44.1,25.2,44.6,24.8,44.6,24.4C44.6,24.1,44.5,23.8,44.3,23.7zM13.3,22.9L1.8,22.9C3,13.6,10.7,6.3,20,5.4C14.3,9.6,13.4,19.4,13.3,22.9zM23.1,22.9L21.9,22.9L14.6,22.9C14.8,19,15.8,8.3,22.6,5.4C24.2,6.4,30.2,10.7,31,22.9L23.1,22.9zM32.2,22.9C31.5,12.6,27.2,7.5,24.6,5.4C34,6.3,41.7,13.6,43,22.9L32.2,22.9z" transform="matrix(1,0,0,1,1.1,1.25)" id="1243"></path>
        </g>
        <text class="st6">
            <tspan style="white-space:pre" y="68.8" x="8">内存分配统计视图</tspan>
        </text>
    </g>
    <g ed:width="449.109375" ed:parentid="1216" ed:layout="rightmap" transform="matrix(1,0,0,1,649.28,1726.33)" id="1230" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L449.1,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">wait_classes_global_by_avg_latency,x$wait_classes_global_by_avg_latency</tspan>
        </text>
    </g>
    <g ed:width="396.515625" ed:parentid="1216" ed:layout="rightmap" transform="matrix(1,0,0,1,649.28,1749.92)" id="1232" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L396.5,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">wait_classes_global_by_latency,x$wait_classes_global_by_latency</tspan>
        </text>
    </g>
    <g ed:width="332.796875" ed:parentid="1216" ed:layout="rightmap" transform="matrix(1,0,0,1,649.28,1773.5)" id="1234" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L332.8,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">waits_by_host_by_latency,x$waits_by_host_by_latency</tspan>
        </text>
    </g>
    <g ed:width="332.265625" ed:parentid="1216" ed:layout="rightmap" transform="matrix(1,0,0,1,649.28,1797.08)" id="1236" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L332.3,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">waits_by_user_by_latency,x$waits_by_user_by_latency</tspan>
        </text>
    </g>
    <g ed:width="314.265625" ed:parentid="1216" ed:layout="rightmap" transform="matrix(1,0,0,1,649.28,1820.67)" id="1238" ed:height="18.58333250880241">
        <path fill="none" stroke="#696969" stroke-linejoin="round" stroke-width="1.33333" d="M0,18.6L314.3,18.6"></path>
        <text class="st6">
            <tspan style="white-space:pre" y="12.6" x="8">waits_global_by_latency,x$waits_global_by_latency</tspan>
        </text>
    </g>
</svg>
</div>
        <div id="copyright">Created With  <a href="https://www.edrawsoft.com/" target="_blank" title="edrawsoft">MindMaster</a></div>
      </div>
    </div>
<script>
    eval(atob('dmFyIG11YT13aW5kb3cubmF2aWdhdG9yLnVzZXJBZ2VudDsNCnZhciB1YT0obXVhLmluZGV4T2YoJ3J2OjExJykrbXVhLmluZGV4T2YoJ01TSUUnKSk+PTA7DQpkb2N1bWVudC5nZXRFbGVtZW50QnlJZCgnc3ZnLWNvbnRhaW5lcicpLm9uY29udGV4dG1lbnUgPSBmdW5jdGlvbiAoZXZlbnQpIHsNCiAgICBldmVudC5wcmV2ZW50RGVmYXVsdCgpOw0KfQ0KZG9jdW1lbnQuZ2V0RWxlbWVudEJ5SWQoJ3N2Zy1jb250YWluZXInKS5vbm1vdXNlZG93biA9IGZ1bmN0aW9uIChldmVudCkgew0KICAgIGlmKGV2ZW50LndoaWNoID09Myl7DQogICAgICAgIHRoaXMuc3R5bGUuY3Vyc29yID0gJ3BvaW50ZXInOw0KICAgICAgICB0aGlzLm9ubW91c2Vtb3ZlID0gZnVuY3Rpb24gKGV2KSB7DQogICAgICAgICAgICB0aGlzLnNjcm9sbEJ5KC0oZXYubW92ZW1lbnRYKSwgMCk7DQogICAgICAgICAgICB3aW5kb3cuc2Nyb2xsQnkoMCwtKGV2Lm1vdmVtZW50WSkpDQogICAgICAgIH0NCiAgICAgICAgdGhpcy5vbm1vdXNldXAgPSBmdW5jdGlvbiAoKSB7DQogICAgICAgICAgICB0aGlzLnN0eWxlLmN1cnNvciA9ICBudWxsOw0KICAgICAgICAgICAgdGhpcy5vbm1vdXNldXAgPSBudWxsOw0KICAgICAgICAgICAgdGhpcy5vbm1vdXNlbW92ZSA9IG51bGw7DQogICAgICAgIH0NCiAgICB9DQp9DQpOdW1iZXIucHJvdG90eXBlLnRvc3VpdHN2Zz1mdW5jdGlvbiAoKSB7DQogICAgdmFyIG51bT10aGlzLnZhbHVlT2YoKTsNCiAgICBpZihudW0lMT09PTApew0KICAgICAgICByZXR1cm4gbnVtKzAuNQ0KICAgIH1lbHNlIHJldHVybiBudW07DQp9Ow0KTnVtYmVyLnByb3RvdHlwZS5wbHVzej1mdW5jdGlvbigpIHsNCiAgICB2YXIgbnVtPXRoaXMudmFsdWVPZigpOw0KICAgIHJldHVybiBudW08MTA/JzAnK251bTpudW07DQp9Ow0KZnVuY3Rpb24gcGFyc2VEYXRlKG51bSkgew0KICAgIHZhciBkYXRlID0gbmV3IERhdGUobnVtKTsNCiAgICB2YXIgWSA9IGRhdGUuZ2V0RnVsbFllYXIoKSArICctJzsNCiAgICB2YXIgTSA9IChkYXRlLmdldE1vbnRoKCkrMSkucGx1c3ooKSArICctJzsNCiAgICB2YXIgRCA9IGRhdGUuZ2V0RGF0ZSgpLnBsdXN6KCkgKyAnICc7DQogICAgdmFyIGggPSBkYXRlLmdldEhvdXJzKCkucGx1c3ooKSArICc6JzsNCiAgICB2YXIgbW0gPSBkYXRlLmdldE1pbnV0ZXMoKS5wbHVzeigpICsgJzonOw0KICAgIHZhciBzID0gZGF0ZS5nZXRTZWNvbmRzKCkucGx1c3ooKTsNCiAgICByZXR1cm4gWStNK0QraCttbStzOw0KfQ0KLy8tLXByZWRlZmluZWQNCi8vY29tbWVudC0tDQp2YXIgY29tbWVudHM9ZG9jdW1lbnQucXVlcnlTZWxlY3RvckFsbCgnZz5nW2VkXFw6Y29tbWVudF0nKTsNCmZ1bmN0aW9uIGdldGN3aChwb3B1cCkgew0KICAgIGRvY3VtZW50LmJvZHkuZ2V0RWxlbWVudHNCeVRhZ05hbWUoJ3N2ZycpWzBdLmFwcGVuZENoaWxkKHBvcHVwKTsNCiAgICB2YXIgdz1wb3B1cC5nZXRCb3VuZGluZ0NsaWVudFJlY3QoKS53aWR0aDsNCiAgICB2YXIgaD1wb3B1cC5nZXRCb3VuZGluZ0NsaWVudFJlY3QoKS5oZWlnaHQ7DQogICAgcmV0dXJuIFt3LGhdDQp9DQpmb3IodmFyIGk9MDtpPGNvbW1lbnRzLmxlbmd0aDtpKyspew0KICAgIHZhciBwb3B1cD1kb2N1bWVudC5jcmVhdGVFbGVtZW50TlMoJ2h0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnJywnZycpOw0KICAgIHZhciBwb3B1cFI9IGRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdyZWN0Jyk7DQogICAgdmFyIGhvdmVyPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdyZWN0Jyk7DQogICAgdmFyIG9saW5lPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdyZWN0Jyk7DQogICAgaG92ZXIuc2V0QXR0cmlidXRlKCdmaWxsJywnI2NkY2RmZicpOw0KICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgneCcsJzAnKTsNCiAgICBob3Zlci5zZXRBdHRyaWJ1dGUoJ3knLCcwJyk7DQogICAgaG92ZXIuc2V0QXR0cmlidXRlKCdoZWlnaHQnLCcxNicpOw0KICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgnd2lkdGgnLCcxNicpOw0KICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgnZmlsbC1vcGFjaXR5JywnMC42Jyk7DQogICAgaG92ZXIuc2V0QXR0cmlidXRlKCd0cmFuc2Zvcm0nLGNvbW1lbnRzW2ldLnF1ZXJ5U2VsZWN0b3IoJ3VzZScpLmdldEF0dHJpYnV0ZSgndHJhbnNmb3JtJykpOw0KICAgIGhvdmVyLnN0eWxlLmRpc3BsYXk9J25vbmUnOw0KICAgIGNvbW1lbnRzW2ldLmFwcGVuZENoaWxkKGhvdmVyKTsNCiAgICB2YXIgYT1KU09OLnBhcnNlKGNvbW1lbnRzW2ldLmdldEF0dHJpYnV0ZSgnZWQ6Y29tbWVudCcpKTsNCiAgICB2YXIgaGVpZ2h0PTA7DQogICAgdmFyIGNhcnI9W107DQogICAgZm9yKHZhciBqPTA7ajxhLmxlbmd0aDtqKyspew0KICAgICAgICB2YXIgc3RhbXA9TnVtYmVyKGFbal0uRGF0ZSkqMTAwMDsNCiAgICAgICAgdmFyIHRpbWU9cGFyc2VEYXRlKHN0YW1wKTsNCiAgICAgICAgdmFyIG5hbWU9YVtqXS5OYW1lOw0KICAgICAgICB2YXIgbWVzc2FnZT1hW2pdLk1lc3NhZ2U7DQogICAgICAgIHZhciBtZXNzYWdlQXJyPW1lc3NhZ2Uuc3BsaXQoL1xuLyk7DQogICAgICAgIHZhciBvPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdnJyk7DQogICAgICAgIHZhciBuPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCd0ZXh0Jyk7DQogICAgICAgIHZhciB0PWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCd0ZXh0Jyk7DQogICAgICAgIHZhciBtPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCd0ZXh0Jyk7DQogICAgICAgIG4uc2V0QXR0cmlidXRlKCd4Jyw1KTsNCiAgICAgICAgbi5zZXRBdHRyaWJ1dGUoJ3knLDEyKTsNCiAgICAgICAgbi5zZXRBdHRyaWJ1dGUoJ2ZpbGwnLCcjMDA2ZWZmJyk7DQogICAgICAgIG4udGV4dENvbnRlbnQ9bmFtZSsnOiAnOw0KICAgICAgICBuLnNldEF0dHJpYnV0ZSgnZm9udC1zaXplJywnMTInKTsNCiAgICAgICAgdC5zZXRBdHRyaWJ1dGUoJ3gnLDIwMCk7DQogICAgICAgIHQuc2V0QXR0cmlidXRlKCd5JywxMik7DQogICAgICAgIHQuc2V0QXR0cmlidXRlKCdmaWxsJywnIzk2OTY5NicpOw0KICAgICAgICB0LnRleHRDb250ZW50PXRpbWU7DQogICAgICAgIHQuc2V0QXR0cmlidXRlKCdmb250LXNpemUnLCcxMCcpOw0KICAgICAgICBtLnNldEF0dHJpYnV0ZSgndHJhbnNmb3JtJywndHJhbnNsYXRlKDIwLDI3KScpOw0KICAgICAgICBtLnNldEF0dHJpYnV0ZSgnZm9udC1zaXplJywnMTInKTsNCiAgICAgICAgZm9yKHZhciBrPTA7azxtZXNzYWdlQXJyLmxlbmd0aDtrKyspew0KICAgICAgICAgICAgdmFyIHRzPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCd0c3BhbicpOw0KICAgICAgICAgICAgdHMuc2V0QXR0cmlidXRlKCd4JywnMCcpOw0KICAgICAgICAgICAgdHMuc2V0QXR0cmlidXRlKCd5JyxrKjE2KTsNCiAgICAgICAgICAgIHRzLnRleHRDb250ZW50PW1lc3NhZ2VBcnJba107DQogICAgICAgICAgICBtLmFwcGVuZENoaWxkKHRzKTsNCiAgICAgICAgfQ0KICAgICAgICBvLnNldEF0dHJpYnV0ZSgndHJhbnNmb3JtJywndHJhbnNsYXRlKDAsJytoZWlnaHQrJyknKTsNCiAgICAgICAgby5hcHBlbmRDaGlsZChuKTsNCiAgICAgICAgby5hcHBlbmRDaGlsZCh0KTsNCiAgICAgICAgby5hcHBlbmRDaGlsZChtKTsNCiAgICAgICAgY2Fyci5wdXNoKG8pOw0KICAgICAgICBwb3B1cC5hcHBlbmRDaGlsZChvKTsNCiAgICAgICAgaGVpZ2h0PShtZXNzYWdlQXJyLmxlbmd0aCsxKSoxNitoZWlnaHQ7DQogICAgfQ0KICAgIHZhciB3YXJyPWdldGN3aChwb3B1cCk7DQogICAgb2xpbmUuc2V0QXR0cmlidXRlKCd4JywnMCcpOw0KICAgIG9saW5lLnNldEF0dHJpYnV0ZSgneScsJzAnKTsNCiAgICB2YXIgb3c9d2FyclswXSsxMC41Ow0KICAgIHZhciBvaD13YXJyWzFdKzM7DQogICAgb2xpbmUuc2V0QXR0cmlidXRlKCd3aWR0aCcsb3cpOw0KICAgIG9saW5lLnNldEF0dHJpYnV0ZSgnaGVpZ2h0JyxvaCk7DQogICAgb2xpbmUuc2V0QXR0cmlidXRlKCdmaWxsJywnd2hpdGUnKTsNCiAgICBvbGluZS5zZXRBdHRyaWJ1dGUoJ3N0cm9rZScsJyM2NTY1NjUnKTsNCiAgICBwb3B1cC5hcHBlbmRDaGlsZChvbGluZSk7DQogICAgdmFyIGw9Y2Fyci5sZW5ndGg7DQogICAgd2hpbGUobC0tKXsNCiAgICAgICAgcG9wdXAuYXBwZW5kQ2hpbGQoY2FycltsXSk7DQogICAgfQ0KICAgIHBvcHVwLm9ubW91c2VvdmVyPWZ1bmN0aW9uICgpIHsNCiAgICAgICAgdGhpcy5zdHlsZS5kaXNwbGF5PSdibG9jayc7DQogICAgfTsNCiAgICBwb3B1cC5vbm1vdXNlb3V0PWZ1bmN0aW9uICgpIHsNCiAgICAgICAgdGhpcy5zdHlsZS5kaXNwbGF5ID0gJ25vbmUnOw0KICAgIH07DQogICAgdmFyIGNzPWNvbW1lbnRzW2ldLnF1ZXJ5U2VsZWN0b3IoJ3VzZScpLmdldEF0dHJpYnV0ZSgndHJhbnNmb3JtJykubWF0Y2goL1woKFxTKnxcUypcc1xTKilcKS8pWzFdLnNwbGl0KC8gfCwvKTsNCiAgICB2YXIgcHM9Y29tbWVudHNbaV0ucGFyZW50Tm9kZS5nZXRBdHRyaWJ1dGUoJ3RyYW5zZm9ybScpOw0KICAgIGlmKHBzLnN1YnN0cigwLDIpID09ICd0cicpew0KICAgICAgICB2YXIgcHBzID0gcHMubWF0Y2goL1woKFxTKnxcUypcc1xTKilcKS8pWzFdLnNwbGl0KC8gfCwvKTsNCiAgICAgICAgdmFyIHg9cGFyc2VGbG9hdChjc1swXSkrcGFyc2VGbG9hdChwcHNbMF0pOw0KICAgICAgICB2YXIgeT1wYXJzZUZsb2F0KHBwc1sxXSk7DQogICAgICAgIHg9eC50b3N1aXRzdmcoKTsNCiAgICAgICAgeT15LnRvc3VpdHN2ZygpOw0KICAgICAgICB2YXIgdHJzdHIgPSAndHJhbnNsYXRlKCcreCsnLCcreSsnKSc7DQogICAgfQ0KICAgIGVsc2UgaWYocHMuc3Vic3RyKDAsMikgPT0gJ21hJyl7DQogICAgICAgIHZhciBwcHMgPSBwcy5tYXRjaCgvKFwtP1xkKyhcLlxkKyk/KVtcLCBdKFwtP1xkKyhcLlxkKyk/KVtcLCBdKFwtP1xkKyhcLlxkKyk/KVtcLCBdKFwtP1xkKyhcLlxkKyk/KVtcLCBdKFwtP1xkKyhcLlxkKyk/KVtcLCBdKFwtP1xkKyhcLlxkKyk/KVwpJC8pOw0KICAgICAgICB2YXIgbWFBcnIgPSBbcGFyc2VGbG9hdChwcHNbMV0pLHBhcnNlRmxvYXQocHBzWzNdKSxwYXJzZUZsb2F0KHBwc1s1XSkscGFyc2VGbG9hdChwcHNbN10pLHBhcnNlRmxvYXQocHBzWzldKSxwYXJzZUZsb2F0KHBwc1sxMV0pXTsNCiAgICAgICAgaWYobWFBcnJbMV0gPT0gMCl7DQogICAgICAgICAgICB2YXIgeCA9IHBhcnNlRmxvYXQoY3NbMF0pOw0KICAgICAgICAgICAgdmFyIHk9IHBhcnNlRmxvYXQoY3NbMV0pKzE2Ow0KICAgICAgICAgICAgdmFyIHgxID0geCAqIG1hQXJyWzBdICsgeSAqIG1hQXJyWzJdICsgbWFBcnJbNF07DQogICAgICAgICAgICB2YXIgeTEgPSB4ICogbWFBcnJbMV0gKyB5ICogbWFBcnJbM10gKyBtYUFycls1XTsNCiAgICAgICAgICAgIHgxPXgxLnRvc3VpdHN2ZygpOw0KICAgICAgICAgICAgeTE9eTEudG9zdWl0c3ZnKCk7DQogICAgICAgICAgICB2YXIgdHJzdHIgPSAgJ3RyYW5zbGF0ZSgnK3gxKycsJyt5MSsnKSc7DQogICAgICAgIH1lbHNlew0KICAgICAgICAgICAgdmFyIHggPSBwYXJzZUZsb2F0KGNzWzBdKSsxNjsNCiAgICAgICAgICAgIHZhciB5ID0gcGFyc2VGbG9hdChjc1sxXSkrMTY7DQogICAgICAgICAgICB2YXIgeDEgPSB4ICogbWFBcnJbMF0gKyB5ICogbWFBcnJbMl0gKyBtYUFycls0XTsNCiAgICAgICAgICAgIHZhciB5MSA9IHggKiBtYUFyclsxXSArIHkgKiBtYUFyclszXSArIG1hQXJyWzVdOw0KICAgICAgICAgICAgeCA9IHBhcnNlRmxvYXQoY3NbMF0pKzE2Ow0KICAgICAgICAgICAgeSA9IHBhcnNlRmxvYXQoY3NbMV0pOw0KICAgICAgICAgICAgdmFyIHgyID0geCAqIG1hQXJyWzBdICsgeSAqIG1hQXJyWzJdICsgbWFBcnJbNF07DQogICAgICAgICAgICB2YXIgeTIgPSB4ICogbWFBcnJbMV0gKyB5ICogbWFBcnJbM10gKyBtYUFycls1XTsNCiAgICAgICAgICAgIHZhciBmeCA9IHgxPHgyP3gxLnRvc3VpdHN2ZygpOiB4Mi50b3N1aXRzdmcoKTsNCiAgICAgICAgICAgIHZhciBmeSA9IHkxPnkyP3kxLnRvc3VpdHN2ZygpOiB5Mi50b3N1aXRzdmcoKTsNCiAgICAgICAgICAgIHZhciBvZmZ5ID0gTWF0aC5hYnMoeTEteTIpOw0KICAgICAgICAgICAgdmFyIHRyc3RyID0gICd0cmFuc2xhdGUoJytmeCsnLCcrZnkrJyknOw0KICAgICAgICAgICAgcG9wdXBSLnNldEF0dHJpYnV0ZSgnaGVpZ2h0JyxvZmZ5LnRvU3RyaW5nKCkpOw0KICAgICAgICAgICAgcG9wdXBSLnNldEF0dHJpYnV0ZSgnd2lkdGgnLCcxNicpOw0KICAgICAgICAgICAgcG9wdXBSLnNldEF0dHJpYnV0ZSgneScsKC1vZmZ5KS50b1N0cmluZygpKTsNCiAgICAgICAgICAgIHBvcHVwUi5zZXRBdHRyaWJ1dGUoJ2ZpbGwnLCd0cmFuc3BhcmVudCcpOw0KICAgICAgICAgICAgcG9wdXAuYXBwZW5kQ2hpbGQocG9wdXBSKTsNCiAgICAgICAgfQ0KICAgIH0NCiAgICBwb3B1cC5zZXRBdHRyaWJ1dGUoJ3RyYW5zZm9ybScsdHJzdHIpOw0KICAgIHBvcHVwLnNldEF0dHJpYnV0ZSgnY29tbWVudCcsJycpOw0KICAgIHBvcHVwLnN0eWxlLmRpc3BsYXk9J25vbmUnOw0KICAgIHBvcHVwLnNldEF0dHJpYnV0ZSgnZWQ6Y29tbWVudGlkJyxjb21tZW50c1tpXS5wYXJlbnROb2RlLmlkKTsNCiAgICBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCcjc3ZnLWNvbnRhaW5lciA+IHN2ZycpLmFwcGVuZENoaWxkKHBvcHVwKTsNCiAgICBjb21tZW50c1tpXS5vbm1vdXNlb3Zlcj1mdW5jdGlvbiAoKSB7DQogICAgICAgIHZhciBjb21tZW50aWQ9dGhpcy5wYXJlbnROb2RlLmlkOw0KICAgICAgICB0aGlzLnF1ZXJ5U2VsZWN0b3IoJ3JlY3QnKS5zdHlsZS5kaXNwbGF5PSdibG9jayc7DQogICAgICAgIGRvY3VtZW50LnF1ZXJ5U2VsZWN0b3IoImdbZWRcXDpjb21tZW50aWQ9JyIrY29tbWVudGlkKyInXVtjb21tZW50XSIpLnN0eWxlLmRpc3BsYXk9J2Jsb2NrJzsNCiAgICB9Ow0KICAgIGNvbW1lbnRzW2ldLm9ubW91c2VvdXQ9ZnVuY3Rpb24gKCkgew0KICAgICAgICB2YXIgY29tbWVudGlkPXRoaXMucGFyZW50Tm9kZS5pZDsNCi8vICAgICAgICB3aW5kb3cuZ2V0U2VsZWN0aW9uKCkucmVtb3ZlQWxsUmFuZ2VzKCk7DQogICAgICAgIHRoaXMucXVlcnlTZWxlY3RvcigncmVjdCcpLnN0eWxlLmRpc3BsYXk9J25vbmUnOw0KICAgICAgICBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCJnW2VkXFw6Y29tbWVudGlkPSciK2NvbW1lbnRpZCsiJ11bY29tbWVudF0iKS5zdHlsZS5kaXNwbGF5PSdub25lJzsNCiAgICB9DQp9DQovLy0tY29tbWVudA0KLy9ub3RlLS0NCmlmKCF1YSl7DQogICAgdmFyIG5vdGVzPWRvY3VtZW50LnF1ZXJ5U2VsZWN0b3JBbGwoJ2c+Z1tlZFxcOm5vdGVdJyk7DQogICAgZnVuY3Rpb24gZ2V0d2gocyxwKSB7DQogICAgICAgIHZhciBtYWlucD1kb2N1bWVudC5jcmVhdGVFbGVtZW50KCdkaXYnKTsNCiAgICAgICAgbWFpbnAuc3R5bGUuY3NzVGV4dD1zOw0KICAgICAgICBtYWlucC5zdHlsZS5kaXNwbGF5PSdpbmxpbmUtYmxvY2snOw0KCQltYWlucC5zdHlsZS5tYXhXaWR0aCA9ICc0MDBweCc7DQoJCW1haW5wLnN0eWxlLndvcmRCcmVhayA9ICdicmVhay1hbGwnOw0KICAgICAgICBtYWlucC5pbm5lckhUTUw9cDsNCiAgICAgICAgZG9jdW1lbnQuYm9keS5hcHBlbmRDaGlsZChtYWlucCk7DQogICAgICAgIHZhciB3PW1haW5wLmNsaWVudFdpZHRoOw0KICAgICAgICB2YXIgaD1tYWlucC5jbGllbnRIZWlnaHQ7DQogICAgICAgIGRvY3VtZW50LmJvZHkucmVtb3ZlQ2hpbGQobWFpbnApOw0KICAgICAgICByZXR1cm4gW3csaF0NCiAgICB9DQogICAgZm9yKHZhciBpPTA7aTxub3Rlcy5sZW5ndGg7aSsrKXsNCiAgICAgICAgdmFyIGE9bm90ZXNbaV0uZ2V0QXR0cmlidXRlKCdlZDpub3RlJyk7DQoJCXZhciBub3RlTG9jayA9IG5vdGVzW2ldLmdldEF0dHJpYnV0ZSgnZWQ6bm90ZWxvY2snKTsNCiAgICAgICAgaWYobm90ZUxvY2sgPT0gJ3RydWUnKXsNCiAgICAgICAgICAgIGNvbnRpbnVlOw0KICAgICAgICB9DQogICAgICAgIHZhciBtYWlucD1hLm1hdGNoKC88Ym9keVtePl0qPiguKik8XC9ib2R5Pi8pWzFdOw0KICAgICAgICB2YXIgbWFpbnM9YS5tYXRjaCgvc3R5bGU9IiguKj8pIi8pWzFdOw0KICAgICAgICB2YXIgb3V0PWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdnJyk7DQogICAgICAgIHZhciBvbGluZT1kb2N1bWVudC5jcmVhdGVFbGVtZW50TlMoJ2h0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnJywncmVjdCcpOw0KICAgICAgICB2YXIgcG9wdXA9ZG9jdW1lbnQuY3JlYXRlRWxlbWVudE5TKCdodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZycsJ2ZvcmVpZ25PYmplY3QnKTsNCiAgICAgICAgdmFyIHBvcHVwUj0gZG9jdW1lbnQuY3JlYXRlRWxlbWVudE5TKCdodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZycsJ3JlY3QnKTsNCiAgICAgICAgdmFyIGhvdmVyPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdyZWN0Jyk7DQogICAgICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgnZmlsbCcsJyNjZGNkZmYnKTsNCiAgICAgICAgaG92ZXIuc2V0QXR0cmlidXRlKCd4JywnMCcpOw0KICAgICAgICBob3Zlci5zZXRBdHRyaWJ1dGUoJ3knLCcwJyk7DQogICAgICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgnaGVpZ2h0JywnMTYnKTsNCiAgICAgICAgaG92ZXIuc2V0QXR0cmlidXRlKCd3aWR0aCcsJzE2Jyk7DQogICAgICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgnZmlsbC1vcGFjaXR5JywnMC42Jyk7DQogICAgICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgndHJhbnNmb3JtJyxub3Rlc1tpXS5xdWVyeVNlbGVjdG9yKCd1c2UnKS5nZXRBdHRyaWJ1dGUoJ3RyYW5zZm9ybScpKTsNCiAgICAgICAgaG92ZXIuc3R5bGUuZGlzcGxheT0nbm9uZSc7DQogICAgICAgIG5vdGVzW2ldLmFwcGVuZENoaWxkKGhvdmVyKTsNCiAgICAgICAgcG9wdXAuc3R5bGUuY3NzVGV4dD1tYWluczsNCiAgICAgICAgcG9wdXAuaW5uZXJIVE1MPW1haW5wOw0KICAgICAgICB2YXIgd2g9Z2V0d2gobWFpbnMsbWFpbnApOw0KICAgICAgICBwb3B1cC5zZXRBdHRyaWJ1dGUoJ3dpZHRoJyx3aFswXSk7DQogICAgICAgIHBvcHVwLnNldEF0dHJpYnV0ZSgnaGVpZ2h0Jyx3aFsxXSk7DQogICAgICAgIHBvcHVwLnNldEF0dHJpYnV0ZSgndHJhbnNmb3JtJywndHJhbnNsYXRlKDgsNCknKTsNCgkJcG9wdXAuc3R5bGUud29yZEJyZWFrID0gJ2JyZWFrLWFsbCc7DQogICAgICAgIHBvcHVwLnN0eWxlLnRleHRBbGlnbj0nbGVmdCc7DQogICAgICAgIG9saW5lLnNldEF0dHJpYnV0ZSgneCcsJzAnKTsNCiAgICAgICAgb2xpbmUuc2V0QXR0cmlidXRlKCd5JywnMCcpOw0KICAgICAgICBvbGluZS5zZXRBdHRyaWJ1dGUoJ3dpZHRoJyx3aFswXSsxNik7DQogICAgICAgIG9saW5lLnNldEF0dHJpYnV0ZSgnaGVpZ2h0Jyx3aFsxXSs4KTsNCiAgICAgICAgb2xpbmUuc2V0QXR0cmlidXRlKCdzdHJva2UnLCcjYTI3YTAwJyk7DQogICAgICAgIG9saW5lLnNldEF0dHJpYnV0ZSgnZmlsbCcsJyNmZmU3OWQnKTsNCiAgICAgICAgb3V0LmFwcGVuZENoaWxkKG9saW5lKTsNCiAgICAgICAgb3V0LmFwcGVuZENoaWxkKHBvcHVwKTsNCiAgICAgICAgb3V0LnNldEF0dHJpYnV0ZSgnbm90ZScsJycpOw0KICAgICAgICBvdXQuc3R5bGUuZGlzcGxheT0nbm9uZSc7DQogICAgICAgIG91dC5zZXRBdHRyaWJ1dGUoJ2VkOm5vdGVpZCcsbm90ZXNbaV0ucGFyZW50Tm9kZS5pZCk7DQogICAgICAgIG91dC5vbm1vdXNlb3Zlcj1mdW5jdGlvbiAoKSB7DQogICAgICAgICAgICB0aGlzLnN0eWxlLmRpc3BsYXk9J2Jsb2NrJzsNCiAgICAgICAgfTsNCiAgICAgICAgb3V0Lm9ubW91c2VvdXQ9ZnVuY3Rpb24gKCkgew0KLy8gICAgICAgIHdpbmRvdy5nZXRTZWxlY3Rpb24gPyB3aW5kb3cuZ2V0U2VsZWN0aW9uKCkucmVtb3ZlUmFuZ2Uod2luZG93LmdldFNlbGVjdGlvbigpLnJlKTpkb2N1bWVudC5zZWxlY3Rpb24uZW1wdHkoKTsNCg0KICAgICAgICAgICAgdGhpcy5zdHlsZS5kaXNwbGF5PSdub25lJzsNCiAgICAgICAgfTsNCiAgICAgICAgdmFyIGNzPW5vdGVzW2ldLnF1ZXJ5U2VsZWN0b3IoJ3VzZScpLmdldEF0dHJpYnV0ZSgndHJhbnNmb3JtJykubWF0Y2goL1woKFxTKnxcUypcc1xTKilcKS8pWzFdLnNwbGl0KC8gfCwvKTsNCiAgICAgICAgdmFyIHBzPW5vdGVzW2ldLnBhcmVudE5vZGUuZ2V0QXR0cmlidXRlKCd0cmFuc2Zvcm0nKTsNCiAgICAgICAgaWYocHMuc3Vic3RyKDAsMikgPT0gJ3RyJyl7DQogICAgICAgICAgICB2YXIgcHBzID0gcHMubWF0Y2goL1woKFxTKnxcUypcc1xTKilcKS8pWzFdLnNwbGl0KC8gfCwvKTsNCiAgICAgICAgICAgIHZhciB4PXBhcnNlRmxvYXQoY3NbMF0pK3BhcnNlRmxvYXQocHBzWzBdKTsNCiAgICAgICAgICAgIHZhciB5PXBhcnNlRmxvYXQocHBzWzFdKTsNCiAgICAgICAgICAgIHg9eC50b3N1aXRzdmcoKTsNCiAgICAgICAgICAgIHk9eS50b3N1aXRzdmcoKTsNCiAgICAgICAgICAgIHZhciB0cnN0ciA9ICd0cmFuc2xhdGUoJyt4KycsJyt5KycpJzsNCiAgICAgICAgfWVsc2UgaWYocHMuc3Vic3RyKDAsMikgPT0gJ21hJyl7DQogICAgICAgICAgICB2YXIgcHBzID0gcHMubWF0Y2goLyhcLT9cZCsoXC5cZCspPylbXCwgXShcLT9cZCsoXC5cZCspPylbXCwgXShcLT9cZCsoXC5cZCspPylbXCwgXShcLT9cZCsoXC5cZCspPylbXCwgXShcLT9cZCsoXC5cZCspPylbXCwgXShcLT9cZCsoXC5cZCspPylcKSQvKTsNCiAgICAgICAgICAgIHZhciBtYUFyciA9IFtwYXJzZUZsb2F0KHBwc1sxXSkscGFyc2VGbG9hdChwcHNbM10pLHBhcnNlRmxvYXQocHBzWzVdKSxwYXJzZUZsb2F0KHBwc1s3XSkscGFyc2VGbG9hdChwcHNbOV0pLHBhcnNlRmxvYXQocHBzWzExXSldOw0KICAgICAgICAgICAgaWYobWFBcnJbMV0gPT0gMCl7DQogICAgICAgICAgICAgICAgdmFyIHggPSBwYXJzZUZsb2F0KGNzWzBdKTsNCiAgICAgICAgICAgICAgICB2YXIgeSA9IHBhcnNlRmxvYXQoY3NbMV0pKzE2Ow0KICAgICAgICAgICAgICAgIHZhciB4MSA9IHggKiBtYUFyclswXSArIHkgKiBtYUFyclsyXSArIG1hQXJyWzRdOw0KICAgICAgICAgICAgICAgIHZhciB5MSA9IHggKiBtYUFyclsxXSArIHkgKiBtYUFyclszXSArIG1hQXJyWzVdOw0KICAgICAgICAgICAgICAgIHgxPXgxLnRvc3VpdHN2ZygpOw0KICAgICAgICAgICAgICAgIHkxPXkxLnRvc3VpdHN2ZygpOw0KICAgICAgICAgICAgICAgIHZhciB0cnN0ciA9ICAndHJhbnNsYXRlKCcreDErJywnK3kxKycpJzsNCiAgICAgICAgICAgIH1lbHNlew0KICAgICAgICAgICAgICAgIHZhciB4ID0gcGFyc2VGbG9hdChjc1swXSkrMTY7DQogICAgICAgICAgICAgICAgdmFyIHkgPSBwYXJzZUZsb2F0KGNzWzFdKSsxNjsNCiAgICAgICAgICAgICAgICB2YXIgeDEgPSB4ICogbWFBcnJbMF0gKyB5ICogbWFBcnJbMl0gKyBtYUFycls0XTsNCiAgICAgICAgICAgICAgICB2YXIgeTEgPSB4ICogbWFBcnJbMV0gKyB5ICogbWFBcnJbM10gKyBtYUFycls1XTsNCiAgICAgICAgICAgICAgICB4ID0gcGFyc2VGbG9hdChjc1swXSkrMTY7DQogICAgICAgICAgICAgICAgeSA9IHBhcnNlRmxvYXQoY3NbMV0pOw0KICAgICAgICAgICAgICAgIHZhciB4MiA9IHggKiBtYUFyclswXSArIHkgKiBtYUFyclsyXSArIG1hQXJyWzRdOw0KICAgICAgICAgICAgICAgIHZhciB5MiA9IHggKiBtYUFyclsxXSArIHkgKiBtYUFyclszXSArIG1hQXJyWzVdOw0KICAgICAgICAgICAgICAgIHZhciBmeCA9IHgxPHgyP3gxLnRvc3VpdHN2ZygpOiB4Mi50b3N1aXRzdmcoKTsNCiAgICAgICAgICAgICAgICB2YXIgZnkgPSB5MT55Mj95MS50b3N1aXRzdmcoKTogeTIudG9zdWl0c3ZnKCk7DQoJCQkJdmFyIG9mZnkgPSBNYXRoLmFicyh5MS15Mik7CQkJCQkJCQkJCSAgDQogICAgICAgICAgICAgICAgdmFyIHRyc3RyID0gICd0cmFuc2xhdGUoJytmeCsnLCcrZnkrJyknOw0KICAgICAgICAgICAgICAgIHBvcHVwUi5zZXRBdHRyaWJ1dGUoJ2hlaWdodCcsb2ZmeS50b1N0cmluZygpKTsNCiAgICAgICAgICAgICAgICBwb3B1cFIuc2V0QXR0cmlidXRlKCd3aWR0aCcsJzE2Jyk7DQogICAgICAgICAgICAgICAgcG9wdXBSLnNldEF0dHJpYnV0ZSgneScsKC1vZmZ5KS50b1N0cmluZygpKTsNCiAgICAgICAgICAgICAgICBwb3B1cFIuc2V0QXR0cmlidXRlKCdmaWxsJywndHJhbnNwYXJlbnQnKTsNCiAgICAgICAgICAgICAgICBwb3B1cC5hcHBlbmRDaGlsZChwb3B1cFIpOw0KICAgICAgICAgICAgfQ0KICAgICAgICB9DQogICAgICAgIG91dC5zZXRBdHRyaWJ1dGUoJ3RyYW5zZm9ybScsdHJzdHIpOw0KICAgICAgICBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCcjc3ZnLWNvbnRhaW5lciA+IHN2ZycpLmFwcGVuZENoaWxkKG91dCk7DQogICAgICAgIG5vdGVzW2ldLm9ubW91c2VvdmVyPWZ1bmN0aW9uICgpIHsNCiAgICAgICAgICAgIHZhciBub3RlaWQ9dGhpcy5wYXJlbnROb2RlLmlkOw0KICAgICAgICAgICAgdGhpcy5xdWVyeVNlbGVjdG9yKCdyZWN0Jykuc3R5bGUuZGlzcGxheT0nYmxvY2snOw0KICAgICAgICAgICAgZG9jdW1lbnQucXVlcnlTZWxlY3RvcigiZ1tlZFxcOm5vdGVpZD0nIitub3RlaWQrIiddW25vdGVdIikuc3R5bGUuZGlzcGxheT0nYmxvY2snOw0KICAgICAgICB9Ow0KICAgICAgICBub3Rlc1tpXS5vbm1vdXNlb3V0PWZ1bmN0aW9uICgpIHsNCiAgICAgICAgICAgIHZhciBub3RlaWQ9dGhpcy5wYXJlbnROb2RlLmlkOw0KLy8gICAgICAgIHdpbmRvdy5nZXRTZWxlY3Rpb24oKS5yZW1vdmVBbGxSYW5nZXMoKTsNCiAgICAgICAgICAgIHRoaXMucXVlcnlTZWxlY3RvcigncmVjdCcpLnN0eWxlLmRpc3BsYXk9J25vbmUnOw0KICAgICAgICAgICAgZG9jdW1lbnQucXVlcnlTZWxlY3RvcigiZ1tlZFxcOm5vdGVpZD0nIitub3RlaWQrIiddW25vdGVdIikuc3R5bGUuZGlzcGxheT0nbm9uZSc7DQogICAgICAgIH0NCiAgICB9DQp9ZWxzZXsNCiAgICBjb25zb2xlLmxvZygn5oqx5q2J77yMSUXmtY/op4jlmajkuI3mlK/mjIFub3Rl6Kej5p6Q77yM6K+35L2/55So5YW25LuW5YaF5qC45rWP6KeI5Zmo44CC6LCi6LCi77yBJykNCn0NCi8vLS1ub3RlDQovL2h5cGVybGluay0tDQp2YXIgbGlua3M9ZG9jdW1lbnQucXVlcnlTZWxlY3RvckFsbCgnZz5nW2VkXFw6aHlwZXJsaW5rXScpOw0KZnVuY3Rpb24gZ2V0bWF4bGVuKGFycixicnIpIHsNCiAgICB2YXIgbD0wOw0KICAgIHZhciBsbD0wOw0KICAgIGZvcih2YXIgaj0wO2o8YXJyLmxlbmd0aDtqKyspew0KICAgICAgICB2YXIgZT1kb2N1bWVudC5jcmVhdGVFbGVtZW50TlMoJ2h0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnJywndGV4dCcpOw0KICAgICAgICBpZighaXNOYU4obGlua2FycltqXSkpew0KICAgICAgICAgICAgZS50ZXh0Q29udGVudD0nUGFnZS0nK2FycltqXTsNCiAgICAgICAgfWVsc2V7DQogICAgICAgICAgICBlLnRleHRDb250ZW50PWFycltqXTsNCiAgICAgICAgfQ0KICAgICAgICBlLnN0eWxlLmZvbnRTaXplPScxMnB4JzsNCiAgICAgICAgZG9jdW1lbnQuYm9keS5nZXRFbGVtZW50c0J5VGFnTmFtZSgnc3ZnJylbMF0uYXBwZW5kQ2hpbGQoZSk7DQogICAgICAgIHZhciBldz1lLmdldEJCb3goKS53aWR0aDsNCiAgICAgICAgZG9jdW1lbnQuYm9keS5nZXRFbGVtZW50c0J5VGFnTmFtZSgnc3ZnJylbMF0ucmVtb3ZlQ2hpbGQoZSk7DQogICAgICAgIHZhciBoPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCd0ZXh0Jyk7DQogICAgICAgIGgudGV4dENvbnRlbnQ9YnJyW2pdOw0KICAgICAgICBoLnN0eWxlLmZvbnRTaXplPScxMnB4JzsNCiAgICAgICAgaC5zdHlsZS5mb250V2VpZ2h0PSdib2xkJzsNCiAgICAgICAgZG9jdW1lbnQuYm9keS5nZXRFbGVtZW50c0J5VGFnTmFtZSgnc3ZnJylbMF0uYXBwZW5kQ2hpbGQoaCk7DQogICAgICAgIHZhciBodz1oLmdldEJCb3goKS53aWR0aDsNCiAgICAgICAgZG9jdW1lbnQuYm9keS5nZXRFbGVtZW50c0J5VGFnTmFtZSgnc3ZnJylbMF0ucmVtb3ZlQ2hpbGQoaCk7DQogICAgICAgIGw9ZXc+aHc/ZXc6aHc7DQogICAgICAgIGxsPWw+bGw/bDpsbDsNCiAgICB9DQogICAgcmV0dXJuIGxsOw0KfQ0KZm9yKHZhciBpPTA7aTxsaW5rcy5sZW5ndGg7aSsrKXsNCiAgICB2YXIgcG9wdXA9ZG9jdW1lbnQuY3JlYXRlRWxlbWVudE5TKCdodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZycsJ2cnKTsNCiAgICB2YXIgcG9wdXBSPSBkb2N1bWVudC5jcmVhdGVFbGVtZW50TlMoJ2h0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnJywncmVjdCcpOw0KICAgIHZhciBob3Zlcj1kb2N1bWVudC5jcmVhdGVFbGVtZW50TlMoJ2h0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnJywncmVjdCcpOw0KICAgIHZhciBkZXNjYXJyPVtdOw0KICAgIHZhciBsaW5rYXJyPVtdOw0KICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgnZmlsbCcsJyNjZGNkZmYnKTsNCiAgICBob3Zlci5zZXRBdHRyaWJ1dGUoJ3gnLCcwJyk7DQogICAgaG92ZXIuc2V0QXR0cmlidXRlKCd5JywnMCcpOw0KICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgnaGVpZ2h0JywnMTYnKTsNCiAgICBob3Zlci5zZXRBdHRyaWJ1dGUoJ3dpZHRoJywnMTYnKTsNCiAgICBob3Zlci5zZXRBdHRyaWJ1dGUoJ2ZpbGwtb3BhY2l0eScsJzAuNicpOw0KICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgndHJhbnNmb3JtJyxsaW5rc1tpXS5xdWVyeVNlbGVjdG9yKCd1c2UnKS5nZXRBdHRyaWJ1dGUoJ3RyYW5zZm9ybScpKTsNCiAgICBob3Zlci5zdHlsZS5kaXNwbGF5PSdub25lJzsNCiAgICBsaW5rc1tpXS5hcHBlbmRDaGlsZChob3Zlcik7DQogICAgLy8gY29uc29sZS5sb2cobGlua3NbaV0uZ2V0QXR0cmlidXRlKCdlZDpoeXBlcmxpbmsnKSk7DQogICAgdmFyIGE9SlNPTi5wYXJzZShsaW5rc1tpXS5nZXRBdHRyaWJ1dGUoJ2VkOmh5cGVybGluaycpKTsNCiAgICB2YXIgY3M9bGlua3NbaV0ucXVlcnlTZWxlY3RvcigndXNlJykuZ2V0QXR0cmlidXRlKCd0cmFuc2Zvcm0nKS5tYXRjaCgvXCgoXFMqfFxTKlxzXFMqKVwpLylbMV0uc3BsaXQoLyB8LC8pOw0KICAgIHZhciBwcz1saW5rc1tpXS5wYXJlbnROb2RlLmdldEF0dHJpYnV0ZSgndHJhbnNmb3JtJyk7DQogICAgaWYocHMuc3Vic3RyKDAsMikgPT0gJ3RyJyl7DQogICAgICAgIHZhciBwcHMgPSBwcy5tYXRjaCgvXCgoXFMqfFxTKlxzXFMqKVwpLylbMV0uc3BsaXQoLyB8LC8pOw0KICAgICAgICB2YXIgeD1wYXJzZUZsb2F0KGNzWzBdKStwYXJzZUZsb2F0KHBwc1swXSk7DQogICAgICAgIHZhciB5PXBhcnNlRmxvYXQocHBzWzFdKTsNCiAgICAgICAgeD14LnRvc3VpdHN2ZygpOw0KICAgICAgICB5PXkudG9zdWl0c3ZnKCk7DQogICAgICAgIHZhciB0cnN0ciA9ICd0cmFuc2xhdGUoJyt4KycsJyt5KycpJzsNCiAgICB9ZWxzZSBpZihwcy5zdWJzdHIoMCwyKSA9PSAnbWEnKXsNCiAgICAgICAgdmFyIHBwcyA9IHBzLm1hdGNoKC8oXC0/XGQrKFwuXGQrKT8pW1wsIF0oXC0/XGQrKFwuXGQrKT8pW1wsIF0oXC0/XGQrKFwuXGQrKT8pW1wsIF0oXC0/XGQrKFwuXGQrKT8pW1wsIF0oXC0/XGQrKFwuXGQrKT8pW1wsIF0oXC0/XGQrKFwuXGQrKT8pXCkkLyk7DQogICAgICAgIHZhciBtYUFyciA9IFtwYXJzZUZsb2F0KHBwc1sxXSkscGFyc2VGbG9hdChwcHNbM10pLHBhcnNlRmxvYXQocHBzWzVdKSxwYXJzZUZsb2F0KHBwc1s3XSkscGFyc2VGbG9hdChwcHNbOV0pLHBhcnNlRmxvYXQocHBzWzExXSldOw0KICAgICAgICBpZihtYUFyclsxXSA9PSAwKXsNCiAgICAgICAgICAgIHZhciB4ID0gcGFyc2VGbG9hdChjc1swXSk7DQogICAgICAgICAgICB2YXIgeSA9IHBhcnNlRmxvYXQoY3NbMV0pKzE2Ow0KICAgICAgICAgICAgdmFyIHgxID0geCAqIG1hQXJyWzBdICsgeSAqIG1hQXJyWzJdICsgbWFBcnJbNF07DQogICAgICAgICAgICB2YXIgeTEgPSB4ICogbWFBcnJbMV0gKyB5ICogbWFBcnJbM10gKyBtYUFycls1XTsNCiAgICAgICAgICAgIHgxPXgxLnRvc3VpdHN2ZygpOw0KICAgICAgICAgICAgeTE9eTEudG9zdWl0c3ZnKCk7DQogICAgICAgICAgICB2YXIgdHJzdHIgPSAgJ3RyYW5zbGF0ZSgnK3gxKycsJyt5MSsnKSc7DQogICAgICAgIH1lbHNlew0KICAgICAgICAgICAgdmFyIHggPSBwYXJzZUZsb2F0KGNzWzBdKSsxNjsNCiAgICAgICAgICAgIHZhciB5ID0gcGFyc2VGbG9hdChjc1sxXSkrMTY7DQogICAgICAgICAgICB2YXIgeDEgPSB4ICogbWFBcnJbMF0gKyB5ICogbWFBcnJbMl0gKyBtYUFycls0XTsNCiAgICAgICAgICAgIHZhciB5MSA9IHggKiBtYUFyclsxXSArIHkgKiBtYUFyclszXSArIG1hQXJyWzVdOw0KICAgICAgICAgICAgeCA9IHBhcnNlRmxvYXQoY3NbMF0pKzE2Ow0KICAgICAgICAgICAgeSA9IHBhcnNlRmxvYXQoY3NbMV0pOw0KICAgICAgICAgICAgdmFyIHgyID0geCAqIG1hQXJyWzBdICsgeSAqIG1hQXJyWzJdICsgbWFBcnJbNF07DQogICAgICAgICAgICB2YXIgeTIgPSB4ICogbWFBcnJbMV0gKyB5ICogbWFBcnJbM10gKyBtYUFycls1XTsNCiAgICAgICAgICAgIHZhciBmeCA9IHgxPHgyP3gxLnRvc3VpdHN2ZygpOiB4Mi50b3N1aXRzdmcoKTsNCiAgICAgICAgICAgIHZhciBmeSA9IHkxPnkyP3kxLnRvc3VpdHN2ZygpOiB5Mi50b3N1aXRzdmcoKTsNCgkJCXZhciBvZmZ5ID0gTWF0aC5hYnMoeTEteTIpOwkJCQkJCQkJCQkgIA0KICAgICAgICAgICAgdmFyIHRyc3RyID0gICd0cmFuc2xhdGUoJytmeCsnLCcrZnkrJyknOw0KICAgICAgICAgICAgcG9wdXBSLnNldEF0dHJpYnV0ZSgnaGVpZ2h0JyxvZmZ5LnRvU3RyaW5nKCkpOw0KICAgICAgICAgICAgcG9wdXBSLnNldEF0dHJpYnV0ZSgnd2lkdGgnLCcxNicpOw0KICAgICAgICAgICAgcG9wdXBSLnNldEF0dHJpYnV0ZSgneScsKC1vZmZ5KS50b1N0cmluZygpKTsNCiAgICAgICAgICAgIHBvcHVwUi5zZXRBdHRyaWJ1dGUoJ2ZpbGwnLCd0cmFuc3BhcmVudCcpOw0KICAgICAgICAgICAgcG9wdXAuYXBwZW5kQ2hpbGQocG9wdXBSKTsNCiAgICAgICAgfQ0KICAgIH0NCiAgICB2YXIgYWw9YS5sZW5ndGg7DQogICAgZm9yKHZhciBqPTA7ajxhbDtqKyspew0KICAgICAgICBsaW5rYXJyLnB1c2goYVtqXS5saW5rKTsNCiAgICAgICAgZGVzY2Fyci5wdXNoKGFbal0uZGVzYyk7DQogICAgfQ0KICAgIHBvcHVwLnNldEF0dHJpYnV0ZSgndHJhbnNmb3JtJyx0cnN0cik7DQogICAgdmFyIG1heD1nZXRtYXhsZW4obGlua2FycixkZXNjYXJyKTsNCiAgICBmb3IodmFyIGs9MDtrPGFsO2srKyl7DQogICAgICAgIHZhciBjPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdhJyk7DQogICAgICAgIHZhciBkPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdyZWN0Jyk7DQogICAgICAgIHZhciBlPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCd0ZXh0Jyk7DQogICAgICAgIHZhciBmPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCd0ZXh0Jyk7DQogICAgICAgIGlmKGlzTmFOKGxpbmthcnJba10pKXsNCiAgICAgICAgICAgIGMuc2V0QXR0cmlidXRlTlMoImh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiwgInhsaW5rIiwgImh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiKTsNCiAgICAgICAgICAgIGMuc2V0QXR0cmlidXRlTlMoImh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiLCAiaHJlZiIsIGxpbmthcnJba10pOw0KICAgICAgICAgICAgYy5zZXRBdHRyaWJ1dGUoJ3RhcmdldCcsJ19ibGFuaycpOw0KICAgICAgICAgICAgZS50ZXh0Q29udGVudD1saW5rYXJyW2tdOw0KICAgICAgICB9ZWxzZXsNCiAgICAgICAgICAgIGUudGV4dENvbnRlbnQ9J1BhZ2UtJytsaW5rYXJyW2tdOw0KICAgICAgICAgICAgYy5zZXRBdHRyaWJ1dGVOUygiaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciLCAieGxpbmsiLCAiaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayIpOw0KICAgICAgICAgICAgYy5zZXRBdHRyaWJ1dGVOUygiaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayIsICJocmVmIiwgIiMiK2xpbmthcnJba10pOw0KICAgICAgICB9DQogICAgICAgIGQuc2V0QXR0cmlidXRlKCd3aWR0aCcsbWF4KzEwKTsNCiAgICAgICAgZC5zZXRBdHRyaWJ1dGUoJ2hlaWdodCcsJzMzJyk7DQogICAgICAgIGQuc2V0QXR0cmlidXRlKCdzdHJva2UnLCcjOTk5OTk5Jyk7DQogICAgICAgIGQuc2V0QXR0cmlidXRlKCdmaWxsJywnd2hpdGUnKTsNCiAgICAgICAgZC5zZXRBdHRyaWJ1dGUoJ3knLDMzKmspOw0KICAgICAgICBmLnRleHRDb250ZW50PWRlc2NhcnJba107DQogICAgICAgIGYuc3R5bGUuZm9udFNpemU9JzEycHgnOw0KICAgICAgICBmLnN0eWxlLmZvbnRXZWlnaHQ9J2JvbGQnOw0KICAgICAgICBmLnNldEF0dHJpYnV0ZSgneCcsNSk7DQogICAgICAgIGYuc2V0QXR0cmlidXRlKCd5JywzMyprKzEyKTsNCiAgICAgICAgZS5zdHlsZS5mb250U2l6ZT0nMTJweCc7DQogICAgICAgIGUuc2V0QXR0cmlidXRlKCd5JywzMyprKzI4KTsNCiAgICAgICAgZS5zZXRBdHRyaWJ1dGUoJ3gnLDUpOw0KICAgICAgICBjLmFwcGVuZENoaWxkKGQpOw0KICAgICAgICBjLmFwcGVuZENoaWxkKGYpOw0KICAgICAgICBjLmFwcGVuZENoaWxkKGUpOw0KICAgICAgICBjLm9ubW91c2VvdmVyPWZ1bmN0aW9uICgpIHsNCiAgICAgICAgICAgIHRoaXMucXVlcnlTZWxlY3RvcigncmVjdCcpLnN0eWxlLmZpbGw9JyNlMWUxZmYnDQogICAgICAgIH07DQogICAgICAgIGMub25tb3VzZW91dD1mdW5jdGlvbiAoKSB7DQogICAgICAgICAgICB0aGlzLnF1ZXJ5U2VsZWN0b3IoJ3JlY3QnKS5zdHlsZS5maWxsPSd3aGl0ZScNCiAgICAgICAgfTsNCiAgICAgICAgcG9wdXAuYXBwZW5kQ2hpbGQoYyk7DQogICAgfQ0KICAgIHBvcHVwLnN0eWxlLmRpc3BsYXk9J25vbmUnOw0KICAgIHBvcHVwLnNldEF0dHJpYnV0ZSgnaHlwZXJsaW5rJywnJyk7DQogICAgcG9wdXAuc2V0QXR0cmlidXRlKCdlZDpsaW5raWQnLGxpbmtzW2ldLnBhcmVudE5vZGUuaWQpOw0KICAgIHBvcHVwLm9ubW91c2VvdmVyPWZ1bmN0aW9uICgpIHsNCiAgICAgICAgdGhpcy5zdHlsZS5kaXNwbGF5PSdibG9jayc7DQogICAgfTsNCiAgICBwb3B1cC5vbmNsaWNrPWZ1bmN0aW9uICgpIHsNCiAgICAgICAgdGhpcy5zdHlsZS5kaXNwbGF5PSdub25lJzsNCiAgICB9Ow0KICAgIHBvcHVwLm9ubW91c2VvdXQ9ZnVuY3Rpb24gKCkgew0KICAgICAgICB0aGlzLnN0eWxlLmRpc3BsYXk9J25vbmUnOw0KICAgIH07DQogICAgZG9jdW1lbnQucXVlcnlTZWxlY3RvcignI3N2Zy1jb250YWluZXIgPiBzdmcnKS5hcHBlbmRDaGlsZChwb3B1cCk7DQogICAgbGlua3NbaV0ub25tb3VzZW92ZXI9ZnVuY3Rpb24gKCkgew0KICAgICAgICB2YXIgbGlua2lkPXRoaXMucGFyZW50Tm9kZS5pZDsNCiAgICAgICAgdGhpcy5xdWVyeVNlbGVjdG9yKCdyZWN0Jykuc3R5bGUuZGlzcGxheT0nYmxvY2snOw0KICAgICAgICBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCJnW2VkXFw6bGlua2lkPSciK2xpbmtpZCsiJ11baHlwZXJsaW5rXSIpLnN0eWxlLmRpc3BsYXk9J2Jsb2NrJzsNCiAgICB9DQogICAgbGlua3NbaV0ub25tb3VzZW91dD1mdW5jdGlvbiAoKSB7DQogICAgICAgIHZhciBsaW5raWQ9dGhpcy5wYXJlbnROb2RlLmlkOw0KICAgICAgICB0aGlzLnF1ZXJ5U2VsZWN0b3IoJ3JlY3QnKS5zdHlsZS5kaXNwbGF5PSdub25lJzsNCiAgICAgICAgZG9jdW1lbnQucXVlcnlTZWxlY3RvcigiZ1tlZFxcOmxpbmtpZD0nIitsaW5raWQrIiddW2h5cGVybGlua10iKS5zdHlsZS5kaXNwbGF5PSdub25lJzsNCiAgICB9DQp9DQovLy0taHlwZXJsaW5rDQovL2luaXRpYWxpemUtLQ0KdmFyIHNoYXBlPWRvY3VtZW50LnF1ZXJ5U2VsZWN0b3JBbGwoJ2dbZWRcXDp0b2d0b3BpY2lkXScpOw0KdmFyIG1JZD1kb2N1bWVudC5xdWVyeVNlbGVjdG9yQWxsKCdnW2VkXFw6dG9waWN0eXBlXScpOw0KdmFyIGRhdGFUcmVlPXt9Ow0KdmFyIGV4dHJhUmVsYT17fTsNCnZhciBjaGVja0lEPScnOw0KZm9yKHZhciBpPTA7aTxtSWQubGVuZ3RoO2krKyl7DQogICAgdmFyIHR5cGU9bUlkW2ldLmdldEF0dHJpYnV0ZSgnZWQ6dG9waWN0eXBlJyk7DQogICAgdmFyIHNpZD1tSWRbaV0uaWQ7DQogICAgaWYodHlwZSE9PSdjYWxsb3V0Jyl7DQogICAgICAgIGluaXQoc2lkLGRhdGFUcmVlKQ0KICAgIH0NCn0NCmZ1bmN0aW9uIGluaXQoaWQsIG9iaikgew0KICAgIHZhciBjaGlsZHMgPSBkb2N1bWVudC5xdWVyeVNlbGVjdG9yQWxsKCJnW2VkXFw6cGFyZW50aWQ9JyIgKyBpZCArICInXTpub3QoW2VkXFw6dG9waWN0eXBlXSkiKTsNCiAgICB2YXIgY2FsbHMgPSBkb2N1bWVudC5xdWVyeVNlbGVjdG9yQWxsKCJnW2VkXFw6cGFyZW50aWQ9JyIgKyBpZCArICInXVtlZFxcOnRvcGljdHlwZV0iKTsNCiAgICB2YXIgc3VtbWFyeSA9IGRvY3VtZW50LnF1ZXJ5U2VsZWN0b3JBbGwoInBhdGhbZWRcXDpwYXJlbnRpZCo9JyIgKyBpZCArICInXVtlZFxcOnR5cGU9J3N1bW1hcnknXSIpOw0KICAgIHZhciBib3VuZGFyeT0gZG9jdW1lbnQucXVlcnlTZWxlY3RvckFsbCgicGF0aFtlZFxcOnBhcmVudGlkKj0nIiArIGlkICsgIiddW2VkXFw6dHlwZT0nYm91bmRhcnknXSIpOw0KICAgIHZhciByZWxhZnJvbT1kb2N1bWVudC5xdWVyeVNlbGVjdG9yQWxsKCJnW2VkXFw6ZnJvbWlkKj0nIiArIGlkICsgIiddW2VkXFw6dHlwZT0ncmVsYXRpb24nXSIpOw0KICAgIHZhciByZWxhdG89ZG9jdW1lbnQucXVlcnlTZWxlY3RvckFsbCgiZ1tlZFxcOnRvaWQqPSciICsgaWQgKyAiJ11bZWRcXDp0eXBlPSdyZWxhdGlvbiddIik7DQogICAgb2JqWyJtIiArIGlkXSA9IHt9Ow0KICAgIHZhciB0eXBlID0gZG9jdW1lbnQuZ2V0RWxlbWVudEJ5SWQoaWQpLmdldEF0dHJpYnV0ZSgnZWQ6dG9waWN0eXBlJyk7DQogICAgdmFyIGl3PWRvY3VtZW50LmdldEVsZW1lbnRCeUlkKGlkKS5nZXRBdHRyaWJ1dGUoJ2VkOndpZHRoJyk7DQogICAgdmFyIGloPWRvY3VtZW50LmdldEVsZW1lbnRCeUlkKGlkKS5nZXRBdHRyaWJ1dGUoJ2VkOmhlaWdodCcpOw0KICAgIGlmICh0eXBlKSB7DQogICAgICAgIG9ialsibSIgKyBpZF0udHlwZSA9IHR5cGU7DQogICAgfQ0KICAgIGlmKGl3JiZpaCl7DQogICAgICAgIG9ialsibSIgKyBpZF0ud2lkdGggPWl3Ow0KICAgICAgICBvYmpbIm0iICsgaWRdLmhlaWdodCA9aWg7DQogICAgfQ0KICAgIGlmIChyZWxhZnJvbS5sZW5ndGggIT09IDApIHsNCiAgICAgICAgb2JqWyJtIiArIGlkXS5yZWxhZnJvbSA9IHt9Ow0KICAgICAgICBmb3IgKHZhciBpID0gMDsgaSA8IHJlbGFmcm9tLmxlbmd0aDsgaSsrKSB7DQogICAgICAgICAgICB2YXIgaW5kZXhpZCA9IHJlbGFmcm9tW2ldLmlkOw0KICAgICAgICAgICAgdmFyIHRvaWQgPSBkb2N1bWVudC5nZXRFbGVtZW50QnlJZChpbmRleGlkKS5nZXRBdHRyaWJ1dGUoJ2VkOnRvaWQnKTsNCiAgICAgICAgICAgIGlmIChleHRyYVJlbGFbaW5kZXhpZF0gPT09IHVuZGVmaW5lZCkgew0KICAgICAgICAgICAgICAgIGV4dHJhUmVsYVtpbmRleGlkXSA9IHsNCiAgICAgICAgICAgICAgICAgICAgaWQ6IGluZGV4aWQsDQogICAgICAgICAgICAgICAgICAgIGZyb21pZDogaWQsDQogICAgICAgICAgICAgICAgICAgIHRvaWQ6IHRvaWQsDQogICAgICAgICAgICAgICAgICAgIGlzQzogZmFsc2UNCiAgICAgICAgICAgICAgICB9Ow0KICAgICAgICAgICAgfQ0KICAgICAgICAgICAgb2JqWyJtIiArIGlkXS5yZWxhZnJvbVtpbmRleGlkXT17fTsNCiAgICAgICAgICAgIG9ialsibSIgKyBpZF0ucmVsYWZyb20uZGlzcGxheT1kb2N1bWVudC5nZXRFbGVtZW50QnlJZChpZCkuc3R5bGUuZGlzcGxheSAhPT0gJ25vbmUnPydibG9jayc6J25vbmUnOw0KICAgICAgICB9DQogICAgfQ0KICAgIGlmIChyZWxhdG8ubGVuZ3RoICE9PSAwKSB7DQogICAgICAgIG9ialsibSIgKyBpZF0ucmVsYXRvID0ge307DQogICAgICAgIGZvciAodmFyIGkgPSAwOyBpIDwgcmVsYXRvLmxlbmd0aDsgaSsrKSB7DQogICAgICAgICAgICB2YXIgaW5kZXhpZD1yZWxhdG9baV0uaWQ7DQogICAgICAgICAgICB2YXIgZnJvbWlkPWRvY3VtZW50LmdldEVsZW1lbnRCeUlkKGluZGV4aWQpLmdldEF0dHJpYnV0ZSgnZWQ6ZnJvbWlkJyk7DQogICAgICAgICAgICBpZihleHRyYVJlbGFbaW5kZXhpZF0gPT09IHVuZGVmaW5lZCl7DQogICAgICAgICAgICAgICAgZXh0cmFSZWxhW2luZGV4aWRdPXsNCiAgICAgICAgICAgICAgICAgICAgaWQ6aW5kZXhpZCwNCiAgICAgICAgICAgICAgICAgICAgZnJvbWlkOmZyb21pZCwNCiAgICAgICAgICAgICAgICAgICAgdG9pZDppZCwNCiAgICAgICAgICAgICAgICAgICAgaXNDOmZhbHNlDQogICAgICAgICAgICAgICAgfTsNCiAgICAgICAgICAgIH0NCiAgICAgICAgICAgIG9ialsibSIgKyBpZF0ucmVsYXRvW2luZGV4aWRdPXt9Ow0KICAgICAgICAgICAgb2JqWyJtIiArIGlkXS5yZWxhdG8uZGlzcGxheT1kb2N1bWVudC5nZXRFbGVtZW50QnlJZChpZCkuc3R5bGUuZGlzcGxheSAhPT0gJ25vbmUnPydibG9jayc6J25vbmUnOw0KICAgICAgICB9DQogICAgfQ0KICAgIGlmIChjaGlsZHMubGVuZ3RoICE9PSAwKSB7DQogICAgICAgIG9ialsibSIgKyBpZF0uY2hpbGQgPSB7fTsNCiAgICAgICAgaWYgKGRvY3VtZW50LnF1ZXJ5U2VsZWN0b3IoImdbZWRcXDp0b2d0b3BpY2lkPSciICsgaWQgKyAiJ10iKSkgew0KICAgICAgICAgICAgLy8gY29uc29sZS5sb2coZG9jdW1lbnQucXVlcnlTZWxlY3RvcigiZ1tlZFxcOnRvZ3RvcGljaWQ9JyIgKyBpZCArICInXSIpLmNoaWxkTm9kZXNbMF0uZ2V0QXR0cmlidXRlKCd4bGluazpocmVmJykpOw0KICAgICAgICAgICAgdmFyIHRvZyA9IGRvY3VtZW50LnF1ZXJ5U2VsZWN0b3IoImdbZWRcXDp0b2d0b3BpY2lkPSciICsgaWQgKyAiJ10iKS5nZXRFbGVtZW50c0J5VGFnTmFtZSgndXNlJylbMF0uZ2V0QXR0cmlidXRlKCd4bGluazpocmVmJykuc2xpY2UoMSk7DQogICAgICAgICAgICBvYmpbIm0iICsgaWRdLnRvZ3R5cGUgPSB0b2c7DQogICAgICAgIH0NCiAgICAgICAgZm9yICh2YXIgaSA9IDA7IGkgPCBjaGlsZHMubGVuZ3RoOyBpKyspIHsNCiAgICAgICAgICAgIHZhciBjaWQgPSBjaGlsZHNbaV0uaWQ7DQogICAgICAgICAgICBpbml0KGNpZCwgb2JqWyJtIiArIGlkXS5jaGlsZCk7DQogICAgICAgIH0NCiAgICB9DQogICAgaWYgKGNhbGxzLmxlbmd0aCAhPT0gMCkgew0KICAgICAgICBvYmpbIm0iICsgaWRdLmNhbGwgPSB7fTsNCiAgICAgICAgZm9yICh2YXIgaSA9IDA7IGkgPCBjYWxscy5sZW5ndGg7IGkrKykgew0KICAgICAgICAgICAgdmFyIGNpZCA9IGNhbGxzW2ldLmlkOw0KICAgICAgICAgICAgaW5pdChjaWQsIG9ialsibSIgKyBpZF0uY2FsbCk7DQogICAgICAgIH0NCiAgICB9DQogICAgaWYgKGJvdW5kYXJ5Lmxlbmd0aCAhPT0gMCkgew0KICAgICAgICBvYmpbIm0iICsgaWRdLmJvdW5kYXJ5ID0ge307DQogICAgICAgIGZvciAodmFyIGkgPSAwOyBpIDwgYm91bmRhcnkubGVuZ3RoOyBpKyspIHsNCiAgICAgICAgICAgIHZhciBjaWQgPWJvdW5kYXJ5W2ldLmlkOw0KICAgICAgICAgICAgaW5pdChjaWQsIG9ialsibSIgKyBpZF0uYm91bmRhcnkpOw0KICAgICAgICB9DQogICAgfQ0KICAgIGlmIChzdW1tYXJ5Lmxlbmd0aCAhPT0gMCkgew0KICAgICAgICBvYmpbIm0iICsgaWRdLnN1bW1hcnkgPSB7fTsNCiAgICAgICAgZm9yICh2YXIgaSA9IDA7IGkgPCBzdW1tYXJ5Lmxlbmd0aDsgaSsrKSB7DQogICAgICAgICAgICB2YXIgY2lkID0gc3VtbWFyeVtpXS5pZDsNCiAgICAgICAgICAgIGluaXQoY2lkLCBvYmpbIm0iICsgaWRdLnN1bW1hcnkpOw0KICAgICAgICB9DQogICAgfQ0KfQ0KLy8tLWluaXRpYWxpemUNCi8vdG9nZ2xlZGlzcGxheS0tDQp2YXIgY2hhaW5BcnI9W107DQpmdW5jdGlvbiBnZXRjaGFpbihpZCl7DQogICAgY2hhaW5BcnIudW5zaGlmdCgnbScraWQpOw0KICAgIHZhciBwYXJlbnQ9ZG9jdW1lbnQuZ2V0RWxlbWVudEJ5SWQoaWQpLmdldEF0dHJpYnV0ZSgnZWQ6cGFyZW50aWQnKTsNCiAgICBpZighcGFyZW50KXsNCiAgICAgICAgcmV0dXJuOw0KICAgIH0NCglpZihwYXJlbnQubWF0Y2goL1wsLykpew0KICAgICAgICBwYXJlbnQgPSBwYXJlbnQubWF0Y2goL1xkKyg/PVwsKS8pWzBdDQogICAgfQ0KICAgIGdldGNoYWluKHBhcmVudCk7DQp9DQpmdW5jdGlvbiBnZXRvYmooaWQpIHsNCiAgICBjaGFpbkFycj1bXTsNCiAgICBnZXRjaGFpbihpZCk7DQogICAgdmFyIG1haW49Y2hhaW5BcnJbMF07DQogICAgaWYoY2hhaW5BcnIubGVuZ3RoPjEpew0KICAgICAgICB2YXIgb2JqPWRhdGFUcmVlW21haW5dOw0KICAgICAgICAvLyBjb25zb2xlLmxvZyhjaGFpbkFycik7DQogICAgICAgIGZvcih2YXIgaT0xO2k8Y2hhaW5BcnIubGVuZ3RoO2krKykgew0KICAgICAgICAgICAgdmFyIGEgPSBjaGFpbkFycltpXTsNCiAgICAgICAgICAgIGZvcih2YXIgaj0wO2o8T2JqZWN0LmtleXMob2JqKS5sZW5ndGg7aisrKXsNCiAgICAgICAgICAgICAgICB2YXIgY29iaj0gb2JqW09iamVjdC5rZXlzKG9iailbal1dW2FdOw0KICAgICAgICAgICAgICAgIGlmKGNvYmopew0KICAgICAgICAgICAgICAgICAgICBvYmo9Y29iajsNCiAgICAgICAgICAgICAgICAgICAgY29udGludWUNCiAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICB9DQogICAgICAgIH0NCiAgICAgICAgcmV0dXJuIG9iag0KICAgIH1lbHNlew0KICAgICAgICB2YXIgb2JqPWRhdGFUcmVlW21haW5dOw0KICAgICAgICByZXR1cm4gb2JqDQogICAgfQ0KDQp9DQpmb3IodmFyIGk9MDtpPHNoYXBlLmxlbmd0aDtpKyspew0KICAgIHNoYXBlW2ldLm9uY2xpY2s9ZnVuY3Rpb24gKCkgew0KICAgICAgICB2YXIgaWQ9TnVtYmVyKHRoaXMuZ2V0QXR0cmlidXRlKCdlZDp0b2d0b3BpY2lkJykpOw0KICAgICAgICB2YXIgb2JqPWdldG9iaihpZCk7DQoNCiAgICAgICAgdmFyIHR5cGU9b2JqLnRvZ3R5cGU9PT0nbWludXMnPydwbHVzJzonbWludXMnOw0KICAgICAgICB2YXIgZGlzcGxheT1vYmoudG9ndHlwZT09PSdtaW51cyc/J25vbmUnOidibG9jayc7DQogICAgICAgIHRoaXMuZ2V0RWxlbWVudHNCeVRhZ05hbWUoJ3VzZScpWzBdLnNldEF0dHJpYnV0ZSgneGxpbms6aHJlZicsJyMnK3R5cGUpOw0KICAgICAgICBvYmoudG9ndHlwZT10eXBlOw0KICAgICAgICBjaGVja0lEPW9iajsNCg0KICAgICAgICB1dGQob2JqLGlkLGRpc3BsYXkpOw0KICAgICAgICBleHRyYVJlbGFGaW4oKTsNCiAgICB9DQp9DQpmdW5jdGlvbiB1dGQob2JqLGlkLHNob3csb2MpIHsNCg0KICAgIHZhciBwc2hvdz1kb2N1bWVudC5nZXRFbGVtZW50QnlJZChpZCkuc3R5bGUuZGlzcGxheSE9PSAnbm9uZSc/J2Jsb2NrJzonbm9uZSc7DQogICAgaWYgKG9iai5yZWxhZnJvbSl7DQogICAgICAgIGlmKG9iai5yZWxhZnJvbS5kaXNwbGF5IT09IHBzaG93KXsNCiAgICAgICAgICAgIHZhciByZWxhZnJvbXM9T2JqZWN0LmtleXMob2JqLnJlbGFmcm9tKTsNCiAgICAgICAgICAgIHJlbGFmcm9tcy5zcGxpY2UocmVsYWZyb21zLmluZGV4T2YoJ2Rpc3BsYXknKSwxKTsNCiAgICAgICAgICAgIGZvcih2YXIgaz0wO2s8cmVsYWZyb21zLmxlbmd0aDtrKyspew0KICAgICAgICAgICAgICAgIHZhciBkPXJlbGFmcm9tc1trXTsNCiAgICAgICAgICAgICAgICBleHRyYVJlbGFbZF0uaXNDPXRydWU7DQogICAgICAgICAgICB9DQogICAgICAgICAgICBvYmoucmVsYWZyb20uZGlzcGxheSA9IHBzaG93Ow0KICAgICAgICB9DQogICAgfQ0KICAgIGlmIChvYmoucmVsYXRvKXsNCiAgICAgICAgaWYob2JqLnJlbGF0by5kaXNwbGF5IT09IHBzaG93KXsNCiAgICAgICAgICAgIHZhciByZWxhdG9zPU9iamVjdC5rZXlzKG9iai5yZWxhdG8pOw0KICAgICAgICAgICAgcmVsYXRvcy5zcGxpY2UocmVsYXRvcy5pbmRleE9mKCdkaXNwbGF5JyksMSk7DQogICAgICAgICAgICBmb3IodmFyIGs9MDtrPHJlbGF0b3MubGVuZ3RoO2srKyl7DQogICAgICAgICAgICAgICAgdmFyIGQ9cmVsYXRvc1trXTsNCiAgICAgICAgICAgICAgICBleHRyYVJlbGFbZF0uaXNDPXRydWU7DQogICAgICAgICAgICB9DQogICAgICAgICAgICBvYmoucmVsYXRvLmRpc3BsYXkgPSBwc2hvdzsNCiAgICAgICAgfQ0KICAgIH0NCiAgICBpZihvYmouY2FsbCl7DQogICAgICAgIHZhciBjYWxscz1PYmplY3Qua2V5cyhvYmouY2FsbCk7DQogICAgICAgIGlmKGNoZWNrSUQhPT1vYmopew0KICAgICAgICAgICAgZm9yKHZhciBpPTA7aSA8IGNhbGxzLmxlbmd0aDtpKyspew0KICAgICAgICAgICAgICAgIHZhciBhPWNhbGxzW2ldLnNsaWNlKDEpOw0KICAgICAgICAgICAgICAgIHZhciBiPW9iai5jYWxsW2NhbGxzW2ldXTsNCiAgICAgICAgICAgICAgICB2YXIgYz1iLnRvZ3R5cGU7DQogICAgICAgICAgICAgICAgZG9jdW1lbnQuZ2V0RWxlbWVudEJ5SWQoYSkuc3R5bGUuZGlzcGxheT1zaG93Ow0KICAgICAgICAgICAgICAgIGlmIChiLnJlbGFmcm9tJiYhYyl7DQogICAgICAgICAgICAgICAgICAgIGlmKGIucmVsYWZyb20uZGlzcGxheSE9PSBzaG93KXsNCiAgICAgICAgICAgICAgICAgICAgICAgIHZhciByZWxhZnJvbXM9T2JqZWN0LmtleXMoYi5yZWxhZnJvbSk7DQogICAgICAgICAgICAgICAgICAgICAgICByZWxhZnJvbXMuc3BsaWNlKHJlbGFmcm9tcy5pbmRleE9mKCdkaXNwbGF5JyksMSk7DQogICAgICAgICAgICAgICAgICAgICAgICBmb3IodmFyIGs9MDtrPHJlbGFmcm9tcy5sZW5ndGg7aysrKXsNCiAgICAgICAgICAgICAgICAgICAgICAgICAgICB2YXIgZD1yZWxhZnJvbXNba107DQogICAgICAgICAgICAgICAgICAgICAgICAgICAgZXh0cmFSZWxhW2RdLmlzQz10cnVlOw0KICAgICAgICAgICAgICAgICAgICAgICAgfQ0KICAgICAgICAgICAgICAgICAgICAgICAgYi5yZWxhZnJvbS5kaXNwbGF5ID0gc2hvdzsNCiAgICAgICAgICAgICAgICAgICAgfQ0KICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgICAgICBpZiAoYi5yZWxhdG8mJiFjKXsNCiAgICAgICAgICAgICAgICAgICAgaWYoYi5yZWxhdG8uZGlzcGxheSE9PSBzaG93KXsNCiAgICAgICAgICAgICAgICAgICAgICAgIHZhciByZWxhdG9zPU9iamVjdC5rZXlzKGIucmVsYXRvKTsNCiAgICAgICAgICAgICAgICAgICAgICAgIHJlbGF0b3Muc3BsaWNlKHJlbGF0b3MuaW5kZXhPZignZGlzcGxheScpLDEpOw0KICAgICAgICAgICAgICAgICAgICAgICAgZm9yKHZhciBrPTA7azxyZWxhdG9zLmxlbmd0aDtrKyspew0KICAgICAgICAgICAgICAgICAgICAgICAgICAgIHZhciBkPXJlbGF0b3Nba107DQogICAgICAgICAgICAgICAgICAgICAgICAgICAgZXh0cmFSZWxhW2RdLmlzQz10cnVlOw0KICAgICAgICAgICAgICAgICAgICAgICAgfQ0KICAgICAgICAgICAgICAgICAgICAgICAgYi5yZWxhdG8uZGlzcGxheSA9IHNob3c7DQogICAgICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgaWYoYyl7DQogICAgICAgICAgICAgICAgICAgIGRvY3VtZW50LnF1ZXJ5U2VsZWN0b3IoImdbZWRcXDp0b2d0b3BpY2lkPSciK2ErIiddIikuc3R5bGUuZGlzcGxheT1zaG93Ow0KICAgICAgICAgICAgICAgICAgICBpZihjPT09J21pbnVzJyl7DQogICAgICAgICAgICAgICAgICAgICAgICB1dGQoYixhLHNob3cpDQogICAgICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgICAgICAgICAgaWYgKChiLmNhbGx8fGIuYm91bmRhcnl8fGIuc3VtbWFyeSkmJmM9PT0ncGx1cycpIHsNCiAgICAgICAgICAgICAgICAgICAgICAgIHV0ZChiLGEsc2hvdyx0cnVlKQ0KICAgICAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgfQ0KICAgICAgICAgICAgICAgIGlmKGIuY2FsbCYmIWMpIHsNCiAgICAgICAgICAgICAgICAgICAgdXRkKGIsYSxzaG93LHRydWUpDQogICAgICAgICAgICAgICAgfQ0KICAgICAgICAgICAgICAgIGlmIChiLnN1bW1hcnkmJiFjKSB7DQogICAgICAgICAgICAgICAgICAgIHV0ZChiLGEsc2hvdykNCiAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgaWYgKGIuYm91bmRhcnkmJiFjKSB7DQogICAgICAgICAgICAgICAgICAgIHV0ZChiLGEsc2hvdykNCiAgICAgICAgICAgICAgICB9DQoNCiAgICAgICAgICAgIH0NCiAgICAgICAgfQ0KICAgIH0NCiAgICBpZihvYmouc3VtbWFyeSl7DQogICAgICAgIHZhciBzdW1tYXJ5cz1PYmplY3Qua2V5cyhvYmouc3VtbWFyeSk7DQogICAgICAgIGlmKChjaGVja0lEIT09b2JqJiYob2JqLnRvZ3R5cGU9PT0nbWludXMnfHwhb2JqLnRvZ3R5cGUpKXx8Y2hlY2tJRD09PW9iail7DQogICAgICAgICAgICBmb3IodmFyIGk9MDtpPHN1bW1hcnlzLmxlbmd0aDtpKyspew0KICAgICAgICAgICAgICAgIHZhciBhPXN1bW1hcnlzW2ldLnNsaWNlKDEpOw0KCQkJCXZhciBvc3AgPSBkb2N1bWVudC5nZXRFbGVtZW50QnlJZChhKS5nZXRBdHRyaWJ1dGUoJ2VkOnBhcmVudGlkJyk7DQogICAgICAgICAgICAgICAgaWYob3NwLm1hdGNoKC9cLC8pKXsNCiAgICAgICAgICAgICAgICAgICAgdmFyIG9zcGEgPSBvc3Auc3BsaXQoJywnKTsNCiAgICAgICAgICAgICAgICAgICAgdmFyIG9zcEw9MDsNCg0KICAgICAgICAgICAgICAgICAgICBmb3IodmFyIGo9MDtqPG9zcGEubGVuZ3RoO2orKyl7DQogICAgICAgICAgICAgICAgICAgICAgICBpZihzaG93ID09ICdub25lJyl7DQogICAgICAgICAgICAgICAgICAgICAgICAgICAgaWYoZG9jdW1lbnQuZ2V0RWxlbWVudEJ5SWQob3NwYVtqXSkuc3R5bGUuZGlzcGxheSAhPSAnbm9uZScpew0KICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBicmVhazsNCiAgICAgICAgICAgICAgICAgICAgICAgICAgICB9ZWxzZXsNCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgb3NwTCsrOw0KICAgICAgICAgICAgICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgICAgICAgICAgICAgIH1lbHNlew0KICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBpZihkb2N1bWVudC5nZXRFbGVtZW50QnlJZChhKS5zdHlsZS5kaXNwbGF5ICE9ICdub25lJyl7DQogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBicmVhazsNCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgfWVsc2V7DQogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBvc3BMKys7DQogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgICAgICAgICAgICAgIH0NCg0KICAgICAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgICAgIGlmKG9zcEwgIT09IG9zcGEubGVuZ3RoKXsNCiAgICAgICAgICAgICAgICAgICAgICAgIGNvbnRpbnVlOw0KICAgICAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgfQ0KICAgICAgICAgICAgICAgIHZhciBiPW9iai5zdW1tYXJ5W3N1bW1hcnlzW2ldXTsNCiAgICAgICAgICAgICAgICAvLyBjb25zb2xlLmxvZyhhKTsNCiAgICAgICAgICAgICAgICBkb2N1bWVudC5nZXRFbGVtZW50QnlJZChhKS5zdHlsZS5kaXNwbGF5PXNob3c7DQovLyAgICAgICAgICAgICAgICBpZihjKXsNCi8vICAgICAgICAgICAgICAgICAgICBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCJnW2VkXFw6dG9ndG9waWNpZD0nIithKyInXSIpLnN0eWxlLmRpc3BsYXk9c2hvdzsNCi8vICAgICAgICAgICAgICAgICAgICBpZihjPT09J21pbnVzJyl7DQovLyAgICAgICAgICAgICAgICAgICAgICAgIHV0ZChiLHNob3cpDQovLyAgICAgICAgICAgICAgICAgICAgfQ0KLy8gICAgICAgICAgICAgICAgICAgIGlmIChiLmNhbGwmJmM9PT0ncGx1cycpIHsNCi8vICAgICAgICAgICAgICAgICAgICAgICAgdXRkKGIsc2hvdyx0cnVlKQ0KLy8gICAgICAgICAgICAgICAgICAgIH0NCi8vICAgICAgICAgICAgICAgIH0NCi8vICAgICAgICAgICAgICAgIGlmKGIuY2FsbCYmIWMpIHsNCi8vICAgICAgICAgICAgICAgICAgICB1dGQoYixzaG93LHRydWUpDQovLyAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgaWYoT2JqZWN0LmtleXMoYikubGVuZ3RoIT09MCl7DQogICAgICAgICAgICAgICAgICAgIHV0ZChiLGEsc2hvdykNCiAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICB9DQogICAgICAgIH0NCiAgICB9DQogICAgaWYob2JqLmJvdW5kYXJ5KXsNCiAgICAgICAgdmFyIGJvdW5kYXJ5cz1PYmplY3Qua2V5cyhvYmouYm91bmRhcnkpOw0KICAgICAgICBpZihjaGVja0lEIT09b2JqKXsNCiAgICAgICAgICAgIGZvcih2YXIgaT0wO2k8Ym91bmRhcnlzLmxlbmd0aDtpKyspew0KICAgICAgICAgICAgICAgIHZhciBhPWJvdW5kYXJ5c1tpXS5zbGljZSgxKTsNCiAgICAgICAgICAgICAgICB2YXIgYj1vYmouYm91bmRhcnlbYm91bmRhcnlzW2ldXTsNCiAgICAgICAgICAgICAgICAvLyBjb25zb2xlLmxvZyhhKTsNCiAgICAgICAgICAgICAgICBkb2N1bWVudC5nZXRFbGVtZW50QnlJZChhKS5zdHlsZS5kaXNwbGF5PXNob3c7DQovLyAgICAgICAgICAgICAgICBpZihjKXsNCi8vICAgICAgICAgICAgICAgICAgICBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCJnW2VkXFw6dG9ndG9waWNpZD0nIithKyInXSIpLnN0eWxlLmRpc3BsYXk9c2hvdzsNCi8vICAgICAgICAgICAgICAgICAgICBpZihjPT09J21pbnVzJyl7DQovLyAgICAgICAgICAgICAgICAgICAgICAgIHV0ZChiLHNob3cpDQovLyAgICAgICAgICAgICAgICAgICAgfQ0KLy8gICAgICAgICAgICAgICAgICAgIGlmIChiLmNhbGwmJmM9PT0ncGx1cycpIHsNCi8vICAgICAgICAgICAgICAgICAgICAgICAgdXRkKGIsc2hvdyx0cnVlKQ0KLy8gICAgICAgICAgICAgICAgICAgIH0NCi8vICAgICAgICAgICAgICAgIH0NCi8vICAgICAgICAgICAgICAgIGlmKGIuY2FsbCYmIWMpIHsNCi8vICAgICAgICAgICAgICAgICAgICB1dGQoYixzaG93LHRydWUpDQovLyAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgaWYoT2JqZWN0LmtleXMoYikubGVuZ3RoIT09MCl7DQogICAgICAgICAgICAgICAgICAgIHV0ZChiLGEsc2hvdykNCiAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICB9DQogICAgICAgIH0NCiAgICB9DQogICAgaWYoIW9jJiZvYmouY2hpbGQpIHsNCiAgICAgICAgdmFyIGNoaWxkcyA9IE9iamVjdC5rZXlzKG9iai5jaGlsZCk7DQogICAgICAgIGZvciAodmFyIGkgPSAwOyBpIDwgY2hpbGRzLmxlbmd0aDsgaSsrKSB7DQogICAgICAgICAgICB2YXIgYSA9IGNoaWxkc1tpXS5zbGljZSgxKTsNCiAgICAgICAgICAgIHZhciBiID0gb2JqLmNoaWxkW2NoaWxkc1tpXV07DQogICAgICAgICAgICB2YXIgYyA9IGIudG9ndHlwZTsNCiAgICAgICAgICAgIGRvY3VtZW50LmdldEVsZW1lbnRCeUlkKGEpLnN0eWxlLmRpc3BsYXkgPSBzaG93Ow0KCQkJdmFyIHRTUGF0aCA9IGRvY3VtZW50LnF1ZXJ5U2VsZWN0b3IoInBhdGhbZWRcXDp0b3N1cGVyaWQ9JyIrYSsiJ10iKTsNCiAgICAgICAgICAgIGlmKHRTUGF0aCl7DQogICAgICAgICAgICAgICAgdFNQYXRoLnN0eWxlLmRpc3BsYXkgPSBzaG93Ow0KICAgICAgICAgICAgfQ0KCQkJdmFyIG5vdGVUaXAgPSBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCJnW2VkXFw6bm90ZXRvPSciK2ErIiddIik7DQogICAgICAgICAgICBpZihub3RlVGlwKXsNCiAgICAgICAgICAgICAgICBub3RlVGlwLnN0eWxlLmRpc3BsYXkgPSBzaG93Ow0KICAgICAgICAgICAgfQ0KICAgICAgICAgICAgaWYgKGIucmVsYWZyb20mJiFjKXsNCiAgICAgICAgICAgICAgICBpZihiLnJlbGFmcm9tLmRpc3BsYXkhPT0gc2hvdyl7DQogICAgICAgICAgICAgICAgICAgIHZhciByZWxhZnJvbXM9T2JqZWN0LmtleXMoYi5yZWxhZnJvbSk7DQogICAgICAgICAgICAgICAgICAgIHJlbGFmcm9tcy5zcGxpY2UocmVsYWZyb21zLmluZGV4T2YoJ2Rpc3BsYXknKSwxKTsNCiAgICAgICAgICAgICAgICAgICAgZm9yKHZhciBrPTA7azxyZWxhZnJvbXMubGVuZ3RoO2srKyl7DQogICAgICAgICAgICAgICAgICAgICAgICB2YXIgZD1yZWxhZnJvbXNba107DQogICAgICAgICAgICAgICAgICAgICAgICBleHRyYVJlbGFbZF0uaXNDPXRydWU7DQogICAgICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgICAgICAgICAgYi5yZWxhZnJvbS5kaXNwbGF5ID0gc2hvdzsNCiAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICB9DQogICAgICAgICAgICBpZiAoYi5yZWxhdG8mJiFjKXsNCiAgICAgICAgICAgICAgICBpZihiLnJlbGF0by5kaXNwbGF5IT09IHNob3cpew0KICAgICAgICAgICAgICAgICAgICB2YXIgcmVsYXRvcz1PYmplY3Qua2V5cyhiLnJlbGF0byk7DQogICAgICAgICAgICAgICAgICAgIHJlbGF0b3Muc3BsaWNlKHJlbGF0b3MuaW5kZXhPZignZGlzcGxheScpLDEpOw0KICAgICAgICAgICAgICAgICAgICBmb3IodmFyIGs9MDtrPHJlbGF0b3MubGVuZ3RoO2srKyl7DQogICAgICAgICAgICAgICAgICAgICAgICB2YXIgZD1yZWxhdG9zW2tdOw0KICAgICAgICAgICAgICAgICAgICAgICAgZXh0cmFSZWxhW2RdLmlzQz10cnVlOw0KICAgICAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgICAgIGIucmVsYXRvLmRpc3BsYXkgPSBzaG93Ow0KICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgIH0NCiAgICAgICAgICAgIGlmIChjKSB7DQogICAgICAgICAgICAgICAgZG9jdW1lbnQucXVlcnlTZWxlY3RvcigiZ1tlZFxcOnRvZ3RvcGljaWQ9JyIgKyBhICsgIiddIikuc3R5bGUuZGlzcGxheSA9IHNob3c7DQogICAgICAgICAgICAgICAgaWYgKGMgPT09ICdtaW51cycpIHsNCiAgICAgICAgICAgICAgICAgICAgdXRkKGIsYSxzaG93KQ0KICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgICAgICBpZiAoKGIuY2FsbHx8Yi5ib3VuZGFyeXx8Yi5zdW1tYXJ5KSYmYz09PSdwbHVzJykgew0KICAgICAgICAgICAgICAgICAgICB1dGQoYixhLHNob3csdHJ1ZSkNCiAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICB9DQogICAgICAgICAgICBpZiAoYi5jYWxsJiYhYykgew0KICAgICAgICAgICAgICAgIHV0ZChiLGEsc2hvdyx0cnVlKQ0KICAgICAgICAgICAgfQ0KICAgICAgICAgICAgaWYgKGIuc3VtbWFyeSYmIWMpIHsNCiAgICAgICAgICAgICAgICB1dGQoYixhLHNob3cpDQogICAgICAgICAgICB9DQogICAgICAgICAgICBpZiAoYi5ib3VuZGFyeSYmIWMpIHsNCiAgICAgICAgICAgICAgICB1dGQoYixhLHNob3cpDQogICAgICAgICAgICB9DQogICAgICAgIH0NCiAgICB9DQp9DQoNCmZ1bmN0aW9uIGV4dHJhUmVsYUZpbigpIHsNCiAgICB2YXIgZXh0cmFrZXlzPU9iamVjdC5rZXlzKGV4dHJhUmVsYSk7DQogICAgZm9yKHZhciBpPTA7aTxleHRyYWtleXMubGVuZ3RoO2krKyl7DQogICAgICAgIHZhciBleHRyYU9iaj1leHRyYVJlbGFbZXh0cmFrZXlzW2ldXTsNCiAgICAgICAgaWYoZXh0cmFPYmouaXNDID09PSB0cnVlKXsNCiAgICAgICAgICAgIHZhciBmc2hvdz1kb2N1bWVudC5nZXRFbGVtZW50QnlJZChleHRyYU9iai5mcm9taWQpLnN0eWxlLmRpc3BsYXkgIT09J25vbmUnPyB0cnVlOiBmYWxzZTsNCiAgICAgICAgICAgIHZhciB0c2hvdz1kb2N1bWVudC5nZXRFbGVtZW50QnlJZChleHRyYU9iai50b2lkKS5zdHlsZS5kaXNwbGF5ICE9PSdub25lJz8gdHJ1ZTogZmFsc2U7DQogICAgICAgICAgICBkb2N1bWVudC5nZXRFbGVtZW50QnlJZChleHRyYU9iai5pZCkuc3R5bGUuZGlzcGxheT1mc2hvdyAmJiB0c2hvdz8gJ2Jsb2NrJzogJ25vbmUnOw0KICAgICAgICAgICAgZXh0cmFSZWxhW2V4dHJha2V5c1tpXV0uaXNDID0gZmFsc2U7DQogICAgICAgIH0NCiAgICB9DQp9'))
</script>
</body>
