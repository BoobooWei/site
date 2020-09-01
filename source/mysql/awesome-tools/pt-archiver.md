---
title: pt-archiver 归档表无主键的问题探索
---

# 问题描述

一张没有主键的表，归档时发现归档后的数据行数不准确，少了很多。

# 场景模拟

## 表结构和数据

准备两张表 table_demo 和 table_demo1 

|表|主键|归档表|
|:--|:--|:--|
|table_demo|Yes|table_demo_history|
|table_demo1|NO|table_demo1_history|


```sql 
CREATE TABLE table_demo(  
id int auto_increment primary key,   
uname  varchar(20) ,  
ucreatetime  datetime  ,  
age  int(11)) DEFAULT CHARACTER SET=utf8 COLLATE=utf8_general_ci;

CREATE TABLE table_demo1(  
id int,   
uname  varchar(20) ,  
ucreatetime  datetime  ,  
age  int(11)) DEFAULT CHARACTER SET=utf8 COLLATE=utf8_general_ci;


alter table table_demo add index idx_ucreatetime (ucreatetime);
alter table table_demo1 add index idx_ucreatetime (ucreatetime);

delimiter $$
create  procedure test1() 
begin
declare v_cnt decimal (10)  default 0 ;
start transaction;
dd:loop           
 
        insert  into table_demo values      
        (null,'用户1',sysdate(),20),        
        (null,'用户2',sysdate(),20),        
        (null,'用户3',sysdate(),20),        
        (null,'用户4',sysdate(),20),        
        (null,'用户5',sysdate(),20),        
        (null,'用户6',sysdate(),20),        
        (null,'用户7',sysdate(),20),        
        (null,'用户8',sysdate(),20),        
        (null,'用户9',sysdate(),20),        
        (null,'用户0',sysdate(),20)            
                ;                                       
        set v_cnt = v_cnt+10 ;                           
            if  v_cnt = 1000 then leave dd;                          
            end if;         
        end loop dd ;
commit;
end $$


delimiter ;
call test1();

insert into table_demo1 select * from table_demo;
select count(*) from table_demo1;
update table_demo1 set ucreatetime="2020-09-02 11:51:27" where id between 200 and 300;
update table_demo1 set ucreatetime="2020-09-03 11:51:27" where id between 300 and 400;
update table_demo1 set ucreatetime="2020-09-04 11:51:27" where id between 400 and 500;
update table_demo1 set ucreatetime="2020-09-05 11:51:27" where id between 500 and 600;
update table_demo1 set ucreatetime="2020-09-06 11:51:27" where id between 600 and 700;
update table_demo1 set ucreatetime="2020-09-07 11:51:27" where id between 700 and 800;
update table_demo1 set ucreatetime="2020-09-08 11:51:27" where id between 800 and 900;
update table_demo1 set ucreatetime="2020-09-09 11:51:27" where id between 900 and 1000;
	
update table_demo set ucreatetime="2020-09-02 11:51:27" where id between 200 and 300;
update table_demo set ucreatetime="2020-09-03 11:51:27" where id between 300 and 400;
update table_demo set ucreatetime="2020-09-04 11:51:27" where id between 400 and 500;
update table_demo set ucreatetime="2020-09-05 11:51:27" where id between 500 and 600;
update table_demo set ucreatetime="2020-09-06 11:51:27" where id between 600 and 700;
update table_demo set ucreatetime="2020-09-07 11:51:27" where id between 700 and 800;
update table_demo set ucreatetime="2020-09-08 11:51:27" where id between 800 and 900;
update table_demo set ucreatetime="2020-09-09 11:51:27" where id between 900 and 1000;


select count(*), ucreatetime from table_demo group by ucreatetime;
select count(*), ucreatetime from table_demo1 group by ucreatetime;

create table table_demo1_history like table_demo1;
create table table_demo_history like table_demo;
create table table_demo1_history1 like table_demo1;
	
truncate table table_demo_history;
truncate table table_demo1_history;
truncate table table_demo1_history1;
```

## 开启general_log

```sql
# 开启general_log，具体操作如下：
show variables like '%general_log%';
SET GLOBAL general_log = 'OFF';
SET GLOBAL general_log = 'ON';
show variables like '%general_log%';
```


## 归档命令

```bash
#  table_demo 有主键
pt-archiver  --source h=localhost,P=3306,u=root,p=root,D=qixin,t=table_demo --dest h=localhost,P=3306,u=root,p=root,D=qixin,t=table_demo_history --no-check-charset --where "ucreatetime >= '2020-09-01 11:51:26'" --progress 5000 --limit=10 --txn-size=10 --statistics --bulk-insert --no-delete --charset 'utf8' --noversion-check
# table_demo1 无主键 --limit 10
pt-archiver  --source h=localhost,P=3306,u=root,p=root,D=qixin,t=table_demo1 --dest h=localhost,P=3306,u=root,p=root,D=qixin,t=table_demo1_history --no-check-charset --where "ucreatetime >= '2020-09-01 11:51:26'" --progress 5000 --limit=10 --txn-size=10 --statistics --bulk-insert --no-delete --charset 'utf8' --noversion-check
# table_demo1 无主键 --limit 500
pt-archiver  --source h=localhost,P=3306,u=root,p=root,D=qixin,t=table_demo1 --dest h=localhost,P=3306,u=root,p=root,D=qixin,t=table_demo1_history1 --no-check-charset --where "ucreatetime >= '2020-09-01 11:51:26'" --progress 5000 --limit=500 --txn-size=10 --statistics --bulk-insert --no-delete --charset 'utf8' --noversion-check
```


## 验证数据


```sql
select count(*), ucreatetime from table_demo_history group by ucreatetime;
select count(*), ucreatetime from table_demo1_history1 group by ucreatetime;
select count(*), ucreatetime from table_demo1_history group by ucreatetime;
```

# 操作记录

```bash
[root@NB-flexgw1:/root]# pt-archiver --version
pt-archiver 3.0.4
[root@NB-flexgw1:/root]# mysql --version
mysql  Ver 14.14 Distrib 5.7.20-19, for Linux (x86_64) using  EditLine wrapper

root@MySQL-01 12:09:  [qixin]> select count(*), ucreatetime from table_demo1 group by ucreatetime;
+----------+---------------------+
| count(*) | ucreatetime         |
+----------+---------------------+
|      199 | 2020-09-01 11:51:26 |
|      100 | 2020-09-02 11:51:27 |
|      100 | 2020-09-03 11:51:27 |
|      100 | 2020-09-04 11:51:27 |
|      100 | 2020-09-05 11:51:27 |
|      100 | 2020-09-06 11:51:27 |
|      100 | 2020-09-07 11:51:27 |
|      100 | 2020-09-08 11:51:27 |
|      101 | 2020-09-09 11:51:27 |
+----------+---------------------+
9 rows in set (0.00 sec)

root@MySQL-01 12:09:  [qixin]> select count(*), ucreatetime from table_demo group by ucreatetime;
+----------+---------------------+
| count(*) | ucreatetime         |
+----------+---------------------+
|      199 | 2020-09-01 11:51:26 |
|      100 | 2020-09-02 11:51:27 |
|      100 | 2020-09-03 11:51:27 |
|      100 | 2020-09-04 11:51:27 |
|      100 | 2020-09-05 11:51:27 |
|      100 | 2020-09-06 11:51:27 |
|      100 | 2020-09-07 11:51:27 |
|      100 | 2020-09-08 11:51:27 |
|      101 | 2020-09-09 11:51:27 |
+----------+---------------------+
9 rows in set (0.00 sec)



root@MySQL-01 12:41:  [qixin]> show variables like '%general_log%';
+------------------+------------------------------------+
| Variable_name    | Value                              |
+------------------+------------------------------------+
| general_log      | ON                                 |
| general_log_file | /alidata/mysql/data/NB-flexgw1.log |
+------------------+------------------------------------+
2 rows in set (0.00 sec)

--无索引的表
[root@NB-flexgw1:/root]# pt-archiver  --source h=localhost,P=3306,u=root,p=root,D=qixin,t=table_demo1 --dest h=localhost,P=3306,u=root,p=root,D=qixin,t=table_demo1_history --no-check-charset --where "ucreatetime >= '2020-09-01 11:51:26'" --progress 5000 --limit=10 --txn-size=10 --statistics --bulk-insert --no-delete --charset 'utf8' --noversion-check
TIME                ELAPSED   COUNT
2020-09-01T12:06:04       0       0
2020-09-01T12:06:05       0      90
Started at 2020-09-01T12:06:04, ended at 2020-09-01T12:06:05
Source: A=utf8,D=qixin,P=3306,h=localhost,p=...,t=table_demo1,u=root
Dest:   A=utf8,D=qixin,P=3306,h=localhost,p=...,t=table_demo1_history,u=root
SELECT 90
INSERT 90
DELETE 0
Action              Count       Time        Pct
commit                 20     0.7429      90.38
select                 10     0.0261       3.18
bulk_inserting          9     0.0030       0.36
print_bulkfile         90    -0.0003      -0.03
other                   0     0.0502       6.11


--有索引的表
[root@NB-flexgw1:/root]# pt-archiver  --source h=localhost,P=3306,u=root,p=root,D=qixin,t=table_demo --dest h=localhost,P=3306,u=root,p=root,D=qixin,t=table_demo_history --no-check-charset --where "ucreatetime >= '2020-09-01 11:51:26'" --progress 5000 --limit=10 --txn-size=10 --statistics --bulk-insert --no-delete --charset 'utf8' --noversion-check
TIME                ELAPSED   COUNT
2020-09-01T12:11:10       0       0
2020-09-01T12:11:18       8     999
Started at 2020-09-01T12:11:10, ended at 2020-09-01T12:11:18
Source: A=utf8,D=qixin,P=3306,h=localhost,p=...,t=table_demo,u=root
Dest:   A=utf8,D=qixin,P=3306,h=localhost,p=...,t=table_demo_history,u=root
SELECT 999
INSERT 999
DELETE 0
Action              Count       Time        Pct
commit                200     8.1391      97.00
bulk_inserting        100     0.1428       1.70
select                101     0.0461       0.55
print_bulkfile        999    -0.0018      -0.02
other                   0     0.0642       0.76

[root@NB-flexgw1:/root]# pt-archiver  --source h=localhost,P=3306,u=root,p=root,D=qixin,t=table_demo1 --dest h=localhost,P=3306,u=root,p=root,D=qixin,t=table_demo1_history1 --no-check-charset --where "ucreatetime >= '2020-09-01 11:51:26'" --progress 5000 --limit=500 --txn-size=10 --statistics --bulk-insert --no-delete --charset 'utf8' --noversion-check
TIME                ELAPSED   COUNT
2020-09-01T15:24:52       0       0
2020-09-01T15:24:52       0     901
Started at 2020-09-01T15:24:52, ended at 2020-09-01T15:24:52
Source: A=utf8,D=qixin,P=3306,h=localhost,p=...,t=table_demo1,u=root
Dest:   A=utf8,D=qixin,P=3306,h=localhost,p=...,t=table_demo1_history1,u=root
SELECT 901
INSERT 901
DELETE 0
Action              Count       Time        Pct
bulk_inserting          2     0.2076      51.39
commit                182     0.1521      37.64
select                  3     0.0189       4.68
print_bulkfile        901    -0.0021      -0.51
other                   0     0.0275       6.80

root@MySQL-01 15:24:  [qixin]> select count(*), ucreatetime from table_demo_history group by ucreatetime;
+----------+---------------------+
| count(*) | ucreatetime         |
+----------+---------------------+
|      199 | 2020-09-01 11:51:26 |
|      100 | 2020-09-02 11:51:27 |
|      100 | 2020-09-03 11:51:27 |
|      100 | 2020-09-04 11:51:27 |
|      100 | 2020-09-05 11:51:27 |
|      100 | 2020-09-06 11:51:27 |
|      100 | 2020-09-07 11:51:27 |
|      100 | 2020-09-08 11:51:27 |
|      100 | 2020-09-09 11:51:27 |
+----------+---------------------+
9 rows in set (0.00 sec)

root@MySQL-01 15:25:  [qixin]> select count(*), ucreatetime from table_demo1_history group by ucreatetime;
+----------+---------------------+
| count(*) | ucreatetime         |
+----------+---------------------+
|       10 | 2020-09-01 11:51:26 |
|       10 | 2020-09-02 11:51:27 |
|       10 | 2020-09-03 11:51:27 |
|       10 | 2020-09-04 11:51:27 |
|       10 | 2020-09-05 11:51:27 |
|       10 | 2020-09-06 11:51:27 |
|       10 | 2020-09-07 11:51:27 |
|       10 | 2020-09-08 11:51:27 |
|       10 | 2020-09-09 11:51:27 |
+----------+---------------------+
9 rows in set (0.00 sec)

root@MySQL-01 15:25:  [qixin]> select count(*), ucreatetime from table_demo1_history1 group by ucreatetime;
+----------+---------------------+
| count(*) | ucreatetime         |
+----------+---------------------+
|      199 | 2020-09-01 11:51:26 |
|      100 | 2020-09-02 11:51:27 |
|      100 | 2020-09-03 11:51:27 |
|      100 | 2020-09-04 11:51:27 |
|        1 | 2020-09-05 11:51:27 |
|      100 | 2020-09-06 11:51:27 |
|      100 | 2020-09-07 11:51:27 |
|      100 | 2020-09-08 11:51:27 |
|      101 | 2020-09-09 11:51:27 |
+----------+---------------------+
9 rows in set (0.00 sec)

```

