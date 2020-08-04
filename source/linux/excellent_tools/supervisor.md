---
title: 通过supervisor来实现后台运行
---

# 简介

[supervisor](http://supervisord.org/introduction.html)是一个Linux/Unix系统上的进程监控工具，是一个Python开发的通用的进程管理程序，可以管理和监控Linux上面的进程，能将一个普通的命令行进程变为后台daemon，并监控进程状态，异常退出时能自动重启。

# 安装使用

```bash 
pip3 install supervisor
echo_supervisord_conf > /etc/supervisord/supervisord.conf
supervisord -c /etc/supervisord/supervisord.conf
supervisorctl status <programe_name>
```

# 案例参考

第一步：部署待运行的程序

```
[root@telegraf_test rds_for_mysql_to_influxdb]# tree /usr/local/cloudcare/rds_for_mysql_to_influxdb
/usr/local/cloudcare/rds_for_mysql_to_influxdb
├── api
│   ├── cms_api.py
│   ├── config.py
│   ├── influxdb_api.py
│   ├── __init__.py
│   ├── inputs_rds.py
│   ├── inputs_rds.py.bac
│   └── rds_api.py
├── __init__.py
├── manage.py
└── requirements.txt
```

第二步：Daemon程序部署

```bash
[root@telegraf_test ~]# pip3 install supervisor
[root@telegraf_test ~]# echo_supervisord_conf > /etc/supervisord/supervisord.conf
[root@telegraf_test ~]# vim /etc/supervisord/supervisord.conf
[root@telegraf_test ~]# supervisord -c /etc/supervisord/supervisord.conf
Unlinking stale socket /tmp/supervisor.sock
[root@telegraf_test supervisord]# ps -ef|grep super
root        18     2  0 Apr16 ?        00:00:06 [sync_supers]
root     20321     1  0 13:23 ?        00:00:00 /usr/local/python3/bin/python3.7 /usr/local/python3/bin/supervisord -c /etc/supervisord/supervisord.conf
root     20330 16923  0 13:23 pts/1    00:00:00 grep super
[root@telegraf_test supervisord]# supervisorctl status inputsrds
inputsrds                        EXITED    May 26 01:23 PM
[root@telegraf_test ~]# grep -v '^;' /etc/supervisord/supervisord.conf
 
[unix_http_server]
file=/tmp/supervisor.sock   ; the path to the socket file
 
 
[supervisord]
logfile=/tmp/supervisord.log ; main log file; default $CWD/supervisord.log
logfile_maxbytes=50MB        ; max main logfile bytes b4 rotation; default 50MB
logfile_backups=10           ; # of main logfile backups; 0 means none, default 10
loglevel=info                ; log level; default info; others: debug,warn,trace
pidfile=/tmp/supervisord.pid ; supervisord pidfile; default supervisord.pid
nodaemon=false               ; start in foreground if true; default false
minfds=1024                  ; min. avail startup file descriptors; default 1024
minprocs=200                 ; min. avail process descriptors;default 200
 
 
[rpcinterface:supervisor]
supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface
 
 
[supervisorctl]
serverurl=unix:///tmp/supervisor.sock ; use a unix:// URL  for a unix socket
 
[program:inputsrds]
command=/usr/local/python3/bin/python3 /usr/local/cloudcare/rds_for_mysql_to_influxdb/manage.py              ; the program (relative uses PATH, can take args)
directory=/usr/local/cloudcare/rds_for_mysql_to_influxdb                ; directory to cwd to before exec (def no cwd)
autostart=true                ; start at supervisord start (default: true)
stdout_logfile=/usr/local/cloudcare/rds_for_mysql_to_influxdb/inputsrds.log        ; stdout log path, NONE for none; default AUTO
stdout_logfile_maxbytes=1MB   ; max # logfile bytes b4 rotation (default 50MB)
stdout_logfile_backups=10     ; # of stdout logfile backups (0 means none, default 10)
stderr_logfile=/usr/local/cloudcare/rds_for_mysql_to_influxdb/inputsrds.err        ; stderr log path, NONE for none; default AUTO
stderr_logfile_maxbytes=1MB   ; max # logfile bytes b4 rotation (default 50MB)
stderr_logfile_backups=10     ; # of stderr logfile backups (0 means none, default 10)
 
 
[root@telegraf_test rds_for_mysql_to_influxdb]# tailf inputsrds.log
]
run_time 1558848201.675511
last_manage_time 1558845484.8113325
last_get_top_10_time 1558845484.8113325
last_get_all_slow_time 1558845484.8113325
get_top_10 running at 1558845484.8113325
get_all_slow running at 1558845484.8113325
get_rds_basic running at 1558848201.675511
manage running at 1558848201.675511
```
