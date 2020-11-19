---
title: 2018.10.8.0.xxx.自建MySQL逻辑备份计划生效核实
---

# 检查逻辑备份

```bash
[zyadmin@zeji-git-server ~]$ ll /alidata/backup/ -h
total 1.6G
-rw-r--r-- 1 root root 1.6G Jan  3 00:01 mysql-180103_00.sql
[zyadmin@zeji-git-server ~]$ file /alidata/backup/mysql-180103_00.sql
/alidata/backup/mysql-180103_00.sql: UTF-8 Unicode English text, with very long lines
[zyadmin@zeji-git-server ~]$ head -n 50 /alidata/backup/mysql-180103_00.sql
-- MySQL dump 10.13 Distrib 5.6.29, for Linux (x86_64)
--
-- Host: localhost   Database:
-- ------------------------------------------------------
-- Server version       5.6.29-log
 
/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!40101 SET NAMES utf8 */;
/*!40103 SET @OLD_TIME_ZONE=@@TIME_ZONE */;
/*!40103 SET TIME_ZONE='+00:00' */;
/*!40014 SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0 */;
/*!40014 SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0 */;
/*!40101 SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_AUTO_VALUE_ON_ZERO' */;
/*!40111 SET @OLD_SQL_NOTES=@@SQL_NOTES, SQL_NOTES=0 */;
 
--
-- Current Database: `mysql`
--
 
CREATE DATABASE /*!32312 IF NOT EXISTS*/ `mysql` /*!40100 DEFAULT CHARACTER SET utf8mb4 */;
 
USE `mysql`;
 
--
-- Table structure for table `columns_priv`
--
 
DROP TABLE IF EXISTS `columns_priv`;
/*!40101 SET @saved_cs_client     = @@character_set_client */;
/*!40101 SET character_set_client = utf8 */;
CREATE TABLE `columns_priv` (
  `Host` char(60) COLLATE utf8_bin NOT NULL DEFAULT '',
  `Db` char(64) COLLATE utf8_bin NOT NULL DEFAULT '',
  `User` char(16) COLLATE utf8_bin NOT NULL DEFAULT '',
  `Table_name` char(64) COLLATE utf8_bin NOT NULL DEFAULT '',
  `Column_name` char(64) COLLATE utf8_bin NOT NULL DEFAULT '',
  `Timestamp` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  `Column_priv` set('Select','Insert','Update','References') CHARACTER SET utf8 NOT NULL DEFAULT '',
 PRIMARY KEY (`Host`,`Db`,`User`,`Table_name`,`Column_name`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='Column privileges';
/*!40101 SET character_set_client = @saved_cs_client */;
 
--
-- Dumping data for table `columns_priv`
--
 
LOCK TABLES `columns_priv` WRITE;
/*!40000 ALTER TABLE `columns_priv` DISABLE KEYS */;
```

# 总结

目前`/alidata/backup/mysql-180103_00.sql` 逻辑备份已生成，大小在1.6G。

备份计划已生效。

备注：DBA组负责备份计划的设定建议，并在业务侧审核的基础上进行备份计划设定，且已确定备份计划的生效。另外建议在备份计划生效后对数据库备份恢复进行管控，保证将来数据库恢复的可行性。
