---
title: 自建MySQL物理备份计划实施明细demo
---

# 备份策略

- 备份工具：innobackupex
- 备份分类：物理备份、在线热备、全备+增备
- 备份策略：每天1:59开始执行备份脚本；周一全备份，周二到周日增量；每周自动删除上一周过期备份数据
- 备份文件：/alidata/backup
- 其他信息：备份索引/alidata/xtrabackup_cron/var/mysql_increment_hot_backup.index

# 备份脚本

innobakcupex自动备份脚本以及说明见附件。

# 实施明细

> 192.168.20.1 192.168.20.2 都需要配置

## Step01.安装percona-xtrabackup

```bash
yum install -y libev
yum install -y https://repo.percona.com/yum/percona-release-latest.noarch.rpm 
yum install -y percona-xtrabackup-24
```

安装成功如下：

```bash
Installed:
 percona-xtrabackup-24.x86_64 0:2.4.9-1.el7                                               
 
Dependency Installed:
 perl-DBD-MySQL.x86_64 0:4.023-5.el7           perl-Digest.noarch 0:1.17-245.el7         
 perl-Digest-MD5.x86_64 0:2.52-3.el7         
 
Complete!
 
[root@data-20-1 install]# which innobackupex
/usr/bin/innobackupex
```

## Step02.数据库添加备份用户权限

根据最小权限原则，添加ro_user用户，具体权限如下：

```sql
grant lock tables,reload,process,replication client,super on *.* to ro_user@'localhost' identified by '9n1vLvnGia1unZPBoamG';
 
flush privileges;
```

## Step03.部署备份脚本

```bash
[root@data-20-1 install]# ll /alidata/xtrabackup_cron/
total 24
drwxr-xr-x. 2 root root 4096 Mar  7 09:44 bin
drwxr-xr-x. 2 root root 4096 Mar  7 09:44 conf
drwxr-xr-x. 2 root root 4096 Mar  7 09:44 log
-rw-r--r--. 1 root root 6849 Mar  7 12:30 readme.md
drwxr-xr-x. 2 root root 4096 Mar  7 09:44 var
```

| 目录 | 说明                         |
| :--- | :--------------------------- |
| bin  | 备份脚本存放路径             |
| conf | 备份脚本会读取的配置文件     |
| log  | 备份日志                     |
| var  | 备份文件索引信息以及报错信息 |

## Step04.修改备份配置

新建目录/alidata/backup用于存放备份文件。

```bash
[root@data-20-1 xtrabackup_cron]# vim conf/mysql_increment_hot_backup.conf
 
# mysql 用户名
user=ro_user
 
# mysql 密码
password=9n1vLvnGia1unZPBoamG
 
# 备份路劲
backup_dir=/alidata/backup
 
# percona-xtrabackup 备份软件路径
xtrabackup_dir=/usr
 
# 全备是在一周的第几天
full_backup_week_day=1
 
# 全量备信息名称 前缀
full_backup_prefix=full
 
# 增量备信息名称 前缀
increment_prefix=incr
 
# mysql配置文件
mysql_conf_file=/etc/my.cnf
 
# 错误日志文件(更具此文件知道备份是否成功)
# format:
# {week_day:1,dir:full/incr_2015-12-29_00-00-00_7,type:full/incr,date:2015-12-30}
error_log=../var/mysql_increment_hot_backup.err
 
# 索引文件
# format:
# {week_day:1,dir:full/incr_2015-12-29_00-00-00_7,type:full/incr}
index_file=../var/mysql_increment_hot_backup.index
```

## Step05.测试运行

手动执行脚本，查看备份文件，备份日志，备份索引情况

