---
title: 开源工具-binlog2sql工具
categories:
    - [技术广角, MySQL]
tags:
    - 紧急救援
    - binlog2sql
date: 2020-04-24 10:44:33
---

## 开源工具-binlog2sql工具

从MySQL binlog解析出你要的SQL。根据不同选项，你可以得到原始SQL、回滚SQL、去除主键的INSERT SQL等。

### 用途

* 数据快速回滚（闪回）
* 主从切换后master丢数据的修复
* 从binlog生成标准SQL，带来的衍生功能

### 限制

* mysql server必须开启，离线模式下不能解析
* 参数 binlog_row_image 必须为FULL，暂不支持MINIMAL
* 解析速度不如mysqlbinlog

### 安装

```bash
# centos7
# python2.7
git clone https://github.com/danfengcao/binlog2sql.git && cd binlog2sql
yum -y install epel-release
yum -y install python-pip
pip install --upgrade pip
pip install -r requirements.txt 
```

### 对MySQL的使用限制

MySQL server必须设置以下参数:

```sql
[mysqld]
server_id = 1
log_bin = /var/log/mysql/mysql-bin.log
max_binlog_size = 1G
binlog_format = row
binlog_row_image = full
```


user需要的最小权限集合：

```sql
select, super/replication client, replication slave
```

建议授权

```sql
GRANT SELECT, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 
```

## binlog2sql帮助

```bash
mysql连接配置
-h host; -P port; -u user; -p password
解析模式
--stop-never 持续同步binlog。可选。不加则同步至执行命令时最新的binlog位置。
-K, --no-primary-key 对INSERT语句去除主键。可选。
-B, --flashback 生成回滚语句，可解析大文件，不受内存限制，每打印一千行加一句SLEEP SELECT(1)。可选。与stop-never或no-primary-key不能同时添加。
解析范围控制
--start-file 起始解析文件。必须。
--start-position/--start-pos start-file的起始解析位置。可选。默认为start-file的起始位置。
--stop-file/--end-file 末尾解析文件。可选。默认为start-file同一个文件。若解析模式为stop-never，此选项失效。
--stop-position/--end-pos stop-file的末尾解析位置。可选。默认为stop-file的最末位置；若解析模式为stop-never，此选项失效。
--start-datetime 从哪个时间点的binlog开始解析，格式必须为datetime，如'2016-11-11 11:11:11'。可选。默认不过滤。
--stop-datetime 到哪个时间点的binlog停止解析，格式必须为datetime，如'2016-11-11 11:11:11'。可选。默认不过滤。
对象过滤
-d, --databases 只输出目标db的sql。可选。默认为空。
-t, --tables 只输出目标tables的sql。可选。默认为空。
```

## 案例
 
#### 解析标准SQL

```bash
python binlog2sql.py -h127.0.0.1 -P3306 -uroot -dhjxdb -t user --start-file='mysql-bin.000001' --start-datetime='2017-09-14 13:50:00' --stop-datetime='2017-09-14 14:10:00'
```

### 解析出回滚SQL

* `-B`: 参数代表回滚

```bash
python binlog2sql.py -h127.0.0.1 -P3306 -uroot -dhjxdb -t user --start-file='mysql-bin.000001' --start-position=4946 --stop-position=5921 -B 
```

工具是给人用的，关键还得有思路。加油⛽️
