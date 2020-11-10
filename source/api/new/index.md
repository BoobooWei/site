---
title: MySQL 8.0的新增功能探索
date: 2020-05-26T17:59:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

自从2005年Oracle收购InnoDB存储引擎开发商Innobase伊始，MySQL的命运已然注定。

随着MySQL 5.6开始的源码重构，MySQL已经驶上了快车道，MGR给了PXC致命一击，而Clone Plugin的推出宣判了MariaDB的死刑。MariaDB，或许只能拿来缅怀曾经的青春吧。

## 新增功能探索

官网链接

- [MySQL 8.0中添加的功能](https://dev.mysql.com/doc/refman/8.0/en/mysql-nutshell.html#mysql-nutshell-additions)
- [MySQL 8.0中不推荐使用的功能](https://dev.mysql.com/doc/refman/8.0/en/mysql-nutshell.html#mysql-nutshell-deprecations)
- [MySQL 8.0中删除的功能](https://dev.mysql.com/doc/refman/8.0/en/mysql-nutshell.html#mysql-nutshell-removals)


## 本地环境准备

### 配置YUM源并安装

测试环境中使用YUM安装速度较快，真实生产中建议二进制包安装。

```bash
# redhat 7.3
wget https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm
yum localinstall -y mysql80-community-release-el7-3.noarch.rpm
yum repolist enabled | grep "mysql.*-community.*"
yum-config-manager --enable mysql80-community
yum-config-manager --disable mysql57-community
yum install mysql-community-server
```

### 查看软件结构

```bash
[root@foundation0 install]# rpm -ql mysql-community-server
/etc/logrotate.d/mysql
/etc/my.cnf
/etc/my.cnf.d
/usr/bin/ibd2sdi
/usr/bin/innochecksum
/usr/bin/lz4_decompress
/usr/bin/my_print_defaults
/usr/bin/myisam_ftdump
/usr/bin/myisamchk
/usr/bin/myisamlog
/usr/bin/myisampack
/usr/bin/mysql_secure_installation
/usr/bin/mysql_ssl_rsa_setup
/usr/bin/mysql_tzinfo_to_sql
/usr/bin/mysql_upgrade
/usr/bin/mysqld_pre_systemd
/usr/bin/mysqldumpslow
/usr/bin/perror
/usr/bin/zlib_decompress
/usr/lib/systemd/system/mysqld.service
/usr/lib/systemd/system/mysqld@.service
/usr/lib/tmpfiles.d/mysql.conf
/usr/lib64/mysql/mecab
/usr/lib64/mysql/mecab/dic
/usr/lib64/mysql/mecab/dic/ipadic_euc-jp
/usr/lib64/mysql/mecab/dic/ipadic_euc-jp/char.bin
/usr/lib64/mysql/mecab/dic/ipadic_euc-jp/dicrc
/usr/lib64/mysql/mecab/dic/ipadic_euc-jp/left-id.def
/usr/lib64/mysql/mecab/dic/ipadic_euc-jp/matrix.bin
/usr/lib64/mysql/mecab/dic/ipadic_euc-jp/pos-id.def
/usr/lib64/mysql/mecab/dic/ipadic_euc-jp/rewrite.def
/usr/lib64/mysql/mecab/dic/ipadic_euc-jp/right-id.def
/usr/lib64/mysql/mecab/dic/ipadic_euc-jp/sys.dic
/usr/lib64/mysql/mecab/dic/ipadic_euc-jp/unk.dic
/usr/lib64/mysql/mecab/dic/ipadic_sjis
/usr/lib64/mysql/mecab/dic/ipadic_sjis/char.bin
/usr/lib64/mysql/mecab/dic/ipadic_sjis/dicrc
/usr/lib64/mysql/mecab/dic/ipadic_sjis/left-id.def
/usr/lib64/mysql/mecab/dic/ipadic_sjis/matrix.bin
/usr/lib64/mysql/mecab/dic/ipadic_sjis/pos-id.def
/usr/lib64/mysql/mecab/dic/ipadic_sjis/rewrite.def
/usr/lib64/mysql/mecab/dic/ipadic_sjis/right-id.def
/usr/lib64/mysql/mecab/dic/ipadic_sjis/sys.dic
/usr/lib64/mysql/mecab/dic/ipadic_sjis/unk.dic
/usr/lib64/mysql/mecab/dic/ipadic_utf-8
/usr/lib64/mysql/mecab/dic/ipadic_utf-8/char.bin
/usr/lib64/mysql/mecab/dic/ipadic_utf-8/dicrc
/usr/lib64/mysql/mecab/dic/ipadic_utf-8/left-id.def
/usr/lib64/mysql/mecab/dic/ipadic_utf-8/matrix.bin
/usr/lib64/mysql/mecab/dic/ipadic_utf-8/pos-id.def
/usr/lib64/mysql/mecab/dic/ipadic_utf-8/rewrite.def
/usr/lib64/mysql/mecab/dic/ipadic_utf-8/right-id.def
/usr/lib64/mysql/mecab/dic/ipadic_utf-8/sys.dic
/usr/lib64/mysql/mecab/dic/ipadic_utf-8/unk.dic
/usr/lib64/mysql/mecab/etc
/usr/lib64/mysql/mecab/etc/mecabrc
/usr/lib64/mysql/plugin
/usr/lib64/mysql/plugin/adt_null.so
/usr/lib64/mysql/plugin/auth_socket.so
/usr/lib64/mysql/plugin/authentication_ldap_sasl_client.so
/usr/lib64/mysql/plugin/component_audit_api_message_emit.so
/usr/lib64/mysql/plugin/component_log_filter_dragnet.so
/usr/lib64/mysql/plugin/component_log_sink_json.so
/usr/lib64/mysql/plugin/component_log_sink_syseventlog.so
/usr/lib64/mysql/plugin/component_mysqlbackup.so
/usr/lib64/mysql/plugin/component_validate_password.so
/usr/lib64/mysql/plugin/connection_control.so
/usr/lib64/mysql/plugin/ddl_rewriter.so
/usr/lib64/mysql/plugin/debug
/usr/lib64/mysql/plugin/debug/adt_null.so
/usr/lib64/mysql/plugin/debug/auth_socket.so
/usr/lib64/mysql/plugin/debug/authentication_ldap_sasl_client.so
/usr/lib64/mysql/plugin/debug/component_audit_api_message_emit.so
/usr/lib64/mysql/plugin/debug/component_log_filter_dragnet.so
/usr/lib64/mysql/plugin/debug/component_log_sink_json.so
/usr/lib64/mysql/plugin/debug/component_log_sink_syseventlog.so
/usr/lib64/mysql/plugin/debug/component_mysqlbackup.so
/usr/lib64/mysql/plugin/debug/component_validate_password.so
/usr/lib64/mysql/plugin/debug/connection_control.so
/usr/lib64/mysql/plugin/debug/ddl_rewriter.so
/usr/lib64/mysql/plugin/debug/group_replication.so
/usr/lib64/mysql/plugin/debug/ha_example.so
/usr/lib64/mysql/plugin/debug/ha_mock.so
/usr/lib64/mysql/plugin/debug/innodb_engine.so
/usr/lib64/mysql/plugin/debug/keyring_file.so
/usr/lib64/mysql/plugin/debug/keyring_udf.so
/usr/lib64/mysql/plugin/debug/libmemcached.so
/usr/lib64/mysql/plugin/debug/libpluginmecab.so
/usr/lib64/mysql/plugin/debug/locking_service.so
/usr/lib64/mysql/plugin/debug/mypluglib.so
/usr/lib64/mysql/plugin/debug/mysql_clone.so
/usr/lib64/mysql/plugin/debug/mysql_no_login.so
/usr/lib64/mysql/plugin/debug/rewrite_example.so
/usr/lib64/mysql/plugin/debug/rewriter.so
/usr/lib64/mysql/plugin/debug/semisync_master.so
/usr/lib64/mysql/plugin/debug/semisync_slave.so
/usr/lib64/mysql/plugin/debug/validate_password.so
/usr/lib64/mysql/plugin/debug/version_token.so
/usr/lib64/mysql/plugin/group_replication.so
/usr/lib64/mysql/plugin/ha_example.so
/usr/lib64/mysql/plugin/ha_mock.so
/usr/lib64/mysql/plugin/innodb_engine.so
/usr/lib64/mysql/plugin/keyring_file.so
/usr/lib64/mysql/plugin/keyring_udf.so
/usr/lib64/mysql/plugin/libmemcached.so
/usr/lib64/mysql/plugin/libpluginmecab.so
/usr/lib64/mysql/plugin/locking_service.so
/usr/lib64/mysql/plugin/mypluglib.so
/usr/lib64/mysql/plugin/mysql_clone.so
/usr/lib64/mysql/plugin/mysql_no_login.so
/usr/lib64/mysql/plugin/rewrite_example.so
/usr/lib64/mysql/plugin/rewriter.so
/usr/lib64/mysql/plugin/semisync_master.so
/usr/lib64/mysql/plugin/semisync_slave.so
/usr/lib64/mysql/plugin/validate_password.so
/usr/lib64/mysql/plugin/version_token.so
/usr/lib64/mysql/private
/usr/lib64/mysql/private/libprotobuf-lite.so.3.6.1
/usr/lib64/mysql/private/libprotobuf.so.3.6.1
/usr/sbin/mysqld
/usr/sbin/mysqld-debug
/usr/share/doc/mysql-community-server-8.0.20
/usr/share/doc/mysql-community-server-8.0.20/INFO_BIN
/usr/share/doc/mysql-community-server-8.0.20/INFO_SRC
/usr/share/doc/mysql-community-server-8.0.20/LICENSE
/usr/share/doc/mysql-community-server-8.0.20/README
/usr/share/man/man1/ibd2sdi.1.gz
/usr/share/man/man1/innochecksum.1.gz
/usr/share/man/man1/lz4_decompress.1.gz
/usr/share/man/man1/my_print_defaults.1.gz
/usr/share/man/man1/myisam_ftdump.1.gz
/usr/share/man/man1/myisamchk.1.gz
/usr/share/man/man1/myisamlog.1.gz
/usr/share/man/man1/myisampack.1.gz
/usr/share/man/man1/mysql.server.1.gz
/usr/share/man/man1/mysql_secure_installation.1.gz
/usr/share/man/man1/mysql_ssl_rsa_setup.1.gz
/usr/share/man/man1/mysql_tzinfo_to_sql.1.gz
/usr/share/man/man1/mysql_upgrade.1.gz
/usr/share/man/man1/mysqldumpslow.1.gz
/usr/share/man/man1/mysqlman.1.gz
/usr/share/man/man1/perror.1.gz
/usr/share/man/man1/zlib_decompress.1.gz
/usr/share/man/man8/mysqld.8.gz
/usr/share/mysql-8.0/dictionary.txt
/usr/share/mysql-8.0/innodb_memcached_config.sql
/usr/share/mysql-8.0/install_rewriter.sql
/usr/share/mysql-8.0/mysql-log-rotate
/usr/share/mysql-8.0/uninstall_rewriter.sql
/var/lib/mysql
/var/lib/mysql-files
/var/lib/mysql-keyring
/var/run/mysqld
```

### 启动服务

```bash
systemctl start mysqld
```

### 查看服务进程和监听端口

```bash
[root@foundation0 install]# systemctl status mysqld
● mysqld.service - MySQL Server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
   Active: active (running) since 一 2020-06-01 17:46:52 CST; 7s ago
     Docs: man:mysqld(8)
           http://dev.mysql.com/doc/refman/en/using-systemd.html
  Process: 11627 ExecStartPre=/usr/bin/mysqld_pre_systemd (code=exited, status=0/SUCCESS)
 Main PID: 11903 (mysqld)
   Status: "Server is operational"
   CGroup: /system.slice/mysqld.service
           └─11903 /usr/sbin/mysqld

6月 01 17:46:30 foundation0.ilt.example.com systemd[1]: Starting MySQL Server...
6月 01 17:46:52 foundation0.ilt.example.com systemd[1]: Started MySQL Server.
[root@foundation0 install]# ps -ef|grep mysqld
mysql    11903     1  6 17:46 ?        00:00:00 /usr/sbin/mysqld
root     12029  7260  0 17:47 pts/0    00:00:00 grep --color=auto mysqld
[root@foundation0 install]# ss -luntp|grep mysqld
tcp    LISTEN     0      128      :::3306                 :::*                   users:(("mysqld",pid=11903,fd=33))
tcp    LISTEN     0      70       :::33060                :::*                   users:(("mysqld",pid=11903,fd=31))
```

可以看到MySQL的监听端口多了一个`33060`，原因参见官网()

### 使用临时密码登陆并修改密码

```bash
less /var/log/mysqld.log
020-06-01T09:46:39.152942Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: XScpXMk1!pGy
2020-06-01T09:46:51.114590Z 0 [System] [MY-010116] [Server] /usr/sbin/mysqld (mysqld 8.0.20) starting as process 11903
[root@foundation0 log]# mysql -uroot -p'XScpXMk1!pGy'
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 9
Server version: 8.0.20

Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> select user,host from mysql.user;
ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.

mysql> alter user root@localhost identified by 'Zyadmin@123';
Query OK, 0 rows affected (0.02 sec)

[root@foundation0 log]# mysql -uroot -pZyadmin@123
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 10
Server version: 8.0.20 MySQL Community Server - GPL

Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> select user,host,plugin from mysql.user;
+------------------+-----------+-----------------------+
| user             | host      | plugin                |
+------------------+-----------+-----------------------+
| mysql.infoschema | localhost | caching_sha2_password |
| mysql.session    | localhost | caching_sha2_password |
| mysql.sys        | localhost | caching_sha2_password |
| root             | localhost | caching_sha2_password |
+------------------+-----------+-----------------------+
4 rows in set (0.00 sec)


[root@foundation0 log]# mysql -uroot -pZyadmin@123 -P33060 -e "select @@hostname"
mysql: [Warning] Using a password on the command line interface can be insecure.
+-----------------------------+
| @@hostname                  |
+-----------------------------+
| foundation0.ilt.example.com |
+-----------------------------+

```
