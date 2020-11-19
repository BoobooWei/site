---
title: MySQL 运维实践
---

> MySQL 运维实践用于记录总结工作的 MySQL 运维实践内容。

* [00-MySQL软件推荐](/database/mysql/dba_mysql/00-MySQL软件推荐.html)
* [01-MySQL常用SQL](/database/mysql/dba_mysql/01-MySQL常用SQL.html)
* [02-MySQL参数优化](/database/mysql/dba_mysql/02-MySQL参数优化.html)
* [03-MySQL认证OCP](/database/mysql/dba_mysql/03-MySQL认证OCP.html)
* [04-MySQL技术积累](/database/mysql/dba_mysql/04-MySQL技术积累.html)
* [05-MySQL自动化管理](/database/mysql/dba_mysql/05-MySQL自动化管理.html)
* [06-MySQL性能监控](/database/mysql/dba_mysql/06-MySQL性能监控.html)
* [07-MySQL系统库SYS](/database/mysql/dba_mysql/07-MySQL系统库SYS.html)
* [08-MySQL管理规范](/database/mysql/dba_mysql/08-MySQL管理规范.html)



```bash
ll *.md | awk '{print "* ["$9"](/database/mysql/dba_mysql/"$9")"}' | sed 's/.md//'|sed 's/.md/.html/g'
```
