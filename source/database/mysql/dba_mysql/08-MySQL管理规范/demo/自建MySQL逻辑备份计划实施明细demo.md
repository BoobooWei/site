---
title: 2018.10.8.0.xxx.自建MySQL逻辑备份计划实施明细
---

## 备份策略设置

### 数据库添加备份专用授权

```
mysql> grant select,reload,show databases,super,lock tables,replication client,show view,event,file on *.* to backup@'localhost' identified by 'abc123';
Query OK, 0 rows affected (0.00 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)
```

### 备份脚本

> 系统库也一起备份

```
[root@zeji-git-server mysql]# vim backup.sh
[root@zeji-git-server mysql]# cd
[root@zeji-git-server ~]# ll /usr/local/mysql/backup.sh
-rw-r--r-- 1 root root 685 Jan  2 13:48 /usr/local/mysql/backup.sh
[root@zeji-git-server ~]# cat /usr/local/mysql/backup.sh
#!/bin/bash
DBUser=backup
DBPwd=abc123
DBName=mysql
BackupPath="/alidata/backup"
BackupFile="$DBName-"$(date +%y%m%d_%H)".sql"
# Backup Dest directory, change this if you have someother location
if !(test -d $BackupPath)
then
mkdir $BackupPath -p
fi
cd $BackupPath
mysqldump -u$DBUser -p$DBPwd -A --opt --default-character-set=utf8  --single-transaction --hex-blob --skip-triggers --max_allowed_packet=824288000 > "$BackupPath"/"$BackupFile"
#Delete sql type file
find "$BackupPath" -name "$DBname*[log,sql]" -type f -mtime +14 -exec rm -rf {} \;
```

### crontab配置

```
[root@zeji-git-server ~]# crontab -e
crontab: installing new crontab

#crontab 每日24点备份数据库
1 0 * * * /bin/bash /usr/local/mysql/backup.sh
```

------

## 配置文件修改

### 备份原有配置文件

```
[zyadmin@zeji-git-server ~]$ sudo -i
[root@zeji-git-server ~]# cd /etc/
[root@zeji-git-server etc]# cp my.cnf my.cnf.20180102
[root@zeji-git-server etc]# ls my.cnf*
my.cnf  my.cnf.20180102
[root@zeji-git-server etc]# ls my.cnf* -l
-rw-r--r-- 1 root root 2031 Jan  2 10:55 my.cnf
-rw-r--r-- 1 root root 2031 Jan  2 13:35 my.cnf.20180102
```

### 新建新的配置文件

```
[root@zeji-git-server etc]# vim /etc/my.cnf.new
[root@zeji-git-server etc]# ll my.cnf*
-rw-r--r-- 1 root root 2031 Jan  2 10:55 my.cnf
-rw-r--r-- 1 root root 2031 Jan  2 13:35 my.cnf.20180102
-rw-r--r-- 1 root root 1698 Jan  2 13:38 my.cnf.new
```

### 确定2点钟重启

```
[root@zeji-git-server ~]# cp /etc/my.cnf.new /etc/my.cnf
cp: overwrite `/etc/my.cnf'? yes
[root@zeji-git-server ~]# /etc/init.d/mysql restart
Shutting down MySQL.... SUCCESS!
Starting MySQL. SUCCESS!
```

### 抽查配置参数

```
mysql> show variables like 'innodb_bu%';
+-------------------------------------+----------------+
| Variable_name                       | Value          |
+-------------------------------------+----------------+
| innodb_buffer_pool_dump_at_shutdown | OFF            |
| innodb_buffer_pool_dump_now         | OFF            |
| innodb_buffer_pool_filename         | ib_buffer_pool |
| innodb_buffer_pool_instances        | 8              |
| innodb_buffer_pool_load_abort       | OFF            |
| innodb_buffer_pool_load_at_startup  | OFF            |
| innodb_buffer_pool_load_now         | OFF            |
| innodb_buffer_pool_size             | 4208984064     |
+-------------------------------------+----------------+
```

参数已生效

### 后续事宜

待今日凌晨备份成功后，做备份数据有效性检查后再确定备份计划是否生效。