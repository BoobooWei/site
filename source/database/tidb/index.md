---
title: TiDB 学习
---

# TiDB 学习笔记

* [00_tidb入门指南](/database/tidb/00_tidb入门指南.html)
* [01_raft协议理解](/database/tidb/01_raft协议理解.html)
* [02_tidb21天课程](/database/tidb/tidb_courses/index.html)

```bash
ll *.md | awk '{print "* ["$9"](/database/tidb/"$9")"}' | sed 's/.md//'|sed 's/.md/.html/g'
```

# 官方帮助

## 视频教程

* [TiDB Courses for Beginners: based on version 4.0（TiDB 4.0 新手指南）](https://university.pingcap.com/courses/TiDB%204.0%20%E6%96%B0%E6%89%8B%E6%8C%87%E5%8D%97)
* [TiDB Courses for Application Developers: based on version 4.0（TiDB 4.0 应用开发指南）](https://university.pingcap.com/courses/TiDB%204.0%20%E5%BA%94%E7%94%A8%E5%BC%80%E5%8F%91%E6%8C%87%E5%8D%97)

## Gitbook

* [TiDB In Action: based on 4.0](https://book.tidb.io/)

## 用户文档

* [TiDB Data Migration 用户文档 v1.0 ](https://docs.pingcap.com/zh/tidb-data-migration/v1.0/)
* [TiDB Data Migration 用户文档 v2.0](https://docs.pingcap.com/zh/tidb-data-migration/stable/)

## 官方源码

### TiDB 源码阅读

- [TiDB 源码阅读系列文章（一）序](https://pingcap.com/blog-cn/tidb-source-code-reading-1/)
- [TiDB 源码阅读系列文章（二）初识 TiDB 源码](https://pingcap.com/blog-cn/tidb-source-code-reading-2/)
- [TiDB 源码阅读系列文章（三）SQL 的一生](https://pingcap.com/blog-cn/tidb-source-code-reading-3/)
- [TiDB 源码阅读系列文章（四）Insert 语句概览](https://pingcap.com/blog-cn/tidb-source-code-reading-4/)
- [TiDB 源码阅读系列文章（五）TiDB SQL Parser 的实现](https://pingcap.com/blog-cn/tidb-source-code-reading-5/)
- [TiDB 源码阅读系列文章（六）Select 语句概览](https://pingcap.com/blog-cn/tidb-source-code-reading-6/)
- [TiDB 源码阅读系列文章（七）基于规则的优化](https://pingcap.com/blog-cn/tidb-source-code-reading-7/)
- [TiDB 源码阅读系列文章（八）基于代价的优化](https://pingcap.com/blog-cn/tidb-source-code-reading-8/)
- [TiDB 源码阅读系列文章（九）Hash Join](https://pingcap.com/blog-cn/tidb-source-code-reading-9/)
- [TiDB 源码阅读系列文章（十）Chunk 和执行框架简介](https://pingcap.com/blog-cn/tidb-source-code-reading-10/)
- [TiDB 源码阅读系列文章（十一）Index Lookup Join](https://pingcap.com/blog-cn/tidb-source-code-reading-11/)
- [TiDB 源码阅读系列文章（十二）统计信息(上)](https://pingcap.com/blog-cn/tidb-source-code-reading-12/)
- [TiDB 源码阅读系列文章（十三）索引范围计算简介](https://pingcap.com/blog-cn/tidb-source-code-reading-13/)
- [TiDB 源码阅读系列文章（十四）统计信息（下）](https://pingcap.com/blog-cn/tidb-source-code-reading-14/)
- [TiDB 源码阅读系列文章（十五）Sort Merge Join](https://pingcap.com/blog-cn/tidb-source-code-reading-15/)
- [TiDB 源码阅读系列文章（十六）INSERT 语句详解](https://pingcap.com/blog-cn/tidb-source-code-reading-16/)
- [TiDB 源码阅读系列文章（十七）DDL 源码解析](https://pingcap.com/blog-cn/tidb-source-code-reading-17/)
- [TiDB 源码阅读系列文章（十八）tikv-client（上）](https://pingcap.com/blog-cn/tidb-source-code-reading-18/)
- [TiDB 源码阅读系列文章（十九）tikv-client（下）](https://pingcap.com/blog-cn/tidb-source-code-reading-19/)
- [TiDB 源码阅读系列文章（二十）Table Partition](https://pingcap.com/blog-cn/tidb-source-code-reading-20/)
- [TiDB 源码阅读系列文章（二十一）基于规则的优化 II](https://pingcap.com/blog-cn/tidb-source-code-reading-21/)
- [TiDB 源码阅读系列文章（二十二）Hash Aggregation](https://pingcap.com/blog-cn/tidb-source-code-reading-22/)
- [TiDB 源码阅读系列文章（二十三）Prepare/Execute 请求处理](https://pingcap.com/blog-cn/tidb-source-code-reading-23/)
- [TiDB 源码阅读系列文章（二十四）TiDB Binlog 源码解析](https://pingcap.com/blog-cn/tidb-source-code-reading-24/)

### TiKV 源码阅读

- [TiKV 源码解析系列文章（一）序](https://pingcap.com/blog-cn/tikv-source-code-reading-1/)
- [TiKV 源码解析系列文章（二）raft-rs proposal 示例情景分析](https://pingcap.com/blog-cn/tikv-source-code-reading-2/)
- [TiKV 源码解析系列文章（三）Prometheus（上）](https://pingcap.com/blog-cn/tikv-source-code-reading-3/)
- [TiKV 源码解析系列文章（四）Prometheus（下）](https://pingcap.com/blog-cn/tikv-source-code-reading-4/)
- [TiKV 源码解析系列文章（五）fail-rs 介绍](https://pingcap.com/blog-cn/tikv-source-code-reading-5/)
- [TiKV 源码解析系列文章（六）raft-rs 日志复制过程分析](https://pingcap.com/blog-cn/tikv-source-code-reading-6/)
- [TiKV 源码解析系列文章（七）gRPC Server 的初始化和启动流程](https://pingcap.com/blog-cn/tikv-source-code-reading-7/)
- [TiKV 源码解析系列文章（八）grpc-rs 的封装与实现](https://pingcap.com/blog-cn/tikv-source-code-reading-8/)
- [TiKV 源码解析系列文章（九）Service 层处理流程解析](https://pingcap.com/blog-cn/tikv-source-code-reading-9/)
- [TiKV 源码解析系列文章（十）Snapshot 的发送和接收](https://pingcap.com/blog-cn/tikv-source-code-reading-10/)
- [TiKV 源码解析系列文章（十一）Storage - 事务控制层](https://pingcap.com/blog-cn/tikv-source-code-reading-11/)
- [TiKV 源码解析系列文章（十二）分布式事务](https://pingcap.com/blog-cn/tikv-source-code-reading-12/)
- [TiKV 源码解析系列文章（十三）MVCC 数据读取](https://pingcap.com/blog-cn/tikv-source-code-reading-13/)
- [TiKV 源码解析系列文章（十四）Coprocessor 概览](https://pingcap.com/blog-cn/tikv-source-code-reading-14/)
- [TiKV 源码解析系列文章（十五）表达式计算框架](https://pingcap.com/blog-cn/tikv-source-code-reading-15/)
- [TiKV 源码解析系列文章（十六）TiKV Coprocessor Executor 源码解析](https://pingcap.com/blog-cn/tikv-source-code-reading-16/)
- [TiKV 源码解析系列文章（十七）raftstore 概览](https://pingcap.com/blog-cn/tikv-source-code-reading-17/)
- [TiKV 源码解析系列文章（十八）Raft Propose 的 Commit 和 Apply 情景分析](https://pingcap.com/blog-cn/tikv-source-code-reading-18/)
- [TiKV 源码解析系列文章（二十）Region Split 源码解析](https://pingcap.com/blog-cn/tikv-source-code-reading-20/)
- [TiKV 源码解析系列文章（十九）read index 和 local read 情景分析](https://pingcap.com/blog-cn/tikv-source-code-reading-19/)

### DM 源码阅读

- [DM 源码阅读系列文章（一）序](https://pingcap.com/blog-cn/dm-source-code-reading-1/)
- [DM 源码阅读系列文章（二）整体架构介绍](https://pingcap.com/blog-cn/dm-source-code-reading-2/)
- [DM 源码阅读系列文章（三）数据同步处理单元介绍](https://pingcap.com/blog-cn/dm-source-code-reading-3/)
- [DM 源码阅读系列文章（四）dump/load 全量同步的实现](https://pingcap.com/blog-cn/dm-source-code-reading-4/)
- [DM 源码阅读系列文章（五）Binlog replication 实现](https://pingcap.com/blog-cn/dm-source-code-reading-5/)
- [DM 源码阅读系列文章（六）relay log 的实现](https://pingcap.com/blog-cn/dm-source-code-reading-6/)
- [DM 源码阅读系列文章（七）定制化数据同步功能的实现](https://pingcap.com/blog-cn/dm-source-code-reading-7/)
- [DM 源码阅读系列文章（八）Online Schema Change 迁移支持](https://pingcap.com/blog-cn/dm-source-code-reading-8/)
- [DM 源码阅读系列文章（九）shard DDL 与 checkpoint 机制的实现](https://pingcap.com/blog-cn/dm-source-code-reading-9/)
- [DM 源码阅读系列文章（十）测试框架的实现](https://pingcap.com/blog-cn/dm-source-code-reading-10/)