```bash
cd /alidata/xtrabackup_cron/bin
[root@data-20-1 xtrabackup_cron]# /bin/bash /alidata/xtrabackup_cron/bin/mysql_increment_hot_backup.sh
[root@data-20-1 xtrabackup_cron]# ll /alidata/backup/
total 4
drwxr-xr-x. 22 root root 4096 Mar  7 22:04 full_2018-03-07_22-04-06_3
[root@data-20-1 xtrabackup_cron]# ll log
total 160
-rw-r--r--. 1 root root 163247 Mar  7 22:04 full_2018-03-07_22-04-06_3.log
[root@data-20-1 xtrabackup_cron]# tail log/full_2018-03-07_22-04-06_3.log
180307 22:04:08 [00]       ...done
180307 22:04:08 Backup created in directory '/alidata/backup/full_2018-03-07_22-04-06_3/'
MySQL binlog position: filename 'mysql-bin.000011', position '10484186', GTID of the last change '24097bb4-c631-11e7-8a3e-a0369ff4c124:1-9626,
d9741e92-c62e-11e7-845c-a0369ff4c11c:1-22087'
180307 22:04:08 [00] Writing /alidata/backup/full_2018-03-07_22-04-06_3/backup-my.cnf
180307 22:04:08 [00]       ...done
180307 22:04:08 [00] Writing /alidata/backup/full_2018-03-07_22-04-06_3/xtrabackup_info
180307 22:04:08 [00]       ...done
xtrabackup: Transaction log of lsn (22782816) to (22782825) was copied.
180307 22:04:08 completed OK!
[root@data-20-1 xtrabackup_cron]# ll var/
total 4
-rw-r--r--. 1 root root   0 Dec 30  2015 mysql_increment_hot_backup.err
-rw-r--r--. 1 root root 100 Mar  7 22:04 mysql_increment_hot_backup.index
-rw-r--r--. 1 root root   0 Mar  7 22:04 mysql_increment_hot_backup.index_2018-03-06
[root@data-20-1 xtrabackup_cron]# cat var/mysql_increment_hot_backup.index
{week_day:3,         dir:full_2018-03-07_22-04-06_3,         type:full,         date:2018-03-07}
 
==============================================================================================
[root@data-20-2 xtrabackup_cron]# ll /alidata/backup/
total 4
drwxr-xr-x. 22 root root 4096 Mar  7 22:16 full_2018-03-07_22-16-22_3
[root@data-20-2 xtrabackup_cron]# tail log/full_2018-03-07_22-16-22_3.log
180307 22:16:23 [00]       ...done
180307 22:16:23 Backup created in directory '/alidata/backup/full_2018-03-07_22-16-22_3/'
MySQL binlog position: filename 'mysql-bin.000009', position '3513619', GTID of the last change '24097bb4-c631-11e7-8a3e-a0369ff4c124:1-9628,
d9741e92-c62e-11e7-845c-a0369ff4c11c:1-22087'
180307 22:16:23 [00] Writing /alidata/backup/full_2018-03-07_22-16-22_3/backup-my.cnf
180307 22:16:23 [00]       ...done
180307 22:16:23 [00] Writing /alidata/backup/full_2018-03-07_22-16-22_3/xtrabackup_info
180307 22:16:23 [00]       ...done
xtrabackup: Transaction log of lsn (23083357) to (23083366) was copied.
180307 22:16:24 completed OK!
[root@data-20-2 xtrabackup_cron]# ll var/
total 4
-rw-r--r--. 1 root root   0 Dec 30  2015 mysql_increment_hot_backup.err
-rw-r--r--. 1 root root 100 Mar  7 22:16 mysql_increment_hot_backup.index
-rw-r--r--. 1 root root   0 Mar  7 22:16 mysql_increment_hot_backup.index_2018-03-06
[root@data-20-2 xtrabackup_cron]# cat var/mysql_increment_hot_backup.index
{week_day:3,         dir:full_2018-03-07_22-16-22_3,         type:full,         date:2018-03-07}
```

手动备份成功

## Step06.设置定时备份任务

```bash
crontab -e
59 01 * * * /bin/bash /alidata/xtrabackup_cron/bin/mysql_increment_hot_backup.sh
设置每日01:59分开始执行该备份脚本，两台服务器分别查看配置是否成功：
[root@data-20-1 xtrabackup_cron]# crontab -l -u root
59 01 * * * /bin/bash /alidata/xtrabackup_cron/bin/mysql_increment_hot_backup.sh
[root@data-20-2 xtrabackup_cron]# crontab -u root -l
59 01 * * * /bin/bash /alidata/xtrabackup_cron/bin/mysql_increment_hot_backup.sh
```

# 总结

两台服务器`192.168.20.1`和`192.168.20.2`均已完成物理备份自动备份部署。

- 备份工具：`innobackupex`
- 备份分类：物理备份、在线热备、全备+增备
- 备份策略：每天1:59开始执行备份脚本；周一全备份，周二到周日增量；每周自动删除上一周过期备份数据
- 备份文件：`/alidata/backup`
- 其他信息：备份索引`/alidata/xtrabackup_cron/var/mysql_increment_hot_backup.index`

备份恢复方法：

通过innobackupex手动恢复即可。
