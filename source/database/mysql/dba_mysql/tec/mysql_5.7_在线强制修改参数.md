---
title: MySQL 5.7 在线强制修改参数
---

1. 修改配置文件
2. 执行命令
```bash
mysql> system gdb -p $(pidof mysqld) -ex "set opt_log_slave_updates=0" -batch
```
