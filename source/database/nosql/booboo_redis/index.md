---
title: Redis 基础
---

> Redis 基础课程是2015年准备的一套基础讲义。主要讲解 3.0 版本的基础命令、持久化、主从、集群和排错指南。
> 代码部分可以访问：https://github.com/BoobooWei/booboo_redis

# 内容

* [01-Redis入门](/database/nosql/booboo_redis/01-Redis入门.html)
* [02-Redis持久化](/database/nosql/booboo_redis/02-Redis持久化.html)
* [03-Redis主从](/database/nosql/booboo_redis/03-Redis主从.html)
* [04-Redis集群](/database/nosql/booboo_redis/04-Redis集群.html)
* [05-Redis排错指南](/database/nosql/booboo_redis/05-Redis排错指南.html)


```bash
 ll *.md | awk '{print "* ["$9"](/database/nosql/booboo_redis/"$9")"}' | sed 's/.md//'|sed 's/.md/.html/g'

```