# 分析PT原理

## 有主键

```bash
2020-09-01T04:42:03.705819Z	1056244 Connect	root@localhost on qixin using Socket
2020-09-01T04:42:03.705930Z	1056244 Query	set autocommit=0
2020-09-01T04:42:03.706025Z	1056244 Query	/*!40101 SET NAMES "utf8"*/
2020-09-01T04:42:03.706128Z	1056244 Query	SHOW VARIABLES LIKE 'wait\_timeout'
2020-09-01T04:42:03.782137Z	1056244 Query	SET SESSION wait_timeout=10000
2020-09-01T04:42:03.782285Z	1056244 Query	SELECT @@SQL_MODE
2020-09-01T04:42:03.786019Z	1056244 Query	SET @@SQL_QUOTE_SHOW_CREATE = 1/*!40101, @@SQL_MODE='NO_AUTO_VALUE_ON_ZERO,ONLY_FULL_GROUP_BY'*/
2020-09-01T04:42:03.786244Z	1056244 Query	SHOW VARIABLES LIKE 'version%'
2020-09-01T04:42:03.787356Z	1056244 Query	SHOW ENGINES
2020-09-01T04:42:03.787601Z	1056244 Query	SHOW VARIABLES LIKE 'innodb_version'
2020-09-01T04:42:03.788640Z	1056244 Query	show variables like 'innodb_rollback_on_timeout'
2020-09-01T04:42:03.789538Z	1056244 Query	/*!40101 SET @OLD_SQL_MODE := @@SQL_MODE, @@SQL_MODE := '', @OLD_QUOTE := @@SQL_QUOTE_SHOW_CREATE, @@SQL_QUOTE_SHOW_CREATE := 1 */
2020-09-01T04:42:03.789604Z	1056244 Query	USE `qixin`
2020-09-01T04:42:03.789675Z	1056244 Query	SHOW CREATE TABLE `qixin`.`table_demo`
2020-09-01T04:42:03.789794Z	1056244 Query	/*!40101 SET @@SQL_MODE := @OLD_SQL_MODE, @@SQL_QUOTE_SHOW_CREATE := @OLD_QUOTE */
2020-09-01T04:42:03.790234Z	1056245 Connect	root@localhost on qixin using Socket
2020-09-01T04:42:03.790302Z	1056245 Query	set autocommit=0
2020-09-01T04:42:03.790368Z	1056245 Query	/*!40101 SET NAMES "utf8"*/
2020-09-01T04:42:03.790443Z	1056245 Query	SHOW VARIABLES LIKE 'wait\_timeout'
2020-09-01T04:42:03.791279Z	1056245 Query	SET SESSION wait_timeout=10000
2020-09-01T04:42:03.791363Z	1056245 Query	SELECT @@SQL_MODE
2020-09-01T04:42:03.791422Z	1056245 Query	SET @@SQL_QUOTE_SHOW_CREATE = 1/*!40101, @@SQL_MODE='NO_AUTO_VALUE_ON_ZERO,ONLY_FULL_GROUP_BY'*/
2020-09-01T04:42:03.791510Z	1056245 Query	SHOW VARIABLES LIKE 'version%'
2020-09-01T04:42:03.792414Z	1056245 Query	SHOW ENGINES
2020-09-01T04:42:03.792651Z	1056245 Query	SHOW VARIABLES LIKE 'innodb_version'
2020-09-01T04:42:03.793621Z	1056245 Query	show variables like 'innodb_rollback_on_timeout'
2020-09-01T04:42:03.794502Z	1056245 Query	/*!40101 SET @OLD_SQL_MODE := @@SQL_MODE, @@SQL_MODE := '', @OLD_QUOTE := @@SQL_QUOTE_SHOW_CREATE, @@SQL_QUOTE_SHOW_CREATE := 1 */
2020-09-01T04:42:03.794564Z	1056245 Query	USE `qixin`
2020-09-01T04:42:03.794632Z	1056245 Query	SHOW CREATE TABLE `qixin`.`table_demo_history`
2020-09-01T04:42:03.794756Z	1056245 Query	/*!40101 SET @@SQL_MODE := @OLD_SQL_MODE, @@SQL_QUOTE_SHOW_CREATE := @OLD_QUOTE */
2020-09-01T04:42:03.794975Z	1056244 Query	SHOW VARIABLES LIKE 'wsrep_on'
2020-09-01T04:42:03.795812Z	1056244 Query	SHOW VARIABLES LIKE 'wsrep_on'
2020-09-01T04:42:03.796814Z	1056244 Query	SHOW VARIABLES LIKE 'version%'
2020-09-01T04:42:03.797717Z	1056244 Query	SHOW ENGINES
2020-09-01T04:42:03.797960Z	1056244 Query	SHOW VARIABLES LIKE 'innodb_version'
2020-09-01T04:42:03.798963Z	1056244 Query	SELECT MAX(`id`) FROM `qixin`.`table_demo`
2020-09-01T04:42:03.799375Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') ORDER BY `id` LIMIT 10
2020-09-01T04:42:03.825820Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:03.826092Z	1056245 Query	commit
2020-09-01T04:42:03.826128Z	1056244 Query	commit
2020-09-01T04:42:03.826226Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/AU0TjwWDdcpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:03.826499Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:03.826596Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '10')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:03.827133Z	1056245 Query	commit
2020-09-01T04:42:03.936840Z	1056244 Query	commit
2020-09-01T04:42:03.937094Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/ZhhrnEoaX7pt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:03.937437Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:03.937603Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '20')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:03.938282Z	1056245 Query	commit
2020-09-01T04:42:04.037250Z	1056244 Query	commit
2020-09-01T04:42:04.037427Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/fAB4l8yrJjpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:04.037703Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:04.037864Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '30')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:04.038516Z	1056245 Query	commit
2020-09-01T04:42:04.104206Z	1056244 Query	commit
2020-09-01T04:42:04.104382Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/FmX2lV3SQCpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:04.104634Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:04.104799Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '40')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:04.105450Z	1056245 Query	commit
2020-09-01T04:42:04.171160Z	1056244 Query	commit
2020-09-01T04:42:04.171345Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/YDsDpHpccApt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:04.171608Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:04.171761Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '50')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:04.172417Z	1056245 Query	commit
2020-09-01T04:42:04.237976Z	1056244 Query	commit
2020-09-01T04:42:04.238155Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/ObeEMvc2y0pt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:04.238418Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:04.238569Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '60')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:04.239225Z	1056245 Query	commit
2020-09-01T04:42:04.305109Z	1056244 Query	commit
2020-09-01T04:42:04.305287Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/3j9tv4KIZlpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:04.305551Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:04.305708Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '70')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:04.306362Z	1056245 Query	commit
2020-09-01T04:42:04.371976Z	1056244 Query	commit
2020-09-01T04:42:04.372154Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/BpHvFj6Arvpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:04.372416Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:04.372569Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '80')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:04.373221Z	1056245 Query	commit
2020-09-01T04:42:04.439022Z	1056244 Query	commit
2020-09-01T04:42:04.439206Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/G7_VjlVW5upt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:04.439469Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:04.439625Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '90')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:04.440275Z	1056245 Query	commit
2020-09-01T04:42:04.505950Z	1056244 Query	commit
2020-09-01T04:42:04.506130Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/5QgeHRtTO0pt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:04.506394Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:04.506550Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '100')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:04.507207Z	1056245 Query	commit
2020-09-01T04:42:04.582188Z	1056244 Query	commit
2020-09-01T04:42:04.582374Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/pt5T4WvMJ0pt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:04.582630Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:04.582780Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '110')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:04.583286Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:04.583500Z	1056245 Query	commit
2020-09-01T04:42:04.681590Z	1056244 Query	commit
2020-09-01T04:42:04.681768Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/1ExiMC9SnJpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:04.682055Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:04.682216Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '120')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:04.682858Z	1056245 Query	commit
2020-09-01T04:42:04.790047Z	1056244 Query	commit
2020-09-01T04:42:04.790229Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/xPv5JsoQgept-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:04.790499Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:04.790675Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '130')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:04.791335Z	1056245 Query	commit
2020-09-01T04:42:04.874013Z	1056244 Query	commit
2020-09-01T04:42:04.874197Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/kWzhsIt5bCpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:04.874468Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:04.874618Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '140')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:04.875266Z	1056245 Query	commit
2020-09-01T04:42:04.941001Z	1056244 Query	commit
2020-09-01T04:42:04.941180Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/zHNSfbUpXEpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:04.941438Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:04.941586Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '150')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:04.942237Z	1056245 Query	commit
2020-09-01T04:42:05.007953Z	1056244 Query	commit
2020-09-01T04:42:05.008131Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/emFtHNbqFUpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:05.008390Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:05.008545Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '160')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:05.009205Z	1056245 Query	commit
2020-09-01T04:42:05.074917Z	1056244 Query	commit
2020-09-01T04:42:05.075097Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/IUmdwAJs7zpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:05.075360Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:05.075518Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '170')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:05.076169Z	1056245 Query	commit
2020-09-01T04:42:05.141777Z	1056244 Query	commit
2020-09-01T04:42:05.141960Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/wm8WMjgCckpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:05.142217Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:05.142373Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '180')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:05.143029Z	1056245 Query	commit
2020-09-01T04:42:05.217089Z	1056244 Query	commit
2020-09-01T04:42:05.217268Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/BJQW1ZsVZPpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:05.217528Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:05.217681Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '190')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:05.218335Z	1056245 Query	commit
2020-09-01T04:42:05.293005Z	1056244 Query	commit
2020-09-01T04:42:05.293183Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/Ir6XzHYjKspt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:05.293451Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:05.293603Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '200')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:05.294249Z	1056245 Query	commit
2020-09-01T04:42:05.367842Z	1056244 Query	commit
2020-09-01T04:42:05.368030Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/GE8wk9Qp4Rpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:05.368303Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:05.368458Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '210')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:05.369105Z	1056245 Query	commit
2020-09-01T04:42:05.459813Z	1056244 Query	commit
2020-09-01T04:42:05.459999Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/7ojkfVuqdApt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:05.460273Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:05.460427Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '220')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:05.460943Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:05.461139Z	1056245 Query	commit
2020-09-01T04:42:05.560014Z	1056244 Query	commit
2020-09-01T04:42:05.560199Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/PY2MkLXbPtpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:05.560461Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:05.560613Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '230')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:05.561259Z	1056245 Query	commit
2020-09-01T04:42:05.635426Z	1056244 Query	commit
2020-09-01T04:42:05.635604Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/I2x_apU4X7pt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:05.635871Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:05.636036Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '240')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:05.636685Z	1056245 Query	commit
2020-09-01T04:42:05.711280Z	1056244 Query	commit
2020-09-01T04:42:05.711462Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/k0vRdVxwrWpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:05.711724Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:05.711872Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '250')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:05.712527Z	1056245 Query	commit
2020-09-01T04:42:05.786594Z	1056244 Query	commit
2020-09-01T04:42:05.786776Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/LbcnBI4mpMpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:05.787049Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:05.787212Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '260')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:05.787862Z	1056245 Query	commit
2020-09-01T04:42:05.861873Z	1056244 Query	commit
2020-09-01T04:42:05.862060Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/dysyx3467Cpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:05.862334Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:05.862486Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '270')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:05.863135Z	1056245 Query	commit
2020-09-01T04:42:05.928890Z	1056244 Query	commit
2020-09-01T04:42:05.929078Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/qJWjuN5nU_pt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:05.929349Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:05.929501Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '280')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:05.930151Z	1056245 Query	commit
2020-09-01T04:42:05.995860Z	1056244 Query	commit
2020-09-01T04:42:05.996047Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/R9lGJ3sRvdpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:05.996322Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:05.996473Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '290')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:05.997126Z	1056245 Query	commit
2020-09-01T04:42:06.079362Z	1056244 Query	commit
2020-09-01T04:42:06.079541Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/C5L52MNPY7pt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:06.079811Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:06.079973Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '300')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:06.080625Z	1056245 Query	commit
2020-09-01T04:42:06.146471Z	1056244 Query	commit
2020-09-01T04:42:06.146650Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/yi4ujjePb0pt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:06.146913Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:06.147068Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '310')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:06.147725Z	1056245 Query	commit
2020-09-01T04:42:06.238421Z	1056244 Query	commit
2020-09-01T04:42:06.238598Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/_9MB7glHHmpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:06.238879Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:06.239044Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '320')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:06.239688Z	1056245 Query	commit
2020-09-01T04:42:06.305315Z	1056244 Query	commit
2020-09-01T04:42:06.305493Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/8EcSFIew_Dpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:06.305754Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:06.305914Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '330')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:06.306467Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:06.306643Z	1056245 Query	commit
2020-09-01T04:42:06.372350Z	1056244 Query	commit
2020-09-01T04:42:06.372529Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/oUP7_OmRTbpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:06.372783Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:06.372939Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '340')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:06.373593Z	1056245 Query	commit
2020-09-01T04:42:06.464323Z	1056244 Query	commit
2020-09-01T04:42:06.464502Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/40FBUR4r1Mpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:06.464756Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:06.464911Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '350')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:06.465561Z	1056245 Query	commit
2020-09-01T04:42:06.532161Z	1056244 Query	commit
2020-09-01T04:42:06.532347Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/yVOJPed0Hppt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:06.532604Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:06.532758Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '360')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:06.533413Z	1056245 Query	commit
2020-09-01T04:42:06.640026Z	1056244 Query	commit
2020-09-01T04:42:06.640211Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/OZdhG9j7GYpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:06.640466Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:06.640621Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '370')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:06.641276Z	1056245 Query	commit
2020-09-01T04:42:06.756956Z	1056244 Query	commit
2020-09-01T04:42:06.757133Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/ogUto5EA60pt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:06.757520Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:06.757681Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '380')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:06.758313Z	1056245 Query	commit
2020-09-01T04:42:06.874000Z	1056244 Query	commit
2020-09-01T04:42:06.874181Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/0wwk_r6lcNpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:06.874440Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:06.874591Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '390')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:06.875247Z	1056245 Query	commit
2020-09-01T04:42:06.949301Z	1056244 Query	commit
2020-09-01T04:42:06.949484Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/Oo9Qhs3cbtpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:06.949743Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:06.949902Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '400')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:06.951145Z	1056245 Query	commit
2020-09-01T04:42:07.075070Z	1056244 Query	commit
2020-09-01T04:42:07.075251Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/BnooIBcTOYpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:07.075508Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:07.075660Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '410')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:07.076319Z	1056245 Query	commit
2020-09-01T04:42:07.142030Z	1056244 Query	commit
2020-09-01T04:42:07.142215Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/JmSMOQkEH7pt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:07.142475Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:07.142631Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '420')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:07.143284Z	1056245 Query	commit
2020-09-01T04:42:07.209002Z	1056244 Query	commit
2020-09-01T04:42:07.209181Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/sDwp6tBePopt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:07.209441Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:07.209590Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '430')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:07.210242Z	1056245 Query	commit
2020-09-01T04:42:07.300959Z	1056244 Query	commit
2020-09-01T04:42:07.301139Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/EQWdJUDUhjpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:07.301397Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:07.301552Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '440')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:07.302124Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:07.302278Z	1056245 Query	commit
2020-09-01T04:42:07.401308Z	1056244 Query	commit
2020-09-01T04:42:07.401485Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/CpblUjRy8Jpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:07.401742Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:07.401897Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '450')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:07.402550Z	1056245 Query	commit
2020-09-01T04:42:07.476646Z	1056244 Query	commit
2020-09-01T04:42:07.476824Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/TsXnGSfKxUpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:07.477091Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:07.477248Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '460')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:07.477893Z	1056245 Query	commit
2020-09-01T04:42:07.551923Z	1056244 Query	commit
2020-09-01T04:42:07.552105Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/DwMKwp60eWpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:07.552367Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:07.552520Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '470')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:07.553177Z	1056245 Query	commit
2020-09-01T04:42:07.627223Z	1056244 Query	commit
2020-09-01T04:42:07.627404Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/Zz7ePetzB6pt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:07.627660Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:07.627817Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '480')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:07.628466Z	1056245 Query	commit
2020-09-01T04:42:07.702528Z	1056244 Query	commit
2020-09-01T04:42:07.702705Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/7o_ObySYsFpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:07.702961Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:07.703116Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '490')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:07.703771Z	1056245 Query	commit
2020-09-01T04:42:07.777837Z	1056244 Query	commit
2020-09-01T04:42:07.778019Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/aj0EDs0sRnpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:07.778279Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:07.778436Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '500')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:07.779085Z	1056245 Query	commit
2020-09-01T04:42:07.911330Z	1056244 Query	commit
2020-09-01T04:42:07.911514Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/mkylWPgQ90pt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:07.911768Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:07.911924Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '510')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:07.912577Z	1056245 Query	commit
2020-09-01T04:42:07.986798Z	1056244 Query	commit
2020-09-01T04:42:07.986978Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/tXFiirNV1ypt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:07.987237Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:07.987394Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '520')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:07.988051Z	1056245 Query	commit
2020-09-01T04:42:08.062722Z	1056244 Query	commit
2020-09-01T04:42:08.062900Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/mvt_t5DbPJpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:08.063160Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:08.063318Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '530')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:08.063960Z	1056245 Query	commit
2020-09-01T04:42:08.138006Z	1056244 Query	commit
2020-09-01T04:42:08.138185Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/wX99KfKYRPpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:08.138445Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:08.138594Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '540')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:08.139245Z	1056245 Query	commit
2020-09-01T04:42:08.238157Z	1056244 Query	commit
2020-09-01T04:42:08.238338Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/9reiwtZkidpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:08.238591Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:08.238748Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '550')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:08.239340Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:08.239475Z	1056245 Query	commit
2020-09-01T04:42:08.313622Z	1056244 Query	commit
2020-09-01T04:42:08.313800Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/kNigCSweHSpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:08.314060Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:08.314221Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '560')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:08.314872Z	1056245 Query	commit
2020-09-01T04:42:08.380528Z	1056244 Query	commit
2020-09-01T04:42:08.380698Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/gwf4vqkjbnpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:08.430849Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:08.431035Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '570')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:08.431751Z	1056245 Query	commit
2020-09-01T04:42:08.497706Z	1056244 Query	commit
2020-09-01T04:42:08.497885Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/kHb62JQi_Ypt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:08.498144Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:08.498295Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '580')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:08.498936Z	1056245 Query	commit
2020-09-01T04:42:08.597961Z	1056244 Query	commit
2020-09-01T04:42:08.598139Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/2bMCu9O93ipt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:08.598399Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:08.598563Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '590')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:08.599216Z	1056245 Query	commit
2020-09-01T04:42:08.664987Z	1056244 Query	commit
2020-09-01T04:42:08.665167Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/Jsysc1Yocipt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:08.665422Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:08.665573Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '600')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:08.681574Z	1056245 Query	commit
2020-09-01T04:42:08.790277Z	1056244 Query	commit
2020-09-01T04:42:08.790460Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/lXxwC378x0pt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:08.790716Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:08.790871Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '610')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:08.791515Z	1056245 Query	commit
2020-09-01T04:42:08.865401Z	1056244 Query	commit
2020-09-01T04:42:08.865582Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/KHCg2d10RIpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:08.865837Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:08.865991Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '620')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:08.866627Z	1056245 Query	commit
2020-09-01T04:42:08.940956Z	1056244 Query	commit
2020-09-01T04:42:08.941135Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/_sZHGTYKGApt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:08.941394Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:08.941548Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '630')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:08.942206Z	1056245 Query	commit
2020-09-01T04:42:09.016166Z	1056244 Query	commit
2020-09-01T04:42:09.016353Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/vF45ZHOHcOpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:09.016612Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:09.016764Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '640')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:09.017413Z	1056245 Query	commit
2020-09-01T04:42:09.091544Z	1056244 Query	commit
2020-09-01T04:42:09.091726Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/noIFI_fi4ipt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:09.091988Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:09.092142Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '650')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:09.092790Z	1056245 Query	commit
2020-09-01T04:42:09.166857Z	1056244 Query	commit
2020-09-01T04:42:09.167044Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/il929ioHsRpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:09.167306Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:09.167459Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '660')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:09.168055Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:09.168175Z	1056245 Query	commit
2020-09-01T04:42:09.250405Z	1056244 Query	commit
2020-09-01T04:42:09.250584Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/tz3sh5Szz6pt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:09.250843Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:09.251025Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '670')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:09.251703Z	1056245 Query	commit
2020-09-01T04:42:09.317393Z	1056244 Query	commit
2020-09-01T04:42:09.317571Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/bdQj8e4eEcpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:09.317825Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:09.317981Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '680')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:09.318630Z	1056245 Query	commit
2020-09-01T04:42:09.426044Z	1056244 Query	commit
2020-09-01T04:42:09.426232Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/Jp4ErJJjlcpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:09.426489Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:09.426645Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '690')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:09.427300Z	1056245 Query	commit
2020-09-01T04:42:09.543139Z	1056244 Query	commit
2020-09-01T04:42:09.543318Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/9QUMFfvhO5pt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:09.543575Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:09.543729Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '700')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:09.544387Z	1056245 Query	commit
2020-09-01T04:42:09.610051Z	1056244 Query	commit
2020-09-01T04:42:09.610230Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/KyrzcurRQTpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:09.610487Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:09.610643Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '710')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:09.611298Z	1056245 Query	commit
2020-09-01T04:42:09.710427Z	1056244 Query	commit
2020-09-01T04:42:09.710605Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/opPBKqu5yRpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:09.710862Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:09.711036Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '720')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:09.711687Z	1056245 Query	commit
2020-09-01T04:42:09.794033Z	1056244 Query	commit
2020-09-01T04:42:09.794216Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/_EceM5rAy6pt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:09.794477Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:09.794631Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '730')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:09.795286Z	1056245 Query	commit
2020-09-01T04:42:09.869459Z	1056244 Query	commit
2020-09-01T04:42:09.869638Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/GQMI9teU30pt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:09.869897Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:09.870059Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '740')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:09.870712Z	1056245 Query	commit
2020-09-01T04:42:09.945263Z	1056244 Query	commit
2020-09-01T04:42:09.945446Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/YTVkZlpSjSpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:09.945702Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:09.945856Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '750')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:09.946508Z	1056245 Query	commit
2020-09-01T04:42:10.020048Z	1056244 Query	commit
2020-09-01T04:42:10.020231Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/t0EjLYsgkipt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:10.020488Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:10.020642Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '760')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:10.021297Z	1056245 Query	commit
2020-09-01T04:42:10.112023Z	1056244 Query	commit
2020-09-01T04:42:10.112210Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/2dNKB08SQ4pt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:10.112471Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:10.112625Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '770')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:10.113247Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:10.113347Z	1056245 Query	commit
2020-09-01T04:42:10.187396Z	1056244 Query	commit
2020-09-01T04:42:10.187576Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/501ZKz1LDgpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:10.187830Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:10.187985Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '780')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:10.188645Z	1056245 Query	commit
2020-09-01T04:42:10.263469Z	1056244 Query	commit
2020-09-01T04:42:10.263652Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/D7Gsl1DuXwpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:10.263907Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:10.264069Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '790')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:10.264719Z	1056245 Query	commit
2020-09-01T04:42:10.337982Z	1056244 Query	commit
2020-09-01T04:42:10.338162Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/tsvJXeWH06pt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:10.338420Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:10.338571Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '800')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:10.339222Z	1056245 Query	commit
2020-09-01T04:42:10.455498Z	1056244 Query	commit
2020-09-01T04:42:10.455678Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/jJgEZ7t9Uspt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:10.455932Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:10.456092Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '810')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:10.456745Z	1056245 Query	commit
2020-09-01T04:42:10.522481Z	1056244 Query	commit
2020-09-01T04:42:10.522659Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/KPgLsuzNl4pt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:10.522916Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:10.523074Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '820')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:10.523726Z	1056245 Query	commit
2020-09-01T04:42:10.614443Z	1056244 Query	commit
2020-09-01T04:42:10.614626Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/4l8eq2DHfRpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:10.614879Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:10.615042Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '830')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:10.615691Z	1056245 Query	commit
2020-09-01T04:42:10.723110Z	1056244 Query	commit
2020-09-01T04:42:10.723299Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/zlNeNUynrNpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:10.723551Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:10.723701Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '840')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:10.724353Z	1056245 Query	commit
2020-09-01T04:42:10.806952Z	1056244 Query	commit
2020-09-01T04:42:10.807199Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/wiNAQSa6Ulpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:10.807456Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:10.807602Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '850')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:10.808242Z	1056245 Query	commit
2020-09-01T04:42:10.907267Z	1056244 Query	commit
2020-09-01T04:42:10.907447Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/msdcnapyUCpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:10.907705Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:10.907858Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '860')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:10.908503Z	1056245 Query	commit
2020-09-01T04:42:10.990993Z	1056244 Query	commit
2020-09-01T04:42:10.991171Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/QmLXXAAROJpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:10.991428Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:10.991576Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '870')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:10.992225Z	1056245 Query	commit
2020-09-01T04:42:11.066298Z	1056244 Query	commit
2020-09-01T04:42:11.066479Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/SKEO81cWXupt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:11.066732Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:11.066886Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '880')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:11.067527Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:11.067607Z	1056245 Query	commit
2020-09-01T04:42:11.191397Z	1056244 Query	commit
2020-09-01T04:42:11.191576Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/jbfU5MmZJtpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:11.191830Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:11.191988Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '890')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:11.192635Z	1056245 Query	commit
2020-09-01T04:42:11.266913Z	1056244 Query	commit
2020-09-01T04:42:11.267089Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/4cSSQtrZ3ipt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:11.267351Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:11.267501Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '900')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:11.268149Z	1056245 Query	commit
2020-09-01T04:42:11.375568Z	1056244 Query	commit
2020-09-01T04:42:11.375747Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/wohqNRU8nNpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:11.376007Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:11.376162Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '910')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:11.377030Z	1056245 Query	commit
2020-09-01T04:42:11.484278Z	1056244 Query	commit
2020-09-01T04:42:11.484460Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/bfLollGPQjpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:11.484719Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:11.484873Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '920')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:11.485526Z	1056245 Query	commit
2020-09-01T04:42:11.551214Z	1056244 Query	commit
2020-09-01T04:42:11.551393Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/Pb1red8_xKpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:11.551651Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:11.551805Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '930')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:11.552457Z	1056245 Query	commit
2020-09-01T04:42:11.618267Z	1056244 Query	commit
2020-09-01T04:42:11.618445Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/lfekW_Jg1_pt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:11.618699Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:11.618853Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '940')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:11.619507Z	1056245 Query	commit
2020-09-01T04:42:11.685238Z	1056244 Query	commit
2020-09-01T04:42:11.685415Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/a4XHFOEmYZpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:11.727222Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:11.727397Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '950')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:11.728108Z	1056245 Query	commit
2020-09-01T04:42:11.794098Z	1056244 Query	commit
2020-09-01T04:42:11.794276Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/muB8hQnQLlpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:11.794532Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:11.794687Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '960')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:11.795340Z	1056245 Query	commit
2020-09-01T04:42:11.860870Z	1056244 Query	commit
2020-09-01T04:42:11.861051Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/PrRdopmrOQpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:11.861315Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:11.861467Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '970')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:11.862110Z	1056245 Query	commit
2020-09-01T04:42:11.961280Z	1056244 Query	commit
2020-09-01T04:42:11.961457Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/tzCNO3aFyipt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:11.961715Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:11.961866Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '980')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:11.962516Z	1056245 Query	commit
2020-09-01T04:42:12.061641Z	1056244 Query	commit
2020-09-01T04:42:12.061819Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/G3eClHNfQ_pt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:12.062084Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:12.062240Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '990')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:12.062914Z	1056245 Query	LOAD DATA LOCAL INFILE '/tmp/6cO0RoUE_9pt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:42:12.063086Z	1056244 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:42:12.063178Z	1056244 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '999')) ORDER BY `id` LIMIT 10
2020-09-01T04:42:12.063444Z	1056245 Query	commit
2020-09-01T04:42:12.136943Z	1056244 Query	commit
```

