---
title: 2018.10.8.0.xxx.自建MySQL逻辑备份计划建议
---

## 需求

Git服务器上我们已经部署了crm, 发现系统有点卡 我看了下mysql的配置文件, innodb buffer size才 256M

1. 请梳理调整配置参数, 以提高性能
2. 请备份mysql数据, 每天一次, 数据保持两周14天

## 操作系统概况

> OS 2核/8G 目前系统资源充足

```
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                2
On-line CPU(s) list:   0,1
Thread(s) per core:    2
Core(s) per socket:    1
Socket(s):             1
NUMA node(s):          1
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 79
Model name:            Intel(R) Xeon(R) CPU E5-2682 v4 @ 2.50GHz
Stepping:              1
CPU MHz:               2499.994
BogoMIPS:              4999.98
Hypervisor vendor:     KVM
Virtualization type:   full
L1d cache:             32K
L1i cache:             32K
L2 cache:              256K
L3 cache:              40960K
NUMA node0 CPU(s):     0,1
             total       used       free     shared    buffers     cached
Mem:       8061376    7919544     141832     106804     133572    3715704
-/+ buffers/cache:    4070268    3991108
Swap:            0          0          0
Filesystem      Size  Used Avail Use% Mounted on
/dev/vda1        40G   24G   14G  63% /
tmpfs           3.9G     0  3.9G   0% /dev/shm
/dev/vdb         50G   52M   47G   1% /alidata
procs -----------memory---------- ---swap-- -----io---- --system-- -----cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0      0 141832 133580 3715696    0    0     4   137   13    9  2  0 98  0  0
 0  0      0 141848 133584 3715824    0    0     0    60  422  650  1  0 98  0  0
 2  0      0 141476 133588 3715824    0    0    36   328  791  877 12  1 87  1  0
 0  0      0 141476 133588 3715916    0    0     0  2568  819  839  8  1 89  2  0
 1  0      0 141616 133592 3715868    0    0    12   120  831  881 10  1 89  1  0
Linux 2.6.32-642.6.2.el6.x86_64 (zeji-git-server)       01/02/2018      _x86_64_        (2 CPU)

Device:         rrqm/s   wrqm/s     r/s     w/s    rkB/s    wkB/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util
vda               0.00    57.70    0.20   13.90     7.45   273.10    39.81     0.16   11.13    2.72   11.25   0.59   0.83
vdb               0.00     0.14    0.00    0.00     0.00     0.58   618.47     0.00 1227.92    4.45 1392.64   5.51   0.00

Device:         rrqm/s   wrqm/s     r/s     w/s    rkB/s    wkB/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util
vda               0.00    28.00    1.00   10.00     8.00   148.00    28.36     0.01    0.82    0.00    0.90   0.55   0.60
vdb               0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00    0.00    0.00   0.00   0.00

Device:         rrqm/s   wrqm/s     r/s     w/s    rkB/s    wkB/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util
vda               0.00   264.00    0.00   65.00     0.00  1300.00    40.00     0.22    3.38    0.00    3.38   0.28   1.80
vdb               0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00    0.00    0.00   0.00   0.00

Device:         rrqm/s   wrqm/s     r/s     w/s    rkB/s    wkB/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util
vda               0.00    33.00    0.00    8.00     0.00   160.00    40.00     0.01    1.12    0.00    1.12   0.88   0.70
vdb               0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00    0.00    0.00   0.00   0.00

Device:         rrqm/s   wrqm/s     r/s     w/s    rkB/s    wkB/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util
vda               0.00    41.00    0.00   18.00     0.00   228.00    25.33     0.01    0.78    0.00    0.78   0.44   0.80
vdb               0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00    0.00    0.00   0.00   0.00

      2 CLOSE_WAIT
      1 established)
     68 ESTABLISHED
      1 FIN_WAIT2
      1 Foreign
      8 LISTEN
    132 TIME_WAIT
      1 101.226.225.86
      1 101.226.66.181
      4 115.29.244.224
     33 116.230.188.166
      3 127.0.0.1
      1 140.205.140.205
     26 180.172.97.78
%CPU    PID     COMMAD
6.6  11086  /usr/local/mysql/bin/mysqld
7.4  25435  sidekiq
0.2  12190  /opt/gitlab/embedded/bin/redis-server
0.9  26464  /usr/local/cloudmonitor/jre/bin/java
0.1  31036  /usr/local/aegis/aegis_client/aegis_10_39/AliYunDun
%MEM    PID     COMMAD
7.4  25435  sidekiq
6.6  11086  /usr/local/mysql/bin/mysqld
5.7  4363  unicorn
5.1  12273  unicorn
5.1  12279  unicorn
```

## MySQL配置优化建议

### 修改服务端接收的数据包大小

```
#max_allowed_packet = 1M
max_allowed_packet = 32M
```

### 调整各类缓存配置

```
#table_open_cache = 256
table_open_cache = 400
table_definition_cache = 400
table_open_cache_instances = 64

#sort_buffer_size = 1M
sort_buffer_size = 4M

#read_buffer_size = 1M
read_buffer_size = 8M

#read_rnd_buffer_size = 512K
read_rnd_buffer_size = 4M

#myisam_sort_buffer_size = 16M
myisam_sort_buffer_size = 128M

#thread_cache_size = 32
thread_cache_size = 768

#query_cache_size = 32M
query_cache_size = 0
```

