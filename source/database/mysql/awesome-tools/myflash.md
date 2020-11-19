---
title: 开源工具-MyFlash工具
categories:
    - [技术广角, MySQL]
tags:
    - 紧急救援
    - MyFlash
date: 2020-04-24 10:44:33
---

> 生产中使用binlog2sql较多，但也有缺点例如速度不够快；不支持blob的列，因此使用美团开源的myflash试试。


<!-- MDTOC maxdepth:6 firsth1:1 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

   - [开源工具-MyFlash工具](#开源工具-myflash工具)   
      - [详细说明](#详细说明)   
      - [限制](#限制)   
      - [FAQ](#faq)   
   - [使用实践](#使用实践)   
      - [安装MyFlash](#安装myflash)   
      - [查看帮助](#查看帮助)   
      - [测试使用](#测试使用)   
         - [检查mysql日志参数](#检查mysql日志参数)   
         - [创建测试表](#创建测试表)   
      - [插入测试数据](#插入测试数据)   
      - [回滚](#回滚)   
   - [如何对阿里云RDS进行闪回](#如何对阿里云rds进行闪回)   
      - [阿里云的binlog获取方法](#阿里云的binlog获取方法)   
      - [处理binlog日志的备注信息](#处理binlog日志的备注信息)   
      - [其他方法](#其他方法)   

<!-- /MDTOC -->

## 开源工具-MyFlash工具

MyFlash是由美团点评公司技术工程部开发维护的一个回滚DML操作的工具。该工具通过解析v4版本的binlog，完成回滚操作。相对已有的回滚工具，其增加了更多的过滤选项，让回滚更加容易。 该工具已经在美团点评内部使用

### 详细说明
1. [安装](https://github.com/Meituan-Dianping/MyFlash/blob/master/doc/INSTALL.md)
2. [使用](https://github.com/Meituan-Dianping/MyFlash/blob/master/doc/how_to_use.md)
3. [测试用例](https://github.com/Meituan-Dianping/MyFlash/blob/master/doc/TestCase.md)
### 限制

1. binlog格式必须为row,且binlog_row_image=full
2. 仅支持5.6与5.7
3. 只能回滚DML（增、删、改）
### FAQ
1. 实现的原理是什么？   
- 答：参考文章http://url.cn/5yVTfLY

2. 支持gtid吗？  
- 答：支持。请参考 [使用](./doc/how_to_use.md)

3. 在开启gtid的MySQL server上，应用flashback报错，错误为：ERROR 1782 (HY000) at line 16: @@SESSION.GTID_NEXT cannot be set to ANONYMOUS when @@GLOBAL.GTID_MODE = ON.  ?     
- 答：在导入时加入--skip-gtids
mysqlbinlog --skip-gtids <flashbacklog> | mysql -uxxx -pxxx

4. 如果回滚后的binlog日志尺寸超过20M，在导入时，很耗时。如何处理?      
- 答：参考 [使用](./doc/how_to_use.md) ,搜索maxSplitSize。使用该参数可以对文件进行切片

## 使用实践

### 安装MyFlash

```bash
git clone https://github.com/Meituan-Dianping/MyFlash.git
cd MyFlash
yum install glib2* -y
gcc -w  `pkg-config --cflags --libs glib-2.0` source/binlogParseGlib.c  -o binary/flashback
cp binary/flashback /usr/local/bin/
which flashback
```

### 查看帮助

执行`flashback --help`查看帮助


```bash
[root@node1 MyFlash]# flashback --help
Usage:
  flashback [OPTION?]

Help Options:
  -h, --help                  Show help options

Application Options:
  --databaseNames             databaseName to apply. if multiple, seperate by comma(,)
  --tableNames                tableName to apply. if multiple, seperate by comma(,)
  --start-position            start position
  --stop-position             stop position
  --start-datetime            start time (format %Y-%m-%d %H:%M:%S)
  --stop-datetime             stop time (format %Y-%m-%d %H:%M:%S)
  --sqlTypes                  sql type to filter . support INSERT, UPDATE ,DELETE. if multiple, seperate by comma(,)
  --maxSplitSize              max file size after split, the uint is M
  --binlogFileNames           binlog files to process. if multiple, seperate by comma(,)
  --outBinlogFileNameBase     output binlog file name base
  --logLevel                  log level, available option is debug,warning,error
  --include-gtids             gtids to process
  --exclude-gtids             gtids to skip
```

![](2020-05-08-tec-mysql/20200508165409.jpg)


下面的这些参数是可以任意组合的。

```yaml
1.databaseNames

指定需要回滚的数据库名。多个数据库可以用“,”隔开。如果不指定该参数，相当于指定了所有数据库。

2.tableNames

指定需要回滚的表名。多个表可以用“,”隔开。如果不指定该参数，相当于指定了所有表。

3.start-position

指定回滚开始的位置。如不指定，从文件的开始处回滚。请指定正确的有效的位置，否则无法回滚

4.stop-position

指定回滚结束的位置。如不指定，回滚到文件结尾。请指定正确的有效的位置，否则无法回滚

5.start-datetime

指定回滚的开始时间。注意格式必须是 %Y-%m-%d %H:%M:%S。 如不指定，则不限定时间

6.stop-datetime

指定回滚的结束时间。注意格式必须是 %Y-%m-%d %H:%M:%S。 如不指定，则不限定时间

7.sqlTypes

指定需要回滚的sql类型。目前支持的过滤类型是INSERT, UPDATE ,DELETE。多个类型可以用“,”隔开。

8.maxSplitSize

一旦指定该参数，对文件进行固定尺寸的分割（单位为M），过滤条件有效，但不进行回滚操作。该参数主要用来将大的binlog文件切割，防止单次应用的binlog尺寸过大，对线上造成压力

9.binlogFileNames

指定需要回滚的binlog文件，目前只支持单个文件，后续会增加多个文件支持

10.outBinlogFileNameBase

指定输出的binlog文件前缀，如不指定，则默认为binlog_output_base.flashback

11.logLevel

仅供开发者使用，默认级别为error级别。在生产环境中不要修改这个级别，否则输出过多

12.include-gtids

指定需要回滚的gtid,支持gtid的单个和范围两种形式。

13.exclude-gtids

指定不需要回滚的gtid，用法同include-gtids
```


### 测试使用

#### 检查mysql日志参数

```bash
show variables like 'binlog_format';
show variables like 'binlog_row_image';
```

#### 创建测试表

```sql
create database booboo;
CREATE TABLE `booboo`.`testFlashback2` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `nameShort` varchar(20) DEFAULT NULL,
  `nameLong` varchar(260) DEFAULT NULL,
  `amount` decimal(19,9) DEFAULT NULL,
  `amountFloat` float DEFAULT NULL,
  `amountDouble` double DEFAULT NULL,
  `createDatetime6` datetime(6) DEFAULT NULL,
  `createDatetime` datetime DEFAULT NULL,
  `createTimestamp` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  `nameText` text,
  `nameBlob` blob,
  `nameMedium` mediumtext,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB;
```

### 插入测试数据

```SQL
flush logs
insert into testFlashback2(nameShort,nameLong,amount,amountFloat,amountDouble,createDatetime6,createDatetime,createTimestamp,nameText,nameBlob,nameMedium) values('aaa','bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb',10.5,10.6,10.7,'2017-10-26 10:00:00','2017-10-26 10:00:00','2017-10-26 10:00:00','cccc','dddd','eee');
insert into testFlashback2(nameShort,nameLong,amount,amountFloat,amountDouble,createDatetime6,createDatetime,createTimestamp,nameText,nameBlob,nameMedium) values('aaa','bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb',10.5,10.6,10.7,'2017-10-26 10:00:00','2017-10-26 10:00:00','2017-10-26 10:00:00','cccc','dddd','eee');
flush logs;
```

查看插入的数据

```SQL
root@localhost [booboo]>select * from testFlashback2\G;
*************************** 1. row ***************************
             id: 1
      nameShort: aaa
       nameLong: bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
         amount: 10.500000000
    amountFloat: 10.6
   amountDouble: 10.7
createDatetime6: 2017-10-26 10:00:00.000000
 createDatetime: 2017-10-26 10:00:00
createTimestamp: 2017-10-26 10:00:00
       nameText: cccc
       nameBlob: dddd
     nameMedium: eee
*************************** 2. row ***************************
             id: 2
      nameShort: aaa
       nameLong: bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
         amount: 10.500000000
    amountFloat: 10.6
   amountDouble: 10.7
createDatetime6: 2017-10-26 10:00:00.000000
 createDatetime: 2017-10-26 10:00:00
createTimestamp: 2017-10-26 10:00:00
       nameText: cccc
       nameBlob: dddd
     nameMedium: eee
2 rows in set (0.00 sec)

ERROR:
No query specified
```

### 回滚

1. 找到对应的binlog日志 `/alidata/mysql/log/mysql-bin.000078`

```bash
root@localhost [booboo]>show binary logs;
+------------------+-----------+
| Log_name         | File_size |
+------------------+-----------+
| mysql-bin.000077 |      5991 |
| mysql-bin.000078 |      1348 |
| mysql-bin.000079 |       191 |
+------------------+-----------+
3 rows in set (0.00 sec)

root@localhost [booboo]>exit;
Bye
[root@node1 MyFlash]# cd /alidata/mysql/
bin/           data/          include/       log/           my.cnf         README         share/         support-files/
COPYING        docs/          lib/           man/           mysql-test/    scripts/       sql-bench/     tmp/
[root@node1 MyFlash]# cd /alidata/mysql/log/
mysql-bin.000077  mysql-bin.000078  mysql-bin.000079  mysql-bin.index   test.sql
[root@node1 MyFlash]# cd /alidata/mysql/log/
[root@node1 log]# ll
总用量 24
-rw-rw----. 1 mysql mysql 5991 5月   8 17:08 mysql-bin.000077
-rw-rw----. 1 mysql mysql 1348 5月   8 17:09 mysql-bin.000078
-rw-rw----. 1 mysql mysql  191 5月   8 17:09 mysql-bin.000079
-rw-rw----. 1 mysql mysql  108 5月   8 17:09 mysql-bin.index
[root@node1 log]# mysqlbinlog -v -v  mysql-bin.000078
/*!50530 SET @@SESSION.PSEUDO_SLAVE_MODE=1*/;
/*!40019 SET @@session.max_insert_delayed_threads=0*/;
/*!50003 SET @OLD_COMPLETION_TYPE=@@COMPLETION_TYPE,COMPLETION_TYPE=0*/;
DELIMITER /*!*/;
# at 4
#200508 17:08:40 server id 1003306  end_log_pos 120 CRC32 0x2b689980 	Start: binlog v 4, server v 5.6.45-log created 200508 17:08:40
BINLOG '
mCG1Xg8qTw8AdAAAAHgAAAAAAAQANS42LjQ1LWxvZwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAEzgNAAgAEgAEBAQEEgAAXAAEGggAAAAICAgCAAAACgoKGRkAAYCZ
aCs=
'/*!*/;
# at 120
#200508 17:08:40 server id 1003306  end_log_pos 191 CRC32 0x51646940 	Previous-GTIDs
# 8ae1b527-bd2c-11e9-b46d-000c296330ff:1-9
# at 191
#200508 17:09:12 server id 1003306  end_log_pos 239 CRC32 0x285c9356 	GTID [commit=yes]
SET @@SESSION.GTID_NEXT= '8ae1b527-bd2c-11e9-b46d-000c296330ff:10'/*!*/;
# at 239
#200508 17:09:12 server id 1003306  end_log_pos 321 CRC32 0x59cb1159 	Query	thread_id=31	exec_time=0	error_code=0
SET TIMESTAMP=1588928952/*!*/;
SET @@session.pseudo_thread_id=31/*!*/;
SET @@session.foreign_key_checks=1, @@session.sql_auto_is_null=0, @@session.unique_checks=1, @@session.autocommit=1/*!*/;
SET @@session.sql_mode=1075838976/*!*/;
SET @@session.auto_increment_increment=1, @@session.auto_increment_offset=1/*!*/;
/*!\C utf8 *//*!*/;
SET @@session.character_set_client=33,@@session.collation_connection=33,@@session.collation_server=45/*!*/;
SET @@session.time_zone='SYSTEM'/*!*/;
SET @@session.lc_time_names=0/*!*/;
SET @@session.collation_database=DEFAULT/*!*/;
BEGIN
/*!*/;
# at 321
#200508 17:09:12 server id 1003306  end_log_pos 406 CRC32 0x541898f4 	Table_map: `booboo`.`testflashback2` mapped to number 74
# at 406
#200508 17:09:12 server id 1003306  end_log_pos 715 CRC32 0x9df50e18 	Write_rows: table id 74 flags: STMT_END_F

BINLOG '
uCG1XhMqTw8AVQAAAJYBAAAAAEoAAAAAAAEABmJvb2JvbwAOdGVzdGZsYXNoYmFjazIADAMPD/YE
BRISEfz8/A5QABAEEwkECAYAAAICA/4O9JgYVA==
uCG1Xh4qTw8ANQEAAMsCAAAAAEoAAAAAAAEAAgAM//8A8AEAAAADYWFhzQBiYmJiYmJiYmJiYmJi
YmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJi
YmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJi
YmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJi
YmJiYmJiYmJiYmJiYmJiYmJiYmJigAAAAAodzWUAmpkpQWZmZmZmZiVAmZ30oAAAAACZnfSgAFnx
QaAEAGNjY2MEAGRkZGQDAABlZWUYDvWd
'/*!*/;
### INSERT INTO `booboo`.`testflashback2`
### SET
###   @1=1 /* INT meta=0 nullable=0 is_null=0 */
###   @2='aaa' /* VARSTRING(80) meta=80 nullable=1 is_null=0 */
###   @3='bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb' /* VARSTRING(1040) meta=1040 nullable=1 is_null=0 */
###   @4=10.500000000 /* DECIMAL(19,9) meta=4873 nullable=1 is_null=0 */
###   @5=10.6                 /* FLOAT meta=4 nullable=1 is_null=0 */
###   @6=10.699999999999999289 /* DOUBLE meta=8 nullable=1 is_null=0 */
###   @7='2017-10-26 10:00:00.000000' /* DATETIME(6) meta=6 nullable=1 is_null=0 */
###   @8='2017-10-26 10:00:00' /* DATETIME(0) meta=0 nullable=1 is_null=0 */
###   @9=1508983200 /* TIMESTAMP(0) meta=0 nullable=0 is_null=0 */
###   @10='cccc' /* BLOB/TEXT meta=2 nullable=1 is_null=0 */
###   @11='dddd' /* BLOB/TEXT meta=2 nullable=1 is_null=0 */
###   @12='eee' /* MEDIUMBLOB/MEDIUMTEXT meta=3 nullable=1 is_null=0 */
# at 715
#200508 17:09:12 server id 1003306  end_log_pos 746 CRC32 0x637a4759 	Xid = 153
COMMIT/*!*/;
# at 746
#200508 17:09:12 server id 1003306  end_log_pos 794 CRC32 0x6df52572 	GTID [commit=yes]
SET @@SESSION.GTID_NEXT= '8ae1b527-bd2c-11e9-b46d-000c296330ff:11'/*!*/;
# at 794
#200508 17:09:12 server id 1003306  end_log_pos 876 CRC32 0x6944db63 	Query	thread_id=31	exec_time=0	error_code=0
SET TIMESTAMP=1588928952/*!*/;
BEGIN
/*!*/;
# at 876
#200508 17:09:12 server id 1003306  end_log_pos 961 CRC32 0xb3177cd9 	Table_map: `booboo`.`testflashback2` mapped to number 74
# at 961
#200508 17:09:12 server id 1003306  end_log_pos 1270 CRC32 0x47f9804d 	Write_rows: table id 74 flags: STMT_END_F

BINLOG '
uCG1XhMqTw8AVQAAAMEDAAAAAEoAAAAAAAEABmJvb2JvbwAOdGVzdGZsYXNoYmFjazIADAMPD/YE
BRISEfz8/A5QABAEEwkECAYAAAICA/4O2XwXsw==
uCG1Xh4qTw8ANQEAAPYEAAAAAEoAAAAAAAEAAgAM//8A8AIAAAADYWFhzQBiYmJiYmJiYmJiYmJi
YmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJi
YmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJi
YmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJi
YmJiYmJiYmJiYmJiYmJiYmJiYmJigAAAAAodzWUAmpkpQWZmZmZmZiVAmZ30oAAAAACZnfSgAFnx
QaAEAGNjY2MEAGRkZGQDAABlZWVNgPlH
'/*!*/;
### INSERT INTO `booboo`.`testflashback2`
### SET
###   @1=2 /* INT meta=0 nullable=0 is_null=0 */
###   @2='aaa' /* VARSTRING(80) meta=80 nullable=1 is_null=0 */
###   @3='bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb' /* VARSTRING(1040) meta=1040 nullable=1 is_null=0 */
###   @4=10.500000000 /* DECIMAL(19,9) meta=4873 nullable=1 is_null=0 */
###   @5=10.6                 /* FLOAT meta=4 nullable=1 is_null=0 */
###   @6=10.699999999999999289 /* DOUBLE meta=8 nullable=1 is_null=0 */
###   @7='2017-10-26 10:00:00.000000' /* DATETIME(6) meta=6 nullable=1 is_null=0 */
###   @8='2017-10-26 10:00:00' /* DATETIME(0) meta=0 nullable=1 is_null=0 */
###   @9=1508983200 /* TIMESTAMP(0) meta=0 nullable=0 is_null=0 */
###   @10='cccc' /* BLOB/TEXT meta=2 nullable=1 is_null=0 */
###   @11='dddd' /* BLOB/TEXT meta=2 nullable=1 is_null=0 */
###   @12='eee' /* MEDIUMBLOB/MEDIUMTEXT meta=3 nullable=1 is_null=0 */
# at 1270
#200508 17:09:12 server id 1003306  end_log_pos 1301 CRC32 0xaf0eaeb3 	Xid = 154
COMMIT/*!*/;
SET @@SESSION.GTID_NEXT= 'AUTOMATIC' /* added by mysqlbinlog */ /*!*/;
# at 1301
#200508 17:09:13 server id 1003306  end_log_pos 1348 CRC32 0x53ecb2a7 	Rotate to mysql-bin.000079  pos: 4
DELIMITER ;
# End of log file
ROLLBACK /* added by mysqlbinlog */;
/*!50003 SET COMPLETION_TYPE=@OLD_COMPLETION_TYPE*/;
/*!50530 SET @@SESSION.PSEUDO_SLAVE_MODE=0*/;
```
2. 执行生产flashback文件 `flashback --binlogFileNames=/alidata/mysql/log/mysql-bin.000078`

```bash
[root@node1 log]# flashback --binlogFileNames=/alidata/mysql/log/mysql-bin.000078
[root@node1 log]# ll
总用量 28
-rw-r--r--. 1 root  root   970 5月   8 17:19 binlog_output_base.flashback
-rw-rw----. 1 mysql mysql 5991 5月   8 17:08 mysql-bin.000077
-rw-rw----. 1 mysql mysql 1348 5月   8 17:09 mysql-bin.000078
-rw-rw----. 1 mysql mysql  191 5月   8 17:09 mysql-bin.000079
-rw-rw----. 1 mysql mysql  108 5月   8 17:09 mysql-bin.index
-rw-r--r--. 1 root  root   795 4月  14 16:23 test.sql
```

3.

```bash
[root@node1 log]# mysqlbinlog binlog_output_base.flashback
/*!50530 SET @@SESSION.PSEUDO_SLAVE_MODE=1*/;
/*!40019 SET @@session.max_insert_delayed_threads=0*/;
/*!50003 SET @OLD_COMPLETION_TYPE=@@COMPLETION_TYPE,COMPLETION_TYPE=0*/;
DELIMITER /*!*/;
# at 4
#200508 17:08:40 server id 1003306  end_log_pos 120 CRC32 0x2b689980 	Start: binlog v 4, server v 5.6.45-log created 200508 17:08:40
BINLOG '
mCG1Xg8qTw8AdAAAAHgAAAAAAAQANS42LjQ1LWxvZwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAEzgNAAgAEgAEBAQEEgAAXAAEGggAAAAICAgCAAAACgoKGRkAAYCZ
aCs=
'/*!*/;
# at 120
#200508 17:09:12 server id 1003306  end_log_pos 205 CRC32 0xb3177cd9 	Table_map: `booboo`.`testflashback2` mapped to number 74
# at 205
#200508 17:09:12 server id 1003306  end_log_pos 514 CRC32 0x47f9804d 	Delete_rows: table id 74 flags: STMT_END_F

BINLOG '
uCG1XhMqTw8AVQAAAM0AAAAAAEoAAAAAAAEABmJvb2JvbwAOdGVzdGZsYXNoYmFjazIADAMPD/YE
BRISEfz8/A5QABAEEwkECAYAAAICA/4O2XwXsw==
uCG1XiAqTw8ANQEAAAICAAAAAEoAAAAAAAEAAgAM//8A8AIAAAADYWFhzQBiYmJiYmJiYmJiYmJi
YmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJi
YmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJi
YmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJi
YmJiYmJiYmJiYmJiYmJiYmJiYmJigAAAAAodzWUAmpkpQWZmZmZmZiVAmZ30oAAAAACZnfSgAFnx
QaAEAGNjY2MEAGRkZGQDAABlZWVNgPlH
'/*!*/;
# at 514
#200508 17:09:12 server id 1003306  end_log_pos 545 CRC32 0x637a4759 	Xid = 153
COMMIT/*!*/;
# at 545
#200508 17:09:12 server id 1003306  end_log_pos 630 CRC32 0x541898f4 	Table_map: `booboo`.`testflashback2` mapped to number 74
# at 630
#200508 17:09:12 server id 1003306  end_log_pos 939 CRC32 0x9df50e18 	Delete_rows: table id 74 flags: STMT_END_F

BINLOG '
uCG1XhMqTw8AVQAAAHYCAAAAAEoAAAAAAAEABmJvb2JvbwAOdGVzdGZsYXNoYmFjazIADAMPD/YE
BRISEfz8/A5QABAEEwkECAYAAAICA/4O9JgYVA==
uCG1XiAqTw8ANQEAAKsDAAAAAEoAAAAAAAEAAgAM//8A8AEAAAADYWFhzQBiYmJiYmJiYmJiYmJi
YmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJi
YmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJi
YmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJi
YmJiYmJiYmJiYmJiYmJiYmJiYmJigAAAAAodzWUAmpkpQWZmZmZmZiVAmZ30oAAAAACZnfSgAFnx
QaAEAGNjY2MEAGRkZGQDAABlZWUYDvWd
'/*!*/;
# at 939
#200508 17:09:12 server id 1003306  end_log_pos 970 CRC32 0x637a4759 	Xid = 153
COMMIT/*!*/;
DELIMITER ;
# End of log file
ROLLBACK /* added by mysqlbinlog */;
/*!50003 SET COMPLETION_TYPE=@OLD_COMPLETION_TYPE*/;
/*!50530 SET @@SESSION.PSEUDO_SLAVE_MODE=0*/;
[root@node1 log]# mysqlbinlog -v -v binlog_output_base.flashback
/*!50530 SET @@SESSION.PSEUDO_SLAVE_MODE=1*/;
/*!40019 SET @@session.max_insert_delayed_threads=0*/;
/*!50003 SET @OLD_COMPLETION_TYPE=@@COMPLETION_TYPE,COMPLETION_TYPE=0*/;
DELIMITER /*!*/;
# at 4
#200508 17:08:40 server id 1003306  end_log_pos 120 CRC32 0x2b689980 	Start: binlog v 4, server v 5.6.45-log created 200508 17:08:40
BINLOG '
mCG1Xg8qTw8AdAAAAHgAAAAAAAQANS42LjQ1LWxvZwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAEzgNAAgAEgAEBAQEEgAAXAAEGggAAAAICAgCAAAACgoKGRkAAYCZ
aCs=
'/*!*/;
# at 120
#200508 17:09:12 server id 1003306  end_log_pos 205 CRC32 0xb3177cd9 	Table_map: `booboo`.`testflashback2` mapped to number 74
# at 205
#200508 17:09:12 server id 1003306  end_log_pos 514 CRC32 0x47f9804d 	Delete_rows: table id 74 flags: STMT_END_F

BINLOG '
uCG1XhMqTw8AVQAAAM0AAAAAAEoAAAAAAAEABmJvb2JvbwAOdGVzdGZsYXNoYmFjazIADAMPD/YE
BRISEfz8/A5QABAEEwkECAYAAAICA/4O2XwXsw==
uCG1XiAqTw8ANQEAAAICAAAAAEoAAAAAAAEAAgAM//8A8AIAAAADYWFhzQBiYmJiYmJiYmJiYmJi
YmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJi
YmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJi
YmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJi
YmJiYmJiYmJiYmJiYmJiYmJiYmJigAAAAAodzWUAmpkpQWZmZmZmZiVAmZ30oAAAAACZnfSgAFnx
QaAEAGNjY2MEAGRkZGQDAABlZWVNgPlH
'/*!*/;
### DELETE FROM `booboo`.`testflashback2`
### WHERE
###   @1=2 /* INT meta=0 nullable=0 is_null=0 */
###   @2='aaa' /* VARSTRING(80) meta=80 nullable=1 is_null=0 */
###   @3='bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb' /* VARSTRING(1040) meta=1040 nullable=1 is_null=0 */
###   @4=10.500000000 /* DECIMAL(19,9) meta=4873 nullable=1 is_null=0 */
###   @5=10.6                 /* FLOAT meta=4 nullable=1 is_null=0 */
###   @6=10.699999999999999289 /* DOUBLE meta=8 nullable=1 is_null=0 */
###   @7='2017-10-26 10:00:00.000000' /* DATETIME(6) meta=6 nullable=1 is_null=0 */
###   @8='2017-10-26 10:00:00' /* DATETIME(0) meta=0 nullable=1 is_null=0 */
###   @9=1508983200 /* TIMESTAMP(0) meta=0 nullable=0 is_null=0 */
###   @10='cccc' /* BLOB/TEXT meta=2 nullable=1 is_null=0 */
###   @11='dddd' /* BLOB/TEXT meta=2 nullable=1 is_null=0 */
###   @12='eee' /* MEDIUMBLOB/MEDIUMTEXT meta=3 nullable=1 is_null=0 */
# at 514
#200508 17:09:12 server id 1003306  end_log_pos 545 CRC32 0x637a4759 	Xid = 153
COMMIT/*!*/;
# at 545
#200508 17:09:12 server id 1003306  end_log_pos 630 CRC32 0x541898f4 	Table_map: `booboo`.`testflashback2` mapped to number 74
# at 630
#200508 17:09:12 server id 1003306  end_log_pos 939 CRC32 0x9df50e18 	Delete_rows: table id 74 flags: STMT_END_F

BINLOG '
uCG1XhMqTw8AVQAAAHYCAAAAAEoAAAAAAAEABmJvb2JvbwAOdGVzdGZsYXNoYmFjazIADAMPD/YE
BRISEfz8/A5QABAEEwkECAYAAAICA/4O9JgYVA==
uCG1XiAqTw8ANQEAAKsDAAAAAEoAAAAAAAEAAgAM//8A8AEAAAADYWFhzQBiYmJiYmJiYmJiYmJi
YmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJi
YmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJi
YmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJiYmJi
YmJiYmJiYmJiYmJiYmJiYmJiYmJigAAAAAodzWUAmpkpQWZmZmZmZiVAmZ30oAAAAACZnfSgAFnx
QaAEAGNjY2MEAGRkZGQDAABlZWUYDvWd
'/*!*/;
### DELETE FROM `booboo`.`testflashback2`
### WHERE
###   @1=1 /* INT meta=0 nullable=0 is_null=0 */
###   @2='aaa' /* VARSTRING(80) meta=80 nullable=1 is_null=0 */
###   @3='bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb' /* VARSTRING(1040) meta=1040 nullable=1 is_null=0 */
###   @4=10.500000000 /* DECIMAL(19,9) meta=4873 nullable=1 is_null=0 */
###   @5=10.6                 /* FLOAT meta=4 nullable=1 is_null=0 */
###   @6=10.699999999999999289 /* DOUBLE meta=8 nullable=1 is_null=0 */
###   @7='2017-10-26 10:00:00.000000' /* DATETIME(6) meta=6 nullable=1 is_null=0 */
###   @8='2017-10-26 10:00:00' /* DATETIME(0) meta=0 nullable=1 is_null=0 */
###   @9=1508983200 /* TIMESTAMP(0) meta=0 nullable=0 is_null=0 */
###   @10='cccc' /* BLOB/TEXT meta=2 nullable=1 is_null=0 */
###   @11='dddd' /* BLOB/TEXT meta=2 nullable=1 is_null=0 */
###   @12='eee' /* MEDIUMBLOB/MEDIUMTEXT meta=3 nullable=1 is_null=0 */
# at 939
#200508 17:09:12 server id 1003306  end_log_pos 970 CRC32 0x637a4759 	Xid = 153
COMMIT/*!*/;
DELIMITER ;
# End of log file
ROLLBACK /* added by mysqlbinlog */;
/*!50003 SET COMPLETION_TYPE=@OLD_COMPLETION_TYPE*/;
/*!50530 SET @@SESSION.PSEUDO_SLAVE_MODE=0*/;
```

4. 导入数据库`mysqlbinlog -v -v binlog_output_base.flashback | mysql -uroot -proot`

```bash
[root@node1 log]# mysqlbinlog -v -v binlog_output_base.flashback | mysql -uroot -proot
Warning: Using a password on the command line interface can be insecure.
```

5. 检查执行结果

```SQL
[root@node1 log]# mysql -uroot -proot -e "select count(*) from booboo.testflashback2"
Warning: Using a password on the command line interface can be insecure.
+----------+
| count(*) |
+----------+
|        0 |
+----------+
```

## 如何对阿里云RDS进行闪回

难点：
1. 阿里云的binlog获取方法
2. 阿里云账号没有Super权限因此需要处理binlog日志删除备注信息。

### 阿里云的binlog获取方法

1. 若binlog日志已上传至oss，则需要在控制台进行下载，下载时需要注意找到主的日志（阿里云RDS日志包含主和被的日志）
2. 若binlog日志还在实例本地，则需要在控制台先点击一键上传，待日志上传到oss后，从控制台下载


### 处理binlog日志的备注信息


```bash
# 5.6版本
mysqlbinlog -v -v --skip-gtids binlog_output_base.flashback | sed  's@\/\*.*\*\/@@' |mysql -uroot -proot
```

### 其他方法

对于阿里云RDS实例，若待回滚的表中，没有blob字段建议使用binlog2sql。

```bash
# 远程在线获取binlog日志
mysqlbinlog -vv -P 3306 -uxxx -pxxxx -hxxxxx.mysql.rds.aliyuncs.com --read-from-remote-server --start-datetime='2017-09-12 06:29:00' --stop-datetime='2017-09-12 06:30:01' --base64-output=DECODE-ROWS mysql-bin.000634 > taiping0912.binlog
# binlog2sql远程连接数据库直接生成回滚sql
python binlog2sql.py -P 3306 -uxxx -pxxxx -hxxxxx.mysql.rds.aliyuncs.com -t tablenamexxx --start-file='mysql-bin.000001' --start-position=4946 --stop-position=5921 -B
```
