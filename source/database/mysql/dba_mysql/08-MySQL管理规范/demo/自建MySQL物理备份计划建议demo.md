---
title: 自建MySQL物理备份计划建议demo
---

## 需求

请备份mysql数据, 每天一次, 数据保持两周14天

## 操作系统概况

> OS 2核/8G 目前系统资源充足

此处可以补充系统资源名明细

## MySQL备份建议

### 备份工具选择

鉴于当前数据库中的数据量超过50G，建议使用物理备份工具innobackupex。

### 备份策略

1. 备份工具：innobackupex
2. 备份分类：物理备份、在线热备、全备+增备
3. 备份策略：每天1:59开始执行备份脚本；周一全备份，周二到周日增量；每周自动删除上一周过期备份数据
4. 备份文件：/alidata/backup
5. 其他信息：备份索引/alidata/xtrabackup_cron/var/mysql_increment_hot_backup.index

### 备份脚本

[xtrabackup_cron备份程序](https://github.com/BoobooWei/DBA_Mysql/tree/master/scripts/backup/mysql_xtrabackup)

### 定时任务

```bash
crontab -e
59 01 * * * /bin/bash /alidata/xtrabackup_cron/bin/mysql_increment_hot_backup.sh
```

**请确定定时任务的执行时间，建议选择业务低峰期。**

## 后续事宜

请您根据建议进行审核，待您确认后，我方再开始操作。
