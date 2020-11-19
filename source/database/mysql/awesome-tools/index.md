---
title: MySQL 工具篇
---

> 记录 MySQL生产环境 中使用的优秀工具技巧和坑点

* [binlog2sql](/database/mysql/awesome-tools/binlog2sql.html)
* [myflash](/database/mysql/awesome-tools/myflash.html)
* [percona_tookit](/database/mysql/awesome-tools/percona_tookit.html)
* [pt-archiver](/database/mysql/awesome-tools/pt-archiver.html)
* [workbench](/database/mysql/awesome-tools/workbench.html)


```bash
ll *.md | awk '{print "* ["$9"](/database/mysql/awesome-tools/"$9")"}' | sed 's/.md//'|sed 's/.md/.html/g'
```
