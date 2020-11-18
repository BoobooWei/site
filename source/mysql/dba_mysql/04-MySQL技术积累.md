---
title: MySQL 技术积累
---

* [mysql_5.5_SQL](/mysql/dba_mysql/tec/mysql_5.5_SQL.html)
* [mysql_5.6_consul_ha](/mysql/dba_mysql/tec/mysql_5.6_consul_ha.html)
* [mysql_5.6_datetime](/mysql/dba_mysql/tec/mysql_5.6_datetime.html)
* [mysql_5.6_gtid_temporary_table](/mysql/dba_mysql/tec/mysql_5.6_gtid_temporary_table.html)
* [mysql_5.6_install](/mysql/dba_mysql/tec/mysql_5.6_install.html)
* [mysql_5.6_onlineddl](/mysql/dba_mysql/tec/mysql_5.6_onlineddl.html)
* [mysql_5.6_replication_从机turncate导致主从出错](/mysql/dba_mysql/tec/mysql_5.6_replication_从机turncate导致主从出错.html)
* [mysql_5.6_replication_基于gtid的perconaxtrabackup搭建步骤和排错](/mysql/dba_mysql/tec/mysql_5.6_replication_基于gtid的perconaxtrabackup搭建步骤和排错.html)
* [mysql_5.6_replication_常见故障记录](/mysql/dba_mysql/tec/mysql_5.6_replication_常见故障记录.html)
* [mysql_5.6_timestamp](/mysql/dba_mysql/tec/mysql_5.6_timestamp.html)
* [mysql_5.6_waiting](/mysql/dba_mysql/tec/mysql_5.6_waiting.html)
* [mysql_5.6_修改MySQL](/mysql/dba_mysql/tec/mysql_5.6_修改MySQL.html)
* [mysql_5.6_分区表数据紧急救援](/mysql/dba_mysql/tec/mysql_5.6_分区表数据紧急救援.html)
* [mysql_5.6_分区表测试](/mysql/dba_mysql/tec/mysql_5.6_分区表测试.html)
* [mysql_5.6_数据库归档测试](/mysql/dba_mysql/tec/mysql_5.6_数据库归档测试.html)
* [mysql_5.6_错误日志note级别信息](/mysql/dba_mysql/tec/mysql_5.6_错误日志note级别信息.html)
* [mysql_5.7_LWT](/mysql/dba_mysql/tec/mysql_5.7_LWT.html)
* [mysql_5.7_SQL](/mysql/dba_mysql/tec/mysql_5.7_SQL.html)
* [mysql_5.7_add_date](/mysql/dba_mysql/tec/mysql_5.7_add_date.html)
* [mysql_5.7_cluster_parameter](/mysql/dba_mysql/tec/mysql_5.7_cluster_parameter.html)
* [mysql_5.7_confluence修复字符集](/mysql/dba_mysql/tec/mysql_5.7_confluence修复字符集.html)
* [mysql_5.7_count](/mysql/dba_mysql/tec/mysql_5.7_count.html)
* [mysql_5.7_exsits测试](/mysql/dba_mysql/tec/mysql_5.7_exsits测试.html)
* [mysql_5.7_flush_slow_logs](/mysql/dba_mysql/tec/mysql_5.7_flush_slow_logs.html)
* [mysql_5.7_groupby报错](/mysql/dba_mysql/tec/mysql_5.7_groupby报错.html)
* [mysql_5.7_json](/mysql/dba_mysql/tec/mysql_5.7_json.html)
* [mysql_5.7_lower_case_table_names](/mysql/dba_mysql/tec/mysql_5.7_lower_case_table_names.html)
* [mysql_5.7_max_allowed_packet解决办法](/mysql/dba_mysql/tec/mysql_5.7_max_allowed_packet解决办法.html)
* [mysql_5.7_my.cnf](/mysql/dba_mysql/tec/mysql_5.7_my.cnf.html)
* [mysql_5.7_new](/mysql/dba_mysql/tec/mysql_5.7_new.html)
* [mysql_5.7_online_gtid_on](/mysql/dba_mysql/tec/mysql_5.7_online_gtid_on.html)
* [mysql_5.7_password函数](/mysql/dba_mysql/tec/mysql_5.7_password函数.html)
* [mysql_5.7_replication_parameter](/mysql/dba_mysql/tec/mysql_5.7_replication_parameter.html)
* [mysql_5.7_主从复制修改基于组的并发](/mysql/dba_mysql/tec/mysql_5.7_主从复制修改基于组的并发.html)
* [mysql_5.7_主库查看从库信息](/mysql/dba_mysql/tec/mysql_5.7_主库查看从库信息.html)
* [mysql_5.7_删除索引后不会立刻释放空间](/mysql/dba_mysql/tec/mysql_5.7_删除索引后不会立刻释放空间.html)
* [mysql_5.7_半同步复制](/mysql/dba_mysql/tec/mysql_5.7_半同步复制.html)
* [mysql_5.7_单机部署多实例](/mysql/dba_mysql/tec/mysql_5.7_单机部署多实例.html)
* [mysql_5.7_在线强制修改参数](/mysql/dba_mysql/tec/mysql_5.7_在线强制修改参数.html)
* [mysql_5.7_在线重置root密码方案](/mysql/dba_mysql/tec/mysql_5.7_在线重置root密码方案.html)
* [mysql_5.7_批量生成索引创建语句](/mysql/dba_mysql/tec/mysql_5.7_批量生成索引创建语句.html)
* [mysql_5.7_死锁分析](/mysql/dba_mysql/tec/mysql_5.7_死锁分析.html)
* [mysql_5.7_记一次特殊空格c2a0的数据清洗](/mysql/dba_mysql/tec/mysql_5.7_记一次特殊空格c2a0的数据清洗.html)
* [mysql_5.7_通过frm和ibd强制恢复结构和数据](/mysql/dba_mysql/tec/mysql_5.7_通过frm和ibd强制恢复结构和数据.html)
* [mysql_5.7_分区表物理删除报错解决](/mysql/dba_mysql/tec/mysql_5.7_分区表物理删除报错解决.html)

# MySQL DBA

> 2018-06-12 源自姜老师

## 入门

* 会搭建主从复制？一主多从
* 会用MHA搭建一个高可用集群
* 会搭建最新的MGR集群
* 会编写增删该查的SQL
* 知道InnoDB行锁的不同语法
* 会用mysqldump进行数据库备份

## 高阶

* 精通复制的原理
* 精通MHA的实现逻辑
* 精通MGR的机制
* 能对SQL进行调优
* 精通S、X、IS、IX锁的实现
* 精通mysqldump的实现原理

## Top 5

### 代码能力

* 尝试用Python/Go重写MHA
* 尝试用Python重写一个更快的mydumper/myloader
* 尝试自己写一个RDS
* 尝试自己开发一个内核功能

### 解决能力

* 解决MySQL OMM的方法论
* MySQL唯一键死锁案例推导
* 描述线上遇到过最困难的问题
* 业务秒杀架构设计
* 金融级数据库强一致容灾架构设计

### 总结能力

* 博客或者公众号
* GitHub
* 图书出版
* 论文
* 专利



### 技术能力

* 熟知数据库各版本特性和Bug
* 解决难题的终极能力
* 拥有总结成方法论的能力
* 预测未来五年技术趋势的能力

### 业务能力

* 熟知各个业务在数据库侧的难点和解决之道
* 懂业务，能够协调业务一起进行架构改造
* 敢担责，勇背锅的能力
