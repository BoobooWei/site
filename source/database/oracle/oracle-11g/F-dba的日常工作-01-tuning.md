---
title: DBA的日常工作
---

```bash
dba的日常工作:
oracle的产品安装:参考安装包中的安装文档（单机和RAC --> ops）

数据库的网络配置:listener.ora & tnsnames.ora

创建数据库：安全因素（控制文件和日志成员的多元化），存储的规划（合理设计表空间），数据量大（2g以上，行超过千万级别）的表设计成分区表将表中数据库分散到多个表空间，数据量小的频繁访问的表也要分散到不同的表空间，表和索引要分散到不同的表空间，需要若干的temp表空间，至少2个undo表空间

oracle跟踪文件目录下要有足够的空闲空间:
audit_file_dest=''
diagnostic_dest=''

监控数据库的空间使用情况（每周）

监控数据库中的无效对象，无效索引，无效约束，无效日志（每天）

将空数据库的统计信息

监控数据库中的等待事件：v$system_event,v$session_wait

监控数据量大的表和索引
select * from (select segment_name,blocks,segment_type from dba_segments order by 2 desc) where rownum<11;

指定备份策略

定期查看备份日志

为数据库安装补丁程序（数据库版本升级）

数据库性能调整：内存、网络、存储

通过oem查看数据库运行情况的概览
------------------------------------------------------------------------------------------
为数据库规划内存使用：
OLTP：联机事务处理系统（并发量大，操作相对简单，计算不复杂，访问数据量不大）
OLAP：联机分析系统（连接的会话很少，但计算非常复杂，通常指仓库系统决策支持系统）

OLTP：80% TOTAL MEMORY
PGA：20%
SGA：80%

OLAP：80% TOTAL MEMORY
PGA：50% 
SGA：50% 
------------------------------------------------------------------------------------------
oracle 11g 的内存分配：（sga+pga）组合在一起自动管理
memory_max_target:静态参数，oracle使用的内存上限
memory_target	 :动态参数,自动管理的内存的上向，memory_target<=memory_max_target<=/dev/shm

kernel.shmmni*kernel.shmall = total_memory*80%
/dev/shm = total_memory*80%
memory_max_target = total_memory*80% 

SQL> select * from v$sgainfo;
控制共享池的参数仍然有效，指定的值是共享池收缩的最小值
SQL> alter system set shared_pool_size=300m;
控制数据库缓冲区高速缓存的最小值db_cache_size
SQL> alter system set db_cache_size=500m;
*上面两个参数控制内存收缩的下限，避免性能下降

SQL> select * from v$pgastat;
pga_aggregate_target 控制pga大小的参数无效
------------------------------------------------------------------------------------------
oracle 10g 的内存分配：
从spfile中删除11g内存自动管理的参数
SQL> alter system reset memory_target scope=spfile sid='*';
SQL> startup force

PGA自动管理的参数
pga_aggregate_target --> 单个pga扩张的上限

查看后台进程的PGA使用情况
SQL> select p.pid,b.name,p.PGA_USED_MEM/1048576 mb from v$bgprocess b,v$process p where p.addr=b.paddr;

根据会话查找pga使用情况：
SQL> select PGA_USED_MEM/1048576 from v$process where addr in (select paddr from v$session where username='SCOTT');

pga对排序操作的影响：
通过sql跟踪查看sql语句执行成本：
SQL> set autot trace exp

SQL> select * from t01 order by 5,4,3,2,1;
Elapsed: 00:00:00.00

Execution Plan
----------------------------------------------------------
Plan hash value: 2509066158

-----------------------------------------------------------------------------------
| Id  | Operation	   | Name | Rows  | Bytes |TempSpc| Cost (%CPU)| Time	  |
-----------------------------------------------------------------------------------
|   0 | SELECT STATEMENT   |	  |   200K|  6445K|	  |  5246   (1)| 00:01:03 |
|   1 |  SORT ORDER BY	   |	  |   200K|  6445K|    10M|  5246   (1)| 00:01:03 |
|   2 |   TABLE ACCESS FULL| T01  |   200K|  6445K|	  |   343   (1)| 00:00:05 |
-----------------------------------------------------------------------------------

查看pga的调整建议：
SQL> select PGA_TARGET_FOR_ESTIMATE/1048576,ESTD_PGA_CACHE_HIT_PERCENTAGE from v$pga_target_advice;

修改cursor info中的游标数量：
show parameter session_cached_cursors = 50
查看cursor info中的游标：快速软解析
select SID,USER_NAME,sql_text from v$open_cursor where sql_text like 'select * from scott%';
------------------------------------------------------------------------------------------
```
