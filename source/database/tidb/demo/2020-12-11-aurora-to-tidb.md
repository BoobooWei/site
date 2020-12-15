---
title: 同步 Aurora MySQL -> TiDB
---

## 版本记录

| 需求人 |   |
| ------ | ---- |
| 操作人 |   |
| 审核人 |    |

## 迁移需求

### 业务侧需求沟通记录


这两个表到dev 的 tidb集群；

### 业务侧需求整理

| 需求           | 源端                                                         | 目标端                                                       |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 连接地址       |  | |
| **数据库类型** | Aurora MySQL                                                 | TiDB                                                         |
| 角色           | [  ] Regional<br>[√] Serverless                              | -                                                            |
| 数据库版本     | 5.6.10                                                       | 5.7.25                                                       |
| 库名           |                                                          |                                                       |
| 表名           |                                 |                                |
| 结构           | √                                                            | √                                                            |
| 数据           | √                                                            | √                                                            |
| 迁移类型       | [ ]**数据迁移**： [  ]结构 [ ]全量<br>[√]**数据同步**：[√]结构 [√]全量 [√]增量 |                                                              |

* DM提供了数据迁移功能，类型选择：结构+全量。
* DM数据同步：提供数据同步功能，类型选择：结构+全量+增量。
* **注意**：Serverless 类型的Aurora MySQL 不支持binlog因此只能全量迁移。

## 迁移明细

### 源数据库调研

| 资源id                                    |                |
| ----------------------------------------- | ------------------------------------------------- |
| 资源名称                                  |                                      |
| 登陆方式                                  | [√] 已提供<br/>[  ] 未提供                        |
| 检查MySQL是否开启binlog                   | [  ] 已开启<br/>[  ] 未开启需开启<br>[√] 无法开启 |
| 检查MySQL是否创建用于同步TiDB的账号和权限 | [√] 已存在<br>[  ] 不存在需新建                   |
| 调研待迁移/同步的数据量                   | [√] 已调研<br/>[  ] 未调研                        |

#### 检查binlog

**执行以下SQL调研binlog日志情况：**

```sql
SELECT  
  'log_bin' as  parameter,
  @@log_bin  as  value, 'binlog开启情况 0 未开启 1 开启' as  info
UNION  
SELECT  
  'binlog_format',
  @@binlog_format, 'binlog格式';
```

**若未开启，应如何开启？**

* Aurora MySQL 数据库*（非 Serverless 类型）*默认开启binlog，如未开启，需要联系DBA开启binlog，需要与应用确认停机窗口应用; 

  修改方法：打开RDS控制台，选择修改对应参数组，更改 [binlog_format 参数的值](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_LogAccess.Concepts.MySQL.html#USER_LogAccess.MySQL.BinaryFormat)，将其更改至 ROW，保存后重启资源生效。

* Aurora MySQL 数据库*（Serverless 类型）*不支持开启binlog。

#### 检查账号与权限

检查方法：

```sql
select user,host from mysql.user where user='tidb';
show grants for 'tidb';
```

记录检查结果：

```co
已存在账号与权限，无需新建。
```

若未创建账号与数据库，请执行以下SQL创建账号并授权：

```sql
CREATE USER 'tidb'@'xxx' IDENTIFIED BY 'xxx';
GRANT SELECT, RELOAD, REPLICATION SLAVE, REPLICATION CLIENT ON swap.* TO 'tidb'@'xxx';
```

（可选）若需要[迁移延迟监控](https://docs.pingcap.com/zh/tidb-data-migration/v1.0/feature-overview#%E8%BF%81%E7%A7%BB%E5%BB%B6%E8%BF%9F%E7%9B%91%E6%8E%A7)，请执行以下SQL创建DM迁移心跳检测数据库：

```sql
CREATE DATABASE `dm_heartbeat` /*!40100 DEFAULT CHARACTER SET utf8mb4 */;
GRANT ALL PRIVILEGES ON `dm_heartbeat`.* TO 'tidb'@'xxx';
```

#### 调研数据量

执行以下SQL调研待迁移/同步的数据量：

```sql
SELECT
    t.table_name 表,
    t.table_schema 库,
    t.engine 引擎,
    t.table_length_B 表空间,
    t.data_length_B 数据空间,
    t.index_length_B 索引空间,
    t.table_rows 行数,
    t.avg_row_length_B 平均行长
FROM
    (
    SELECT
        table_name,
            table_schema,
            ENGINE,
            table_rows,
            data_length +  index_length AS table_length_B,
            data_length AS data_length_B,
            index_length AS index_length_B,
            AVG_ROW_LENGTH AS avg_row_length_B
    FROM
        information_schema.tables
    WHERE
        table_schema IN ('xx') and table_type != 'VIEW' and table_name in ('xx','xx')
        ) t;
```

> 运行结果截图保留。


### 目标数据库调研

| 资源id                   |       |
| ------------------------ | -------------------------- |
| 资源名称                 |      |
| 登陆方式                 | [√] 已提供<br/>[  ] 未提供 |
| 检查目标侧是否已存在库表 | [√] 已存在<br/>[  ] 不存在 |

#### 检查目标侧是否有重复表

1. 登陆后确认目标库中是否已存在待迁移/同步的表；
2. 与需求方确认目标侧待删除的表；
3. 确认后清理，所有操作截图留档。

### DM同步任务明细

#### 步骤概览

> 经过调研发现已为上游配置worker，因此只需要增加task。

1. 登陆AWS ec2
2. 切换用户 `sudo su - tidb`
3. 增加task配置文件 `touch /home/tidb/dm-ansible/conf/xx.yaml`
4. 进入dm管理 `/home/tidb/dm-ansible/dmctl/dmctl --master-addr=:8261`
5. 启动task 	`start-task /home/tidb/dm-ansible/conf/xx.yaml`
6. 查看task状态 `query-status swap_user_share_task`

#### 操作记录


#### 报错解决

从报错信息：`"errorMsg": "log_bin is OFF, and should be ON"`，登陆AWS控制台查看 Aurora MySQL 实例的角色发现为 `Serverless 类型`，不支持开启binlog。

#### 任务失败

将方案改为“逻辑导出与导入”。

### 逻辑导出与导入明细

#### 步骤概览

#### 操作记录

由于本次数据量非常小，所以直接选择 `mysql`导入，若量大可以选择TiDB 数据导入工具 [Lightning](https://book.tidb.io/session2/chapter2/lightning-internal.html) 。

#### 业务验证


### 任务速度

| 源库规格     | 128 capacity units |
| ------------ | ------------------------------------------------------- |
| 目标库规格   | i3.2xlarge           |
| 数据量       | 65K                                                     |
| 迁移方式     | mydumper/mysql                                          |
| 迁移时间     | -                                                       |
| 迁移速度     | -                                                       |
| 源库IOPS变化 | -                                                       |
| 源库CPU变化  | -                                                       |

## 其他信息

### DM已配置的上游数据库

### 常用命令

```bash
# dev tidb 
mysql -uroot -h 172.21.43.32 -P 4000 -p
# dev mysql
mysql -utidb -p -h -P3306
# mydumper mysql
/home/tidb/dm-ansible/resources/bin/mydumper --host  --port 3306 --user tidb --verbose 3 --threads 4 --chunk-filesize 64 --skip-tz-utc --logfile /tmp/xx.log -B swap -T user_share,user_share_change --password xxx  --no-locks > xx.sql
```


