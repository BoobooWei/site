---
title: MySQL 8.0的新增功能探索-行和列别名具有ON DUPLICATE KEY UPDATE
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

- **行和列别名具有ON DUPLICATE KEY UPDATE。** 从MySQL 8.0.19开始，可以使用别名引用要插入的行，以及（可选）引用其列。考虑在具有列 和[`INSERT`](https://dev.mysql.com/doc/refman/8.0/en/insert.html)的表`t`上的以下 语句 ： `a``b`

```sql
  INSERT INTO t SET a=9,b=5
      ON DUPLICATE KEY UPDATE a=VALUES(a)+VALUES(b);
```

使用`new`新行的别名，在某些情况下，使用别名`m`以及 `n`该行的列，`INSERT`可以用许多不同的方式重写该 语句，此处显示了一些示例：

```sql
  INSERT INTO t SET a=9,b=5 AS new
      ON DUPLICATE KEY UPDATE a=new.a+new.b;

  INSERT INTO t VALUES(9,5) AS new
      ON DUPLICATE KEY UPDATE a=new.a+new.b;

  INSERT INTO t SET a=9,b=5 AS new(m,n)
      ON DUPLICATE KEY UPDATE a=m+n;

  INSERT INTO t VALUES(9,5) AS new(m,n)
      ON DUPLICATE KEY UPDATE a=m+n;
```

欲了解更多信息和示例，请参见 [第13.2.6.2，"INSERT ... ON DUPLICATE KEY UPDATE语句"](https://dev.mysql.com/doc/refman/8.0/en/insert-on-duplicate.html)。
