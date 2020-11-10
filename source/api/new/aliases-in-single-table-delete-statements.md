---
title: 单表DELETE语句支持表别名
date: 2020-06-02T09:20:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

## 功能描述

{% note info 单表DELETE语句中的别名%}
在MySQL 8.0.16和更高版本中，单表 [`DELETE`](https://dev.mysql.com/doc/refman/8.0/en/delete.html)语句支持使用表别名。
{% endnote %}

## 功能学习

### Single-Table Syntax

```SQL
DELETE [LOW_PRIORITY] [QUICK] [IGNORE] FROM tbl_name [[AS] tbl_alias]
    [PARTITION (partition_name [, partition_name] ...)]
    [WHERE where_condition]
    [ORDER BY ...]
    [LIMIT row_count]
```


### 版本对比

#### < 8.0.16

```SQL
# 5.7.20 加别名报错
root@MySQL-01 09:34:  [test01]> delete from zytest as zzz where orderdate='2014/6/8 0:00';
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'as zzz where orderdate='2014/6/8 0:00'' at line 1

## MySQL5.6/MySQL5.7  不加别名执行正常
root@MySQL-01 09:37:  [test01]> delete from zytest where orderdate='2014/6/8 0:00';
Query OK, 8 rows affected (0.10 sec)

root@MySQL-01 09:37:  [test01]> select row_count();
+-------------+
| row_count() |
+-------------+
|           8 |
+-------------+
1 row in set (0.00 sec)

```

#### >= 8.0.16

```SQL
# 8.0.20
mysql> delete from t1_d as bbb where id<1;
Query OK, 0 rows affected (0.00 sec)
```

