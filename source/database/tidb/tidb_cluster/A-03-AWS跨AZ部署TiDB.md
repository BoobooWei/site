---
title:  AWS 跨 AZ 部署 TiDB Cluster
---

> AWS 上的单一区域 (region)、跨三个可用区（Availability Zones，以下简称 AZ）部署 TiDB 的解决方案

# 参考文档

- [ AWS 跨 AZ 部署 TiDB](https://book.tidb.io/session4/chapter4/cross-az-in-aws.html)

# 集群部署

## TiUP Cluster 部署规划

### 操作系统版本

- CentOS 7.8

### 生产环境服务器数量和配置参数

| 组件       | CPU        | 内存     | 硬盘类型 | 磁盘大小  | 网络                        | 机器数量 |
| ---------- | ---------- | -------- | -------- | --------- | --------------------------- | -------- |
| PD         | 4 Core  | 8 GB  | SSD      | 300 GB | 1 块万兆网卡             | 3        |
| TiDB       | 16 Core | 32 GB | SSD  | 300 GB | 1 块万兆网卡             | 2        |
| TiKV       | 16 Core | 32 GB | SSD      | 2 TB   | 1 块万兆网卡             | 3        |
| Monitoring| 4 Core  | 8 GB | SSD  | 300 GB | 1 块千兆网卡或者万兆网卡 | 1        |

#### PD 配置

- 机器数量 = 3 台 
- 磁盘建议采用 SSD，容量建议 >= 300 GB
- 部署五个 PD 实例：分布在三个 AZ 上 (2:2:1)
- 设置 max-replicas 为 5

*`max-replicas=3` 相比，该设置不会在 AZ 故障期间提高可用性，但会减少由于 EC2 实例的偶然故障而导致集群服务中断的可能性。*

#### TiDB 配置

- 机器数量 = 2 台，若每台机器资源较丰富，则建议部署多个 tidb-server
- 磁盘建议 SAS/SSD，建议容量 >= 300 GB
- 部署 2 个 TiDB 实例：分布在两个 AZ 上 (1:1:0)


#### TiKV 配置

- 机器数量 = 3 台
- 部署必须配置 label，配置后系统会将根据 label 将数据分布在不同的机房、机架、机器防止有单个机房或者机架或者机器宕机时，系统才具备容灾能力
- 磁盘建议采用 SSD，其中建议 PCI-E SSD 容量 <= 2 TB, 普通 SSD 容量 <= 1.5 TB
- 部署 10 个 TiKV 实例：分布在三个 AZ 上`1a:1b:1c=2:4;4；max-replicas=5`

##### 位置标签 (location labels)

> TiKV 设置适当的位置标签。否则，你的集群将无法承受预期的故障。 

设置标签的基本原则是：

* 每台主机都有唯一的标签
* 每个机架（如果有）都有唯一的标签
* 放置每个副本的每个逻辑组 (logical group) 都有唯一的标签
* 每个 AZ 都有唯一的标签

以实际情况为例（10 个 TiKV 实例分布在三个 AZ 上），则恰当的标签为：

```bash
PD：location-labels = ["az","zone","host"]
TiKV：
az	    zone	host
az=1a	zone=1	host=1a_1
az=1a	zone=1	host=1a_2
az=1b	zone=2	host=1b_1
az=1b	zone=2	host=1b_2
az=1b	zone=3	host=1b_3
az=1b	zone=3	host=1b_4
az=1c	zone=4	host=1c_1
az=1c	zone=4	host=1c_2
az=1c	zone=5	host=1c_3
az=1c	zone=5	host=1c_4
```

需要注意的是 `zone` 是一个逻辑的标签，PD 在调度时首先会尝试将 Region 的 Peer 放置在不同的 AZ，此时无法满足(3 个 AZ ,5 个副本)，
下一步保证放置在不同的 az.zone 中( 此时 5 个 zone，5 个副本，满足要求 )。
这样确保了在正常情况下，使用 az.zone 将每个副本（即 Region peer）放置在每个逻辑组（即 zone）中。 
如果可用 az.zone 的数量少于 5，比如 zone 或 AZ 级别的故障出现时，将使用 az.zone.host 标签来均匀调度五个 Region peers，
以保持最大可用性。

##### 置放群组 (placement groups)

如果你想减少相关的硬件故障，可以考虑使用 [Placement Groups](https://docs.aws.amazon.com/zh_cn/AWSEC2/latest/UserGuide/placement-groups.html)。在不同的场景下， 
Partition Placement Groups 和 Spread Placement Groups 各有用处。记住要额外配置位置标签（例如机架的标签）以充分利用 placement groups。


### 生产环境网络要求

TiDB 正常运行需要网络环境提供如下的网络端口配置，管理员可根据实际 TiDB 组件部署的需要进行调整：

| 组件              | 默认端口 | 说明                                                         |
| ----------------- | -------- | ------------------------------------------------------------ |
| TiDB              | 4000     | 应用及 DBA 工具访问通信端口                                  |
| TiDB              | 10080    | TiDB 状态信息上报通信端口                                    |
| TiKV              | 20160    | TiKV 通信端口                                                |
| PD                | 2379     | 提供 TiDB 和 PD 通信端口                                     |
| PD                | 2380     | PD 集群节点间通信端口                                        |
| Pump              | 8250     | Pump 通信端口                                                |
| Drainer           | 8249     | Drainer 通信端口                                             |
| Prometheus        | 9090     | Prometheus 服务通信端口                                      |
| Pushgateway       | 9091     | TiDB，TiKV，PD 监控聚合和上报端口                            |
| Node_exporter     | 9100     | TiDB 集群每个节点的系统信息上报通信端口                      |
| Blackbox_exporter | 9115     | Blackbox_exporter 通信端口，用于 TiDB 集群端口监控           |
| Grafana           | 3000     | Web 监控服务对外服务和客户端(浏览器)访问端口                 |
| Grafana           | 8686     | grafana_collector 通信端口，用于将 Dashboard 导出为 PDF 格式 |
| Kafka_exporter    | 9308     | Kafka_exporter 通信端口，用于监控 binlog kafka 集群          |

### topology 配置

```yaml

```

## TiUP Cluster 部署实践

- 选择可用区：加拿大 (中部)ca-central-1
- 启动实例：4核/8G * 4（c5.xlarge 0.186 USD/hour） ； 16核/32G * 5 (c5.4xlarge $ 0.744 USD/hour)