## 无主键

### limit 10
```bash 
2020-09-01T04:45:54.612856Z	1056246 Connect	root@localhost on qixin using Socket
2020-09-01T04:45:54.623925Z	1056246 Query	set autocommit=0
2020-09-01T04:45:54.624208Z	1056246 Query	/*!40101 SET NAMES "utf8"*/
2020-09-01T04:45:54.624500Z	1056246 Query	SHOW VARIABLES LIKE 'wait\_timeout'
2020-09-01T04:45:54.656371Z	1056246 Query	SET SESSION wait_timeout=10000
2020-09-01T04:45:54.661161Z	1056246 Query	SELECT @@SQL_MODE
2020-09-01T04:45:54.668229Z	1056246 Query	SET @@SQL_QUOTE_SHOW_CREATE = 1/*!40101, @@SQL_MODE='NO_AUTO_VALUE_ON_ZERO,ONLY_FULL_GROUP_BY'*/
2020-09-01T04:45:54.668560Z	1056246 Query	SHOW VARIABLES LIKE 'version%'
2020-09-01T04:45:54.670183Z	1056246 Query	SHOW ENGINES
2020-09-01T04:45:54.670572Z	1056246 Query	SHOW VARIABLES LIKE 'innodb_version'
2020-09-01T04:45:54.672227Z	1056246 Query	show variables like 'innodb_rollback_on_timeout'
2020-09-01T04:45:54.673738Z	1056246 Query	/*!40101 SET @OLD_SQL_MODE := @@SQL_MODE, @@SQL_MODE := '', @OLD_QUOTE := @@SQL_QUOTE_SHOW_CREATE, @@SQL_QUOTE_SHOW_CREATE := 1 */
2020-09-01T04:45:54.673934Z	1056246 Query	USE `qixin`
2020-09-01T04:45:54.674112Z	1056246 Query	SHOW CREATE TABLE `qixin`.`table_demo1`
2020-09-01T04:45:54.678915Z	1056246 Query	/*!40101 SET @@SQL_MODE := @OLD_SQL_MODE, @@SQL_QUOTE_SHOW_CREATE := @OLD_QUOTE */
2020-09-01T04:45:54.679534Z	1056247 Connect	root@localhost on qixin using Socket
2020-09-01T04:45:54.679664Z	1056247 Query	set autocommit=0
2020-09-01T04:45:54.679753Z	1056247 Query	/*!40101 SET NAMES "utf8"*/
2020-09-01T04:45:54.679839Z	1056247 Query	SHOW VARIABLES LIKE 'wait\_timeout'
2020-09-01T04:45:55.218189Z	1056247 Query	SET SESSION wait_timeout=10000
2020-09-01T04:45:55.234263Z	1056247 Query	SELECT @@SQL_MODE
2020-09-01T04:45:55.234411Z	1056247 Query	SET @@SQL_QUOTE_SHOW_CREATE = 1/*!40101, @@SQL_MODE='NO_AUTO_VALUE_ON_ZERO,ONLY_FULL_GROUP_BY'*/
2020-09-01T04:45:55.234560Z	1056247 Query	SHOW VARIABLES LIKE 'version%'
2020-09-01T04:45:55.235579Z	1056247 Query	SHOW ENGINES
2020-09-01T04:45:55.235830Z	1056247 Query	SHOW VARIABLES LIKE 'innodb_version'
2020-09-01T04:45:55.236820Z	1056247 Query	show variables like 'innodb_rollback_on_timeout'
2020-09-01T04:45:55.237728Z	1056247 Query	/*!40101 SET @OLD_SQL_MODE := @@SQL_MODE, @@SQL_MODE := '', @OLD_QUOTE := @@SQL_QUOTE_SHOW_CREATE, @@SQL_QUOTE_SHOW_CREATE := 1 */
2020-09-01T04:45:55.237792Z	1056247 Query	USE `qixin`
2020-09-01T04:45:55.237863Z	1056247 Query	SHOW CREATE TABLE `qixin`.`table_demo1_history`
2020-09-01T04:45:55.241535Z	1056247 Query	/*!40101 SET @@SQL_MODE := @OLD_SQL_MODE, @@SQL_QUOTE_SHOW_CREATE := @OLD_QUOTE */
2020-09-01T04:45:55.241737Z	1056246 Query	SHOW VARIABLES LIKE 'wsrep_on'
2020-09-01T04:45:55.243097Z	1056246 Query	SHOW VARIABLES LIKE 'wsrep_on'
2020-09-01T04:45:55.244589Z	1056246 Query	SHOW VARIABLES LIKE 'version%'
2020-09-01T04:45:55.245981Z	1056246 Query	SHOW ENGINES
2020-09-01T04:45:55.246232Z	1056246 Query	SHOW VARIABLES LIKE 'innodb_version'
2020-09-01T04:45:55.247947Z	1056246 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo1` FORCE INDEX(`idx_ucreatetime`) WHERE (ucreatetime >= '2020-09-01 11:51:26') ORDER BY `ucreatetime` LIMIT 10
2020-09-01T04:45:55.268841Z	1056246 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:45:55.269115Z	1056247 Query	commit
2020-09-01T04:45:55.269152Z	1056246 Query	commit
2020-09-01T04:45:55.276755Z	1056247 Query	LOAD DATA LOCAL INFILE '/tmp/0mSKYe9H_1pt-archiver' INTO TABLE `qixin`.`table_demo1_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:45:55.391896Z	1056246 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:45:55.392042Z	1056246 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo1` FORCE INDEX(`idx_ucreatetime`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (((('2020-09-01 11:51:26' IS NULL AND `ucreatetime` IS NOT NULL) OR (`ucreatetime` > '2020-09-01 11:51:26')))) ORDER BY `ucreatetime` LIMIT 10
2020-09-01T04:45:55.392634Z	1056247 Query	commit
2020-09-01T04:45:55.467403Z	1056246 Query	commit
2020-09-01T04:45:55.467582Z	1056247 Query	LOAD DATA LOCAL INFILE '/tmp/3d4ZQAAhXmpt-archiver' INTO TABLE `qixin`.`table_demo1_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:45:55.467841Z	1056246 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:45:55.468008Z	1056246 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo1` FORCE INDEX(`idx_ucreatetime`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (((('2020-09-02 11:51:27' IS NULL AND `ucreatetime` IS NOT NULL) OR (`ucreatetime` > '2020-09-02 11:51:27')))) ORDER BY `ucreatetime` LIMIT 10
2020-09-01T04:45:55.468659Z	1056247 Query	commit
2020-09-01T04:45:55.534448Z	1056246 Query	commit
2020-09-01T04:45:55.534626Z	1056247 Query	LOAD DATA LOCAL INFILE '/tmp/OM6TBYirhppt-archiver' INTO TABLE `qixin`.`table_demo1_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:45:55.534880Z	1056246 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:45:55.535045Z	1056246 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo1` FORCE INDEX(`idx_ucreatetime`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (((('2020-09-03 11:51:27' IS NULL AND `ucreatetime` IS NOT NULL) OR (`ucreatetime` > '2020-09-03 11:51:27')))) ORDER BY `ucreatetime` LIMIT 10
2020-09-01T04:45:55.535692Z	1056247 Query	commit
2020-09-01T04:45:55.601410Z	1056246 Query	commit
2020-09-01T04:45:55.601589Z	1056247 Query	LOAD DATA LOCAL INFILE '/tmp/ugFgLdzN5Vpt-archiver' INTO TABLE `qixin`.`table_demo1_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:45:55.601842Z	1056246 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:45:55.602005Z	1056246 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo1` FORCE INDEX(`idx_ucreatetime`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (((('2020-09-04 11:51:27' IS NULL AND `ucreatetime` IS NOT NULL) OR (`ucreatetime` > '2020-09-04 11:51:27')))) ORDER BY `ucreatetime` LIMIT 10
2020-09-01T04:45:55.602658Z	1056247 Query	commit
2020-09-01T04:45:55.668336Z	1056246 Query	commit
2020-09-01T04:45:55.668513Z	1056247 Query	LOAD DATA LOCAL INFILE '/tmp/eS64zSsg81pt-archiver' INTO TABLE `qixin`.`table_demo1_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:45:55.668784Z	1056246 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:45:55.668945Z	1056246 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo1` FORCE INDEX(`idx_ucreatetime`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (((('2020-09-05 11:51:27' IS NULL AND `ucreatetime` IS NOT NULL) OR (`ucreatetime` > '2020-09-05 11:51:27')))) ORDER BY `ucreatetime` LIMIT 10
2020-09-01T04:45:55.669588Z	1056247 Query	commit
2020-09-01T04:45:55.735344Z	1056246 Query	commit
2020-09-01T04:45:55.735525Z	1056247 Query	LOAD DATA LOCAL INFILE '/tmp/pm9PxNBm_Mpt-archiver' INTO TABLE `qixin`.`table_demo1_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:45:55.735781Z	1056246 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:45:55.735946Z	1056246 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo1` FORCE INDEX(`idx_ucreatetime`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (((('2020-09-06 11:51:27' IS NULL AND `ucreatetime` IS NOT NULL) OR (`ucreatetime` > '2020-09-06 11:51:27')))) ORDER BY `ucreatetime` LIMIT 10
2020-09-01T04:45:55.736597Z	1056247 Query	commit
2020-09-01T04:45:55.802210Z	1056246 Query	commit
2020-09-01T04:45:55.802392Z	1056247 Query	LOAD DATA LOCAL INFILE '/tmp/ZgF1CTnJbEpt-archiver' INTO TABLE `qixin`.`table_demo1_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:45:55.802647Z	1056246 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:45:55.802805Z	1056246 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo1` FORCE INDEX(`idx_ucreatetime`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (((('2020-09-07 11:51:27' IS NULL AND `ucreatetime` IS NOT NULL) OR (`ucreatetime` > '2020-09-07 11:51:27')))) ORDER BY `ucreatetime` LIMIT 10
2020-09-01T04:45:55.803459Z	1056247 Query	commit
2020-09-01T04:45:55.869209Z	1056246 Query	commit
2020-09-01T04:45:55.869388Z	1056247 Query	LOAD DATA LOCAL INFILE '/tmp/KdLG2B7LqRpt-archiver' INTO TABLE `qixin`.`table_demo1_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:45:55.869636Z	1056246 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:45:55.869790Z	1056246 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo1` FORCE INDEX(`idx_ucreatetime`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (((('2020-09-08 11:51:27' IS NULL AND `ucreatetime` IS NOT NULL) OR (`ucreatetime` > '2020-09-08 11:51:27')))) ORDER BY `ucreatetime` LIMIT 10
2020-09-01T04:45:55.870438Z	1056247 Query	commit
2020-09-01T04:45:55.936160Z	1056246 Query	commit
2020-09-01T04:45:55.936345Z	1056247 Query	LOAD DATA LOCAL INFILE '/tmp/DIf1nNHCG2pt-archiver' INTO TABLE `qixin`.`table_demo1_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T04:45:55.936601Z	1056246 Query	SELECT 'pt-archiver keepalive'
2020-09-01T04:45:55.936763Z	1056246 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo1` FORCE INDEX(`idx_ucreatetime`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (((('2020-09-09 11:51:27' IS NULL AND `ucreatetime` IS NOT NULL) OR (`ucreatetime` > '2020-09-09 11:51:27')))) ORDER BY `ucreatetime` LIMIT 10
2020-09-01T04:45:55.937182Z	1056247 Query	commit
2020-09-01T04:45:56.011530Z	1056246 Query	commit	
```

### limit 500

```bash 
2020-09-01T07:24:52.407145Z	1056259 Query	USE `qixin`
2020-09-01T07:24:52.407216Z	1056259 Query	SHOW CREATE TABLE `qixin`.`table_demo1`
2020-09-01T07:24:52.407338Z	1056259 Query	/*!40101 SET @@SQL_MODE := @OLD_SQL_MODE, @@SQL_QUOTE_SHOW_CREATE := @OLD_QUOTE */
2020-09-01T07:24:52.407779Z	1056260 Connect	root@localhost on qixin using Socket
2020-09-01T07:24:52.407847Z	1056260 Query	set autocommit=0
2020-09-01T07:24:52.407912Z	1056260 Query	/*!40101 SET NAMES "utf8"*/
2020-09-01T07:24:52.407989Z	1056260 Query	SHOW VARIABLES LIKE 'wait\_timeout'
2020-09-01T07:24:52.408816Z	1056260 Query	SET SESSION wait_timeout=10000
2020-09-01T07:24:52.417696Z	1056260 Query	SELECT @@SQL_MODE
2020-09-01T07:24:52.424302Z	1056260 Query	SET @@SQL_QUOTE_SHOW_CREATE = 1/*!40101, @@SQL_MODE='NO_AUTO_VALUE_ON_ZERO,ONLY_FULL_GROUP_BY'*/
2020-09-01T07:24:52.424482Z	1056260 Query	SHOW VARIABLES LIKE 'version%'
2020-09-01T07:24:52.425539Z	1056260 Query	SHOW ENGINES
2020-09-01T07:24:52.425778Z	1056260 Query	SHOW VARIABLES LIKE 'innodb_version'
2020-09-01T07:24:52.426745Z	1056260 Query	show variables like 'innodb_rollback_on_timeout'
2020-09-01T07:24:52.427632Z	1056260 Query	/*!40101 SET @OLD_SQL_MODE := @@SQL_MODE, @@SQL_MODE := '', @OLD_QUOTE := @@SQL_QUOTE_SHOW_CREATE, @@SQL_QUOTE_SHOW_CREATE := 1 */
2020-09-01T07:24:52.427694Z	1056260 Query	USE `qixin`
2020-09-01T07:24:52.427761Z	1056260 Query	SHOW CREATE TABLE `qixin`.`table_demo1_history1`
2020-09-01T07:24:52.427882Z	1056260 Query	/*!40101 SET @@SQL_MODE := @OLD_SQL_MODE, @@SQL_QUOTE_SHOW_CREATE := @OLD_QUOTE */
2020-09-01T07:24:52.428089Z	1056259 Query	SHOW VARIABLES LIKE 'wsrep_on'
2020-09-01T07:24:52.429419Z	1056259 Query	SHOW VARIABLES LIKE 'wsrep_on'
2020-09-01T07:24:52.430896Z	1056259 Query	SHOW VARIABLES LIKE 'version%'
2020-09-01T07:24:52.432260Z	1056259 Query	SHOW ENGINES
2020-09-01T07:24:52.432508Z	1056259 Query	SHOW VARIABLES LIKE 'innodb_version'
2020-09-01T07:24:52.434235Z	1056259 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo1` FORCE INDEX(`idx_ucreatetime`) WHERE (ucreatetime >= '2020-09-01 11:51:26') ORDER BY `ucreatetime` LIMIT 500
2020-09-01T07:24:52.457603Z	1056259 Query	SELECT 'pt-archiver keepalive'
2020-09-01T07:24:52.457874Z	1056260 Query	commit
2020-09-01T07:24:52.457909Z	1056259 Query	commit
2020-09-01T07:24:52.458142Z	1056260 Query	commit
2020-09-01T07:24:52.458169Z	1056259 Query	commit
2020-09-01T07:24:52.458394Z	1056260 Query	commit
2020-09-01T07:24:52.458420Z	1056259 Query	commit
2020-09-01T07:24:52.458640Z	1056260 Query	commit
2020-09-01T07:24:52.458665Z	1056259 Query	commit
2020-09-01T07:24:52.458881Z	1056260 Query	commit
2020-09-01T07:24:52.458906Z	1056259 Query	commit
2020-09-01T07:24:52.459122Z	1056260 Query	commit
2020-09-01T07:24:52.459148Z	1056259 Query	commit
2020-09-01T07:24:52.459372Z	1056260 Query	commit
2020-09-01T07:24:52.459400Z	1056259 Query	commit
2020-09-01T07:24:52.459617Z	1056260 Query	commit
2020-09-01T07:24:52.459642Z	1056259 Query	commit
2020-09-01T07:24:52.459856Z	1056260 Query	commit
2020-09-01T07:24:52.459881Z	1056259 Query	commit
2020-09-01T07:24:52.460096Z	1056260 Query	commit
2020-09-01T07:24:52.460122Z	1056259 Query	commit
2020-09-01T07:24:52.460182Z	1056259 Query	SELECT 'pt-archiver keepalive'
2020-09-01T07:24:52.460392Z	1056260 Query	commit
2020-09-01T07:24:52.460420Z	1056259 Query	commit
2020-09-01T07:24:52.460637Z	1056260 Query	commit
2020-09-01T07:24:52.460664Z	1056259 Query	commit
2020-09-01T07:24:52.460890Z	1056260 Query	commit
2020-09-01T07:24:52.460917Z	1056259 Query	commit
2020-09-01T07:24:52.461134Z	1056260 Query	commit
2020-09-01T07:24:52.461160Z	1056259 Query	commit
2020-09-01T07:24:52.461379Z	1056260 Query	commit
2020-09-01T07:24:52.461404Z	1056259 Query	commit
2020-09-01T07:24:52.461617Z	1056260 Query	commit
2020-09-01T07:24:52.461642Z	1056259 Query	commit
2020-09-01T07:24:52.461856Z	1056260 Query	commit
2020-09-01T07:24:52.461881Z	1056259 Query	commit
2020-09-01T07:24:52.462094Z	1056260 Query	commit
2020-09-01T07:24:52.462120Z	1056259 Query	commit
2020-09-01T07:24:52.462339Z	1056260 Query	commit
2020-09-01T07:24:52.462365Z	1056259 Query	commit
2020-09-01T07:24:52.462580Z	1056260 Query	commit
2020-09-01T07:24:52.462606Z	1056259 Query	commit
2020-09-01T07:24:52.462664Z	1056259 Query	SELECT 'pt-archiver keepalive'
2020-09-01T07:24:52.462866Z	1056260 Query	commit
2020-09-01T07:24:52.462893Z	1056259 Query	commit
2020-09-01T07:24:52.463108Z	1056260 Query	commit
2020-09-01T07:24:52.463134Z	1056259 Query	commit
2020-09-01T07:24:52.463351Z	1056260 Query	commit
2020-09-01T07:24:52.463377Z	1056259 Query	commit
2020-09-01T07:24:52.463600Z	1056260 Query	commit
2020-09-01T07:24:52.463624Z	1056259 Query	commit
2020-09-01T07:24:52.463834Z	1056260 Query	commit
2020-09-01T07:24:52.463858Z	1056259 Query	commit
2020-09-01T07:24:52.464068Z	1056260 Query	commit
2020-09-01T07:24:52.464091Z	1056259 Query	commit
2020-09-01T07:24:52.464309Z	1056260 Query	commit
2020-09-01T07:24:52.464336Z	1056259 Query	commit
2020-09-01T07:24:52.464552Z	1056260 Query	commit
2020-09-01T07:24:52.464577Z	1056259 Query	commit
2020-09-01T07:24:52.464791Z	1056260 Query	commit
2020-09-01T07:24:52.464816Z	1056259 Query	commit
2020-09-01T07:24:52.465032Z	1056260 Query	commit
2020-09-01T07:24:52.465058Z	1056259 Query	commit
2020-09-01T07:24:52.465116Z	1056259 Query	SELECT 'pt-archiver keepalive'
2020-09-01T07:24:52.465324Z	1056260 Query	commit
2020-09-01T07:24:52.465352Z	1056259 Query	commit
2020-09-01T07:24:52.465569Z	1056260 Query	commit
2020-09-01T07:24:52.465596Z	1056259 Query	commit
2020-09-01T07:24:52.465814Z	1056260 Query	commit
2020-09-01T07:24:52.465841Z	1056259 Query	commit
2020-09-01T07:24:52.466059Z	1056260 Query	commit
2020-09-01T07:24:52.466086Z	1056259 Query	commit
2020-09-01T07:24:52.466308Z	1056260 Query	commit
2020-09-01T07:24:52.466333Z	1056259 Query	commit
2020-09-01T07:24:52.466559Z	1056260 Query	commit
2020-09-01T07:24:52.466585Z	1056259 Query	commit
2020-09-01T07:24:52.466800Z	1056260 Query	commit
2020-09-01T07:24:52.466825Z	1056259 Query	commit
2020-09-01T07:24:52.467048Z	1056260 Query	commit
2020-09-01T07:24:52.467076Z	1056259 Query	commit
2020-09-01T07:24:52.467296Z	1056260 Query	commit
2020-09-01T07:24:52.467323Z	1056259 Query	commit
2020-09-01T07:24:52.467540Z	1056260 Query	commit
2020-09-01T07:24:52.467567Z	1056259 Query	commit
2020-09-01T07:24:52.467628Z	1056259 Query	SELECT 'pt-archiver keepalive'
2020-09-01T07:24:52.467833Z	1056260 Query	commit
2020-09-01T07:24:52.467861Z	1056259 Query	commit
2020-09-01T07:24:52.468079Z	1056260 Query	commit
2020-09-01T07:24:52.468107Z	1056259 Query	commit
2020-09-01T07:24:52.468327Z	1056260 Query	commit
2020-09-01T07:24:52.468354Z	1056259 Query	commit
2020-09-01T07:24:52.468571Z	1056260 Query	commit
2020-09-01T07:24:52.468598Z	1056259 Query	commit
2020-09-01T07:24:52.468815Z	1056260 Query	commit
2020-09-01T07:24:52.468842Z	1056259 Query	commit
2020-09-01T07:24:52.469061Z	1056260 Query	commit
2020-09-01T07:24:52.469089Z	1056259 Query	commit
2020-09-01T07:24:52.469311Z	1056260 Query	commit
2020-09-01T07:24:52.469338Z	1056259 Query	commit
2020-09-01T07:24:52.469563Z	1056260 Query	commit
2020-09-01T07:24:52.469590Z	1056259 Query	commit
2020-09-01T07:24:52.469808Z	1056260 Query	commit
2020-09-01T07:24:52.469835Z	1056259 Query	commit
2020-09-01T07:24:52.470053Z	1056260 Query	commit
2020-09-01T07:24:52.470081Z	1056259 Query	commit
2020-09-01T07:24:52.470169Z	1056260 Query	LOAD DATA LOCAL INFILE '/tmp/ilApsDuNwxpt-archiver' INTO TABLE `qixin`.`table_demo1_history1`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T07:24:52.637221Z	1056259 Query	SELECT 'pt-archiver keepalive'
2020-09-01T07:24:52.637405Z	1056259 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo1` FORCE INDEX(`idx_ucreatetime`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (((('2020-09-05 11:51:27' IS NULL AND `ucreatetime` IS NOT NULL) OR (`ucreatetime` > '2020-09-05 11:51:27')))) ORDER BY `ucreatetime` LIMIT 500
2020-09-01T07:24:52.638412Z	1056259 Query	SELECT 'pt-archiver keepalive'
2020-09-01T07:24:52.638626Z	1056260 Query	commit
2020-09-01T07:24:52.712539Z	1056259 Query	commit
2020-09-01T07:24:52.712892Z	1056260 Query	commit
2020-09-01T07:24:52.712952Z	1056259 Query	commit
2020-09-01T07:24:52.713237Z	1056260 Query	commit
2020-09-01T07:24:52.713286Z	1056259 Query	commit
2020-09-01T07:24:52.713543Z	1056260 Query	commit
2020-09-01T07:24:52.713577Z	1056259 Query	commit
2020-09-01T07:24:52.713795Z	1056260 Query	commit
2020-09-01T07:24:52.713823Z	1056259 Query	commit
2020-09-01T07:24:52.714046Z	1056260 Query	commit
2020-09-01T07:24:52.714074Z	1056259 Query	commit
2020-09-01T07:24:52.714292Z	1056260 Query	commit
2020-09-01T07:24:52.714319Z	1056259 Query	commit
2020-09-01T07:24:52.714536Z	1056260 Query	commit
2020-09-01T07:24:52.714563Z	1056259 Query	commit
2020-09-01T07:24:52.714780Z	1056260 Query	commit
2020-09-01T07:24:52.714807Z	1056259 Query	commit
2020-09-01T07:24:52.715027Z	1056260 Query	commit
2020-09-01T07:24:52.715055Z	1056259 Query	commit
2020-09-01T07:24:52.715142Z	1056259 Query	SELECT 'pt-archiver keepalive'
2020-09-01T07:24:52.715343Z	1056260 Query	commit
2020-09-01T07:24:52.715371Z	1056259 Query	commit
2020-09-01T07:24:52.715602Z	1056260 Query	commit
2020-09-01T07:24:52.715628Z	1056259 Query	commit
2020-09-01T07:24:52.715842Z	1056260 Query	commit
2020-09-01T07:24:52.715867Z	1056259 Query	commit
2020-09-01T07:24:52.716085Z	1056260 Query	commit
2020-09-01T07:24:52.716111Z	1056259 Query	commit
2020-09-01T07:24:52.716360Z	1056260 Query	commit
2020-09-01T07:24:52.716386Z	1056259 Query	commit
2020-09-01T07:24:52.716608Z	1056260 Query	commit
2020-09-01T07:24:52.716634Z	1056259 Query	commit
2020-09-01T07:24:52.716849Z	1056260 Query	commit
2020-09-01T07:24:52.716875Z	1056259 Query	commit
2020-09-01T07:24:52.717092Z	1056260 Query	commit
2020-09-01T07:24:52.717118Z	1056259 Query	commit
2020-09-01T07:24:52.717345Z	1056260 Query	commit
2020-09-01T07:24:52.717373Z	1056259 Query	commit
2020-09-01T07:24:52.717591Z	1056260 Query	commit
2020-09-01T07:24:52.717618Z	1056259 Query	commit
2020-09-01T07:24:52.717699Z	1056259 Query	SELECT 'pt-archiver keepalive'
2020-09-01T07:24:52.717887Z	1056260 Query	commit
2020-09-01T07:24:52.717915Z	1056259 Query	commit
2020-09-01T07:24:52.718137Z	1056260 Query	commit
2020-09-01T07:24:52.718165Z	1056259 Query	commit
2020-09-01T07:24:52.718382Z	1056260 Query	commit
2020-09-01T07:24:52.718409Z	1056259 Query	commit
2020-09-01T07:24:52.718634Z	1056260 Query	commit
2020-09-01T07:24:52.718662Z	1056259 Query	commit
2020-09-01T07:24:52.718878Z	1056260 Query	commit
2020-09-01T07:24:52.718905Z	1056259 Query	commit
2020-09-01T07:24:52.719127Z	1056260 Query	commit
2020-09-01T07:24:52.719155Z	1056259 Query	commit
2020-09-01T07:24:52.719372Z	1056260 Query	commit
2020-09-01T07:24:52.719399Z	1056259 Query	commit
2020-09-01T07:24:52.719614Z	1056260 Query	commit
2020-09-01T07:24:52.719641Z	1056259 Query	commit
2020-09-01T07:24:52.719862Z	1056260 Query	commit
2020-09-01T07:24:52.719890Z	1056259 Query	commit
2020-09-01T07:24:52.720111Z	1056260 Query	commit
2020-09-01T07:24:52.720139Z	1056259 Query	commit
2020-09-01T07:24:52.720219Z	1056259 Query	SELECT 'pt-archiver keepalive'
2020-09-01T07:24:52.720406Z	1056260 Query	commit
2020-09-01T07:24:52.720433Z	1056259 Query	commit
2020-09-01T07:24:52.720652Z	1056260 Query	commit
2020-09-01T07:24:52.720679Z	1056259 Query	commit
2020-09-01T07:24:52.720896Z	1056260 Query	commit
2020-09-01T07:24:52.720923Z	1056259 Query	commit
2020-09-01T07:24:52.721144Z	1056260 Query	commit
2020-09-01T07:24:52.721172Z	1056259 Query	commit
2020-09-01T07:24:52.721391Z	1056260 Query	commit
2020-09-01T07:24:52.721418Z	1056259 Query	commit
2020-09-01T07:24:52.721643Z	1056260 Query	commit
2020-09-01T07:24:52.721671Z	1056259 Query	commit
2020-09-01T07:24:52.721888Z	1056260 Query	commit
2020-09-01T07:24:52.721915Z	1056259 Query	commit
2020-09-01T07:24:52.722135Z	1056260 Query	commit
2020-09-01T07:24:52.722163Z	1056259 Query	commit
2020-09-01T07:24:52.722382Z	1056260 Query	commit
2020-09-01T07:24:52.722409Z	1056259 Query	commit
2020-09-01T07:24:52.722627Z	1056260 Query	commit
2020-09-01T07:24:52.722654Z	1056259 Query	commit
2020-09-01T07:24:52.722758Z	1056260 Query	LOAD DATA LOCAL INFILE '/tmp/pEtJXKqDTxpt-archiver' INTO TABLE `qixin`.`table_demo1_history1`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
2020-09-01T07:24:52.763110Z	1056259 Query	SELECT 'pt-archiver keepalive'
2020-09-01T07:24:52.763289Z	1056259 Query	SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` FROM `qixin`.`table_demo1` FORCE INDEX(`idx_ucreatetime`) WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (((('2020-09-09 11:51:27' IS NULL AND `ucreatetime` IS NOT NULL) OR (`ucreatetime` > '2020-09-09 11:51:27')))) ORDER BY `ucreatetime` LIMIT 500
2020-09-01T07:24:52.763708Z	1056260 Query	commit
2020-09-01T07:24:52.838064Z	1056259 Query	commit
2020-09-01T07:24:52.838341Z	1056259 Quit
2020-09-01T07:24:52.838978Z	1056260 Quit
2020-09-01T07:24:57.837254Z	1056256 Query	SET GLOBAL general_log = 'OFF'
```

# 总结

{% note info %}
为什么表无主键时获取不到正确的行数呢？
{% endnote %}

`--limit=10` 代表一次取出10行数据：

## 有主键的表

第一步：PT工具第一步获取 source 表的主键id的最大值，命令如下：

```sql 
SELECT MAX(`id`) FROM `qixin`.`table_demo`
``` 

第二步：按照PT命令中指定的`where过滤条件` AND `对主键ID的过滤`，按照ID排序，取前10行,保存到临时文件中，命令如下：

```sql 
SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` 
FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) 
WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') 
ORDER BY `id` LIMIT 10
```

第三步：通过LOAD DATA LOCAL INFILE 快速加载文件到归档表table_demo_history。

```SQL 
LOAD DATA LOCAL INFILE '/tmp/AU0TjwWDdcpt-archiver' INTO TABLE `qixin`.`table_demo_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
```

接下来就是循环第二步和第三步，直到取完所有的数据, 并加载到归档表table_demo_history。

```sql 
SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` 
FROM `qixin`.`table_demo` FORCE INDEX(`PRIMARY`) 
WHERE (ucreatetime >= '2020-09-01 11:51:26') AND (`id` < '1000') AND ((`id` > '10')) 
ORDER BY `id` LIMIT 10
```

## 无主键的表

第一步：由于没有主键，因此没有办法获取主键ID的最大值，跳过该步骤。

第二步：按照PT命令中指定的`where过滤条件` AND `对主键ID的过滤`，按照ID排序，取前10行,保存到临时文件中，命令如下：

```sql
SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` 
FROM `qixin`.`table_demo1` FORCE INDEX(`idx_ucreatetime`) 
WHERE (ucreatetime >= '2020-09-01 11:51:26') 
ORDER BY `ucreatetime` LIMIT 10
```

第三步：通过LOAD DATA LOCAL INFILE 快速加载文件到归档表table_demo_history。

```SQL 
LOAD DATA LOCAL INFILE '/tmp/0mSKYe9H_1pt-archiver' INTO TABLE `qixin`.`table_demo1_history`CHARACTER SET utf8(`id`,`uname`,`ucreatetime`,`age`)
```

接下来就是循环第二步和第三步，直到取完所有的数据, 并加载到归档表table_demo_history。

```sql
SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` 
FROM `qixin`.`table_demo1` FORCE INDEX(`idx_ucreatetime`) 
WHERE (ucreatetime >= '2020-09-01 11:51:26') AND
 (
((
('2020-09-01 11:51:26' IS NULL AND `ucreatetime` IS NOT NULL) OR 
(`ucreatetime` > '2020-09-01 11:51:26')
))
) 
ORDER BY `ucreatetime` LIMIT 10
```

## 无主键时的过滤

第 1 次查询条件取了 `ucreatetime >= '2020-09-01 11:51:26'` 的 10行数据，且排序规则使用的是`ucreatetime`
第 2 次查询条件取了 `ucreatetime >  '2020-09-01 11:51:26'` 的 10行数据，且排序规则使用的是`ucreatetime`
第 3 次查询条件取了 `ucreatetime >  '2020-09-02 11:51:26'` 的 10行数据，且排序规则使用的是`ucreatetime`
第 4 次查询条件取了 `ucreatetime >  '2020-09-04 11:51:26'` 的 10行数据，且排序规则使用的是`ucreatetime`
第 n 次查询条件取了 `ucreatetime >  '2020-09-xx 11:51:26'` 的 10行数据，且排序规则使用的是`ucreatetime`

个人理解：索引`idx_ucreatetime` 为辅助索引，内部为B+树，叶子节点存放着`ucreatetime`的所有数据

无主键的过滤条件中PT会使用叶子节点的值进行过滤(具体值为上一次查询的最后一个值)，并取limit 10；

从而导致了 `--limit 10` 只能归档90条数据。 

```sql 
root@MySQL-01 14:59:  [qixin]> select distinct ucreatetime from table_demo1;
+---------------------+
| ucreatetime         |
+---------------------+
| 2020-09-01 11:51:26 |
| 2020-09-02 11:51:27 |
| 2020-09-03 11:51:27 |
| 2020-09-04 11:51:27 |
| 2020-09-05 11:51:27 |
| 2020-09-06 11:51:27 |
| 2020-09-07 11:51:27 |
| 2020-09-08 11:51:27 |
| 2020-09-09 11:51:27 |
+---------------------+
9 rows in set (0.02 sec)
```

当调整`limit`参数为 500时，PT的查询如下：

```sql 
SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` 
FROM `qixin`.`table_demo1` FORCE INDEX(`idx_ucreatetime`) 
WHERE (ucreatetime >= '2020-09-01 11:51:26') ORDER BY `ucreatetime` LIMIT 500;

# 执行结果
|  497 | 用户7   | 2020-09-04 11:51:27 |   20 |
|  498 | 用户8   | 2020-09-04 11:51:27 |   20 |
|  499 | 用户9   | 2020-09-04 11:51:27 |   20 |
|  500 | 用户0   | 2020-09-05 11:51:27 |   20 |
+------+---------+---------------------+------+
500 rows in set (0.03 sec)


SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` 
FROM `qixin`.`table_demo1` FORCE INDEX(`idx_ucreatetime`) 
WHERE (ucreatetime >= '2020-09-01 11:51:26') AND
 (
 ((
        ('2020-09-05 11:51:27' IS NULL AND `ucreatetime` IS NOT NULL) OR 
        (`ucreatetime` > '2020-09-05 11:51:27')
 ))
 ) 
 ORDER BY `ucreatetime` LIMIT 500;

# 执行结果
|  998 | 用户8   | 2020-09-09 11:51:27 |   20 |
|  999 | 用户9   | 2020-09-09 11:51:27 |   20 |
| 1000 | 用户0   | 2020-09-09 11:51:27 |   20 |
+------+---------+---------------------+------+
401 rows in set (0.00 sec)

 
SELECT /*!40001 SQL_NO_CACHE */ `id`,`uname`,`ucreatetime`,`age` 
FROM `qixin`.`table_demo1` FORCE INDEX(`idx_ucreatetime`) 
WHERE (ucreatetime >= '2020-09-01 11:51:26') AND 
(
((
    ('2020-09-09 11:51:27' IS NULL AND `ucreatetime` IS NOT NULL) OR 
    (`ucreatetime` > '2020-09-09 11:51:27')
 ))
 ) 
ORDER BY `ucreatetime` LIMIT 500
```

由此可以得出，PT在无主键的归档时，是无法保证数据正确的。建议生产中的表应规范主键。


# 思考

是否可以调整PT源码，使其支持无主键的表呢？

没有必要，让工具去支持不规范的操作是没有意义的。

{% note warn pt-archiver操作的表必须有主键%}
{% endnote %}