### 修改二进制日志格式

有利于误操作回滚

```
#binlog_format=mixed
binlog_format = row
```

### 调整innodb缓冲池

```
#innodb_buffer_pool_size = 256M
innodb_buffer_pool_size = 4014M

#innodb_log_buffer_size = 8M
innodb_log_buffer_size = 32M
```

### 调整逻辑备份配置

```
[mysqldump]
quick
#max_allowed_packet = 16M
max_allowed_packet = 32M
```

### 修改慢查询配置

开启未使用索引的查询记录

```
slow_query_log = ON
slow_query_log_file = /usr/local/mysql/log/slow.log
long_query_time = 1
# new
log_queries_not_using_indexes =1
```

### 优化后的配置文件

如下：

```
[client]
#password   = your_password
port        = 3306
socket      = /tmp/mysql.sock

[mysqld]
port        = 3306
socket      = /tmp/mysql.sock
datadir = /usr/local/mysql/var
skip-external-locking

key_buffer_size = 64M

#max_allowed_packet = 1M
max_allowed_packet = 32M

#table_open_cache = 256
table_open_cache = 400
table_definition_cache = 400
table_open_cache_instances = 64

#sort_buffer_size = 1M
sort_buffer_size = 4M


net_buffer_length = 8K

#read_buffer_size = 1M
read_buffer_size = 8M

#read_rnd_buffer_size = 512K
read_rnd_buffer_size = 4M

#myisam_sort_buffer_size = 16M
myisam_sort_buffer_size = 128M

#thread_cache_size = 32
thread_cache_size = 768

#query_cache_size = 32M
query_cache_size = 0

tmp_table_size = 64M


explicit_defaults_for_timestamp = true
#skip-networking
max_connect_errors = 100
open_files_limit = 65535

log-bin=mysql-bin

#binlog_format=mixed
binlog_format = row

server-id   = 1
expire_logs_days = 10


default_storage_engine = InnoDB
innodb_data_home_dir = /usr/local/mysql/var
innodb_data_file_path = ibdata1:10M:autoextend
innodb_log_group_home_dir = /usr/local/mysql/var

#innodb_buffer_pool_size = 256M
innodb_buffer_pool_size = 4014M

innodb_log_file_size = 64M

#innodb_log_buffer_size = 8M
innodb_log_buffer_size = 32M

innodb_flush_log_at_trx_commit = 1
innodb_lock_wait_timeout = 50

[mysqldump]
quick

#max_allowed_packet = 16M
max_allowed_packet = 32M

[mysql]
no-auto-rehash

[myisamchk]
key_buffer_size = 64M
sort_buffer_size = 1M
read_buffer = 2M
write_buffer = 2M

[mysqlhotcopy]
interactive-timeout

[mysqld]
slow_query_log = ON
slow_query_log_file = /usr/local/mysql/log/slow.log
long_query_time = 1
# new
log_queries_not_using_indexes =1
```

**注意点：修改配置文件后，需要重新启动使其生效，请告知服务器重启时间**

------

## MySQL备份建议

当前数据库中的数据量如下：

```
+---------------+---------------+--------------+------------+-----------+
| total_size_gb | index_size_gb | data_size_gb | perc_index | perc_data |
+---------------+---------------+--------------+------------+-----------+
|        3.1699 |        1.1100 |       2.0599 |      35.00 |     65.00 |
+---------------+---------------+--------------+------------+-----------+
```

### 备份建议

1. 鉴于数据库目前的数据量在3G左右，建议可以使用逻辑备份mysqldump，若后期数据增长较快，达到50G以外，则建议使用物理备份工具innobackupex。
2. 备份文件按要求保留近14天
3. 备份文件建议存放于本地，需要确保磁盘空间够用。

### 备份脚本

`backup.sh`如下

```
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
mysql -u$DBUser -p$DBPwd -e 'show databases;' | grep -Ev 'Database|information_schema|performance_schema|mysql|test|sys' | xargs mysqldump -u$DBUser -p$DBPwd --databases --opt --default-character-set=utf8  --single-transaction --hex-blob --skip-triggers --max_allowed_packet=824288000 > "$BackupPath"/"$BackupFile"
#Delete sql type file
find "$BackupPath" -name "$DBname*[log,sql]" -type f -mtime +14 -exec rm -rf {} \;
```

创建一个单独的备份用户backup，不要用root

```
mysql> grant select,reload,show databases,super,lock tables,replication client,show view,event,file on *.* to backup@'localhost' identified by 'abc123';
```

其中备份文件存放于`/alidata/backup`目录中。

**目前脚本中备份的库是去除系统库的，请确认是否需要备份系统库。**

### 定时任务

```
# crontab
[分] [时] * * * /bin/bash /usr/local/mysql/backup.sh
```

**请确定定时任务的执行时间，建议选择业务低峰期。**

## 后续事宜

请您根据建议进行审核，待您确认后，我方再开始操作。