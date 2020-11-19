---
title: Amy
---

> 记录Amy的生活小事

* [Amy经典语录-第四篇](/amy/withme/2020-02-07-amy.html)
* [Amy经典语录-第五篇](/amy/withme/2020-04-29-amy.html)
* [Amy经典语录-第六篇](/amy/withme/2020-05-08-amy.html)
* [团队家庭聚餐日](/amy/withme/2020-05-18-amy.html)

```bash
ll *.md | awk '{print "* ["$9"](/amy/withme/"$9")"}' | sed 's/.md//'|sed 's/.md/.html/g'
```
