---
title: mysql_5.7_分区表物理删除报错解决
---

> MySQL 5.7

# 误操作重现

1. 生产中创建分区表

```sql
CREATE TABLE `t1` (
       `id` int(11) NOT NULL,
       `col1` int(11) NOT NULL,
       PRIMARY KEY (`id`,`col1`)
     ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4
     /*!50100 PARTITION BY RANGE (col1)
     (PARTITION p100 VALUES LESS THAN (100) ENGINE = InnoDB,
      PARTITION p101 VALUES LESS THAN (200) ENGINE = InnoDB) */;
```	  

2. 物理文件被手动删除后，数据库报错

```sql
root@MySQL-01 15:19:  [test01]> drop table t1;
ERROR 1051 (42S02): Unknown table 'test01.t1'
```
	  
# 解决步骤  
	  	  
1. 新实例（注意版本一致）上创建分区表 `t1`；
2. 从新实例的数据目录中`copy t1.frm` 文件到生产实例对应目录下，注意权限 可以使用`cp -rp` 保留原有权限，也可以`chown mysql.`
3. 生产实例执行 `drop table t1;`
