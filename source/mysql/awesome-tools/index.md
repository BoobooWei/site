---
title: MySQL 工具篇
---

> 记录 MySQL生产环境 中使用的优秀工具技巧和坑点

* [binlog2sql](/mysql/awesome-tools/binlog2sql.html)
* [myflash](/mysql/awesome-tools/myflash.html)
* [percona_tookit](/mysql/awesome-tools/percona_tookit.html)
* [pt-archiver](/mysql/awesome-tools/pt-archiver.html)
* [workbench](/mysql/awesome-tools/workbench.html)


```bash
ll *.md | awk '{print "* ["$9"](/mysql/awesome-tools/"$9")"}' | sed 's/.md//'|sed 's/.md/.html/g'
```
