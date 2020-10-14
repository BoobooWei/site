---
title: RDS 配置指导
---

## 序

本文为：【大类】云产品配置指导-【小类】数据库配置指导。 

## 子类概述

（该子类的整体描述，解决什么问题，通过本节可以获得什么）

简介

阿里云数据库产品种类繁多，在具体配置使用的时候，会遇到各种各样的问题，本文通过对具体数据库的配置概述，让您快速掌握配置中遇到的问题。

## MySQL数据库配置指导

### MySQL数据库实例开通

云数据库RDS有很多不同规格，不同数据库类型的配置，如果在选型和开通RDS实例上的问题，可以参考[数据库选型配置指导](https://confluence.jiagouyun.com/pages/viewpage.action?pageId=54894468)

下面根据不同的应用场景指导创建RDS实例，创建的基本步骤可以参考[创建实例](https://help.aliyun.com/document_detail/26117.html?spm=a2c4g.11186623.6.575.7a6ce5b9kbbM4v)

#### 创建隔离环境下的RDS实例

网络类型是ECS、RDS等产品功能上的区分，与运营商公网接入网络质量无关，任何网络类型的运营商接入均为BGP线路，请您放心使用，并根据自己需要进行选择。

- 经典网络：IP地址由阿里云统一分配，配置简便，使用方便，适合对操作易用性要求比较高、需要快速使用RDS、ECS的用户。
- 专有网络：逻辑隔离的私有网络，用户可以自定义网络拓扑和IP地址，支持通过专线连接。适合对网络管理熟悉了解的用户。

创建界面中“网络类型”选择专有网络。

#### 创建单机房双节点容灾RDS实例

单可用区：有效控制云产品间的网络延迟

- 可用区是指在同一地域下（如杭州地域），电力、网络隔离的物理区域，可用区之间内网互通，可用区内网络延时更小，不同可用区之间故障离。
- RDS单可用区是指RDS实例的主备节点处于相同的可用区。如果ECS和RDS署在相同的可用区， ECS和RDS间的网络延迟更小。

创建界面中“地域”选择单个可用区，例如：“华东2 可用区B”

#### 创建跨可用区双节点容灾（同城容灾）RDS实例

多可用区：轻松实现同城容灾

- 多可用区是指RDS实例的主备节点位于不同的可用区，当主节点所在可用区出现故障（如机房断电等），RDS进行主备切换后，会切换到备节点所在的可用区继续提供服务。多可用区的RDS轻松实现了同城容灾。

- 当RDS发生主备切换后，由于可用区的切换会造成ECS与RDS网络延迟突然加大，对业务的影响需要先行评估。

创建界面中“地域”选择多可用区，例如：“多可用区（可用区B+可用区C)”

####创建异地容灾RDS实例

配置了[跨地域灾备实例](https://help.aliyun.com/document_detail/26137.html?spm=5176.2020520104.200.7.2b4b1450C8495z)后，当实例A’所在地域发生短期不可恢复的重大故障时，用户在另外一个地域的实例B’随时可以进行容灾切换。切换完成后，用户通过修改应用程序中的数据库连接配置，可以将应用请求转到实例B’上，进而获得高于地域极限的数据库可用性。

> 说明：容灾切换前用户需要先停止实例A’到实例B ’的数据复制，以免造成数据错乱。

单机房双节点容灾RDS实例基本可以满足大多数客户的需求，如果是金融类客户建议选择跨可用区双节点容灾（同城容灾）RDS实例。

### MySQL数据库配置使用说明

#### 通过RDS控制台管理数据库

RDS控制台使用帮助参考[ RDS控制台](https://help.aliyun.com/knowledge_detail/41873.html?spm=5176.11065259.1996646101.searchclickresult.42714291PtoCv4)

快速入门请参考 [快速入门MySQL版](https://help.aliyun.com/document_detail/26124.html?spm=a2c4g.11186623.6.574.7330e5b9rrUmaN)

#### 开通RDS实例后延迟显示

在控制台上开通RDS实例后，会有分钟级别的延时，直到控制台显示出创建的MySQL实例列表，可以进入[RDS控制台](https://help.aliyun.com/knowledge_detail/41873.html?spm=5176.11065259.1996646101.searchclickresult.42714291PtoCv4)开始使用数据库。 

#### 获取数据连接地址

**设置白名单**

非生产环境主要是开发人员使用或测试环境，白名单可适当放宽，例如设置为%或者0.0.0.0/0，可允许任何IP地址访问该RDS实例。具体步骤参考[设置白名单](https://help.aliyun.com/document_detail/43185.html?spm=a2c4g.11186623.6.577.776f13ef8OgLzp)

生产环境必须保证数据库的安全性，白名单应该只开放业务访问的ip或者网段，并定期检查，不应该出现%或者0.0.0.0/0。具体步骤参考[设置白名单](https://help.aliyun.com/document_detail/43185.html?spm=a2c4g.11186623.6.577.776f13ef8OgLzp)

**申请连接地址**

RDS支持两种地址：内网地址和外网地址，具体区别参考[RDS内网和外网地址区别](https://help.aliyun.com/document_detail/26128.html?spm=a2c4g.11186623.6.578.681f13efBi0dyF)。

白名单设置好后，才能够申请连接地址。

- 非生产环境可同时申请内网地址和外网地址，方便开发人员从外网访问数据库。具体步骤参考[申请连接地址](https://help.aliyun.com/document_detail/26128.html?spm=a2c4g.11186623.6.578.640c2fe3BWbzur)
- 生产环境应只选择内网地址。如果有临时对外网的访问需求，可以在申请外网后进行释放。具体步骤参考[申请连接地址](https://help.aliyun.com/document_detail/26128.html?spm=a2c4g.11186623.6.578.640c2fe3BWbzur)

#### 创建账号和数据库

**创建账号**

RDS for MySQL实例支持两种数据库账号：高权限账号和普通账号，具体区别参考[账号区别](https://help.aliyun.com/document_detail/87038.html?spm=a2c4g.11186623.6.579.35cc44e88EJcaL)。

- 为应用程序创建普通账号，具体步骤参考[创建账号](https://help.aliyun.com/document_detail/87038.html?spm=a2c4g.11186623.6.579.35cc44e88EJcaL)
- 为数据库管理员创建高权限账号，具体步骤参考[创建账号](https://help.aliyun.com/document_detail/87038.html?spm=a2c4g.11186623.6.579.35cc44e88EJcaL)
- 数据库账号管理可用于创建新的数据库账号，或者修改已有数据库账号的信息。每个实例最多可创建500个数据库账号。

**创建数据库**

普通账号无法直接使用命令进行数据库创建，可以通过RDS管理控制台[创建数据库](https://help.aliyun.com/document_detail/87038.html?spm=a2c4g.11186623.6.579.35cc44e88EJcaL)。

高权限账号可以通过命令进行数据库创建。

- 每个实例最多可以创建500个数据库
- 创建数据库时可选择授权账号，权限有读写、只读、仅DDL或只DML。
- 只支持InnoDB和TokuDB两种引擎，详情请参见文档[为什么RDS for MySQL不支持MyISAM引擎](https://help.aliyun.com/document_detail/52558.html)
- 出于性能和安全性考虑，建议尽量采用InnoDB存储引擎，如需修改为TokuDB参见[设置参数](https://help.aliyun.com/document_detail/26179.html#concept_lfl_xmn_wdb)。

#### 访问数据库

- [使用DMS连接实例](https://help.aliyun.com/document_detail/26138.html?spm=a2c4g.11186623.6.580.652613efyuzVFJ#h2-url-1)
- [使用客户端连接实例](https://help.aliyun.com/document_detail/26138.html?spm=a2c4g.11186623.6.580.652613efyuzVFJ#h2-url-2)
- [连接失败的解决办法](https://help.aliyun.com/document_detail/26138.html?spm=a2c4g.11186623.6.580.652613efyuzVFJ#h2-url-3)

#### 参数配置

大部分实例参数可以使用RDS管理控制台或API进行修改，同时出于安全和稳定性考虑，部分参数不支持修改，具体请参见[设置参数](https://help.aliyun.com/document_detail/26179.html#concept_lfl_xmn_wdb)和[RDS控制台中可以设定的参数](https://confluence.jiagouyun.com/pages/viewpage.action?pageId=54896987)。

[RDS MySQL参数调优最佳实践](https://yq.aliyun.com/articles/51083?spm=5176.10695662.1996646101.searchclickresult.38327d6eK2AX34)

#### 监控部署

设置监控频率和设置报警规则，详细请参见[监控告警](https://help.aliyun.com/document_detail/26200.html?spm=a2c4g.11186623.3.3.6ef42306IFQVWj) 

### MySQL备份与恢复

#### 冷备策略

针对非生产环境，不仅仅对数据的全量备份有需求，还会有对表结构备份的需求，建议同时使用RDS备份工具和逻辑备份脚本：

- 通过阿里云控制台进行物理全备和逻辑全备，物理备份的频率建议一周一次，并放在业务低峰期执行，具体步骤参考阿[备份 RDS 数据](https://help.aliyun.com/document_detail/26206.html?spm=a2c4g.11186623.6.730.761642b5L8oa3t)
- 通过部署Bash脚本实现单库单表的逻辑备份，具体代码参考[MySQL逻辑备份脚本](https://confluence.jiagouyun.com/pages/viewpage.action?pageId=43745533)

针对生产环境，有如下建议：

- 通过阿里云控制台进行物理全备和逻辑全备，物理备份的频率建议一周7次，并放在业务低峰期执行，具体步骤参考[备份 RDS 数据](https://help.aliyun.com/document_detail/26206.html?spm=a2c4g.11186623.6.730.761642b5L8oa3t)
- 通过阿里云控制台进行二进制日志备份，建议本地保留时长设置24小时以上
- 通过部署Bash脚本实现单库单表的逻辑备份，具体代码参考[MySQL逻辑备份脚本](https://confluence.jiagouyun.com/pages/viewpage.action?pageId=43745533)

#### 热备策略

通过创建单机房双节点、跨可用区双节点、[异地容灾实例](https://help.aliyun.com/document_detail/26137.html?spm=a2c4g.11186623.6.585.3591dba0K5Yvzn)均可做到热备，数据可靠性更高。

对于数据可靠性有强需求的业务场景或是有监管需求的金融业务场景，RDS 提供异地灾备实例，帮助用户提升数据可靠性。

####数据恢复

您可以通过以下方式恢复RDS for MySQL的数据：

- 方式一：数据恢复到全备份时间点，直接恢复到原实例。具体请参见[直接恢复到原实例](https://help.aliyun.com/document_detail/53569.html)。
- 方式二：数据恢复到指定时间点，先恢复到一个新实例，经过验证后，再将数据迁移到原实例。具体请参见[克隆实例](https://help.aliyun.com/document_detail/44088.html?spm=a2c4g.11186623.6.737.3d321072fZbTPL)。
- 方式三：数据恢复到人为误操作时间点，具体操作参见[人为误操作工作手册](https://confluence.jiagouyun.com/pages/viewpage.action?pageId=54887715)
- 方法四：[恢复云数据库MySQL的备份文件到自建数据库](https://help.aliyun.com/knowledge_detail/41817.html?spm=a2c4g.11186631.2.2.3b767786Epqc5C)
- 方法五：[RDS逻辑备份恢复问题汇总](https://confluence.jiagouyun.com/pages/viewpage.action?pageId=47124987)

### OLTP和OLAP混合应用场景

企业级应用的两个业务场景：**在线交易**和**数据分析**，是OLTP和OLAP的典型应用。在线交易对数据库的ACID特性有严格的要求，更关注数据库在低延迟、高并发方面的能力，数据分析对并发和延迟要求不高，反而更关注数据库的算法支持、容量、计算处理能力。在企业级应用的不同成长阶段，为这两类业务选择的技术有很大差别。 

#### 小型应用阶段选型_RDS For MySQL单实例

为了节省成本，可以选择将这两类业务放在同一个**RDS For MySQL****单实例**中运行，在数据规模小（单表量在100万行左右）时，可以运转很好。 

#### 中型应用阶段选型_RDS For MySQL读写分离

在数据规模上来时（单表量在1000万行左右），在线交易和数据分析共用一个MySQL数据库单实例会面临资源争抢的问题：

- 由于“少数几个请求”的“批量查询”的“低效”访问，导致数据库的CPU偶尔瞬时100%，影响前台正常用户的访问
- 分析业务会消耗数据库大量的CPU和IO资源，影响到交易业务的延迟，最终使得每个业务都得不到很好服务。

建议选择RDS For MySQL读写分离，实现读写分离和分时复用，一个主库用于交易，多个读库用于分析，且在线业务和离线业务分时复用；

可以创建一个或多个只读实例，利用只读实例满足大量的数据库读取需求，增加应用的吞吐量。读写分离模块将自动对主实例和只读实例进行健康检查，当发现某个实例出现宕机或者延迟超过阈值时，将不再分配读请求给该实例，读写请求在剩余的健康实例间进行分配。以此确保单个只读实例发生故障时，不会影响应用的正常访问。当实例被修复后，RDS会自动将该实例纳回请求分配体系内。

- [只读实例的创建与管理](https://help.aliyun.com/document_detail/26136.html?spm=5176.doc51070.2.1.d3x8y5)
- [读写分离的开通与设置](https://help.aliyun.com/document_detail/51070.html?spm=5176.doc51070.6.661.ObUl9g)
- [读写分离性能压测](https://help.aliyun.com/document_detail/57200.html?spm=5176.doc51070.6.665.57D43J)

####大型应用阶段选型_云数据库POLARDB

巨大的用户数持续刺激业务增长，多维连接（线上、线下、行业+）带来更多的数据，数据规模进一步上升（单表量在3000万以上），传统数据库的处理能力难以应对庞大的数据量，RDS For MySQL单一的主库已经不能满足交易需求，读库也跑不动越来越复杂的分析SQL。此时，建议选择[云数据库POLARDB](https://help.aliyun.com/product/58609.html?spm=5176.155538.806580.docs.4e8e7e0donyyEm)，采用读写分离架构，支持多路应用服务器并发访问，提供企业级的高性能、高可用能力，共享存储支持最大支持100 TB数据。

- [创建数据库集群](https://help.aliyun.com/document_detail/58769.html)
- [设置集群白名单](https://help.aliyun.com/document_detail/68506.html)
- [创建数据库账号](https://help.aliyun.com/document_detail/68508.html)
- [连接集群或实例](https://help.aliyun.com/document_detail/68510.html)
- [POLARDB公测版迁移至商用版](https://help.aliyun.com/document_detail/72472.html)
- [常见问题列表](https://help.aliyun.com/document_detail/73995.html?spm=a2c4g.11186623.6.591.484763ccPPO4uc)

PolarDB添加只读实例的速度比RDS速度快很多，因为PolarDB是计算和存储分离的，添加只读节点只是新开一个计算资源，无需复制数据；而RDS的只读实例是将数据进行了迁移同步。

PolarDB目前的缺点：

- 单可用区高可用（在单机房中计算节点冗余，且数据为3副本）
- 控制台备份方式目前仅支持自动快照备份
- 可用区目前已开放上海、杭州、北京

PolarDB为阿里云自主研发的全面兼容MySQL的数据库，当前产品还在完善中，且阿里对PolarDB的定位是将来会取代RDS系列。

## Redis数据库配置指导

### Redis数据库实例开通

云数据库RDS有很多不同规格，不同数据库类型的配置，如果在选型和开通云Redis实例上的问题，可以参考[数据库选型配置指导](https://confluence.jiagouyun.com/pages/viewpage.action?pageId=54894468)。

下面根据不同的应用场景指导创建RDS实例，创建的基本步骤可以参考[创建实例](https://help.aliyun.com/document_detail/26351.html?spm=a2c4g.11186623.6.569.511bbeb0CxqW2L)

#### 创建隔离环境下的Redis实例

网络类型是ECS、RDS等产品功能上的区分，与运营商公网接入网络质量无关，任何网络类型的运营商接入均为BGP线路，请您放心使用，并根据自己需要进行选择。

- 经典网络：IP地址由阿里云统一分配，配置简便，使用方便，适合对操作易用性要求比较高、需要快速使用RDS、ECS的用户。
- 专有网络：逻辑隔离的私有网络，用户可以自定义网络拓扑和IP地址，支持通过专线连接。适合对网络管理熟悉了解的用户。

创建界面中“网络类型”选择专有网络。

#### 创建单机房Redis实例

单可用区：有效控制云产品间的网络延迟

- 可用区是指在同一地域下（如杭州地域），电力、网络隔离的物理区域，可用区之间内网互通，可用区内网络延时更小，不同可用区之间故障离。
- RDS单可用区是指RDS实例的主备节点处于相同的可用区。如果ECS和RDS署在相同的可用区， ECS和RDS间的网络延迟更小。

创建界面中：“地域”选择单个可用区，例如：“华东2 可用区B”

####创建跨可用区容灾（同城容灾）Redis实例

多可用区：轻松实现同城容灾

- 多可用区是指RDS实例的主备节点位于不同的可用区，当主节点所在可用区出现故障（如机房断电等），RDS进行主备切换后，会切换到备节点所在的可用区继续提供服务。多可用区的RDS轻松实现了同城容灾。

- 当RDS发生主备切换后，由于可用区的切换会造成ECS与RDS网络延迟突然加大，对业务的影响需要先行评估。

创建界面中“地域”选择多可用区，例如：“多可用区（可用区B+可用区C)”

**单机房双节点容灾RDS实例基本可以满足大多数客户的需求，如果是金融类客户建议选择跨可用区双节点容灾（同城容灾）RDS实例。**

云Redis创建过程中，可以设定密码，也可在实例创建后设定密码。

### Redis数据库配置使用说明

#### 通过Redis控制台管理数据库

Redis控制台使用帮助参考 [云数据库Redis版管理控制台](https://help.aliyun.com/document_detail/54467.html?spm=a2c4g.11174283.3.2.2ba0dce0QHH1OD)

快速入门请参考 [快速入门云Redis](https://help.aliyun.com/document_detail/54592.html?spm=a2c4g.11186623.6.565.452215491Pdir3)

#### 开通RDS实例后延迟显示

在控制台上开通RDS实例后，会有分钟级别的延时，直到控制台显示出创建的MySQL实例列表，可以进入[R](https://help.aliyun.com/knowledge_detail/41873.html?spm=5176.11065259.1996646101.searchclickresult.42714291PtoCv4)[edis控制台](https://kvstorenext.console.aliyun.com/) 开始使用数据库。 

#### 获取数据连接地址

**设置白名单**

非生产环境主要是开发人员使用或测试环境，白名单可适当放宽，例如设置为%或者0.0.0.0/0，可允许任何IP地址访问该RDS实例。具体步骤参考[设置白名单](https://help.aliyun.com/document_detail/56464.html?spm=a2c4g.11186623.6.579.5f3f786fyv2er4)

生产环境必须保证数据库的安全性，白名单应该只开放业务访问的ip或者网段，并定期检查，不应该出现%或者0.0.0.0/0。具体步骤参考[设置白名单](https://help.aliyun.com/document_detail/56464.html?spm=a2c4g.11186623.6.579.5f3f786fyv2er4)

**连接地址**

云数据库 Redis 版仅支持阿里云内网访问，不支持外网访问，即只有在阿里云 ECS 上的应用才能与云数据库 Redis 版建立连接并进行数据操作。

如需通过公网访问，请参考[公网连接](https://help.aliyun.com/document_detail/43850.html)。

#### 账号管理

如果您忘记密码、需要修改旧密码，或者在创建实例时没有设置密码，您可以重新设置实例的密码。具体参见[修改密码](https://help.aliyun.com/document_detail/43874.html?spm=a2c4g.11186623.6.578.560c1ce1hz6661) 

#### 访问数据库

- [从Redis管理控制台登录](https://help.aliyun.com/document_detail/43847.html?spm=a2c4g.11186623.6.571.2b7c244bxChzAC#h2-url-1)
- [从DMS管理控制台登录](https://help.aliyun.com/document_detail/43847.html?spm=a2c4g.11186623.6.571.2b7c244bxChzAC#h2-url-2)
- [Redis-cli 连接](https://help.aliyun.com/document_detail/43849.html?spm=a2c4g.11186623.6.573.717f3846cOkKFb)
- [公网连接](https://help.aliyun.com/document_detail/43850.html?spm=a2c4g.11186623.6.574.3e5a585a2nGOJF)

#### 参数配置

云数据库 Redis 版允许用户自定义部分实例参数。具体参见[参数设置](https://help.aliyun.com/document_detail/43885.html?spm=a2c4g.11186623.6.595.68de786fid9imp)，您可以了解相关参数说明及其设置方法。 

#### 监控部署

Redis 实例提供实例监控功能，当检测到实例异常时，还能够发送短信通知用户。

设置报警规则，详细请参见[报警设置](https://help.aliyun.com/document_detail/43884.html?spm=a2c4g.11186623.6.600.7e824d00Au164B)

####性能监控

Redis 提供十种监控组，您可以在 Redis 控制台上根据业务需要自定义监控项目，也可以通过 DMS for Redis 开启对 Redis 实例的实时监控。具体监控指标解读参见[性能监控](https://help.aliyun.com/document_detail/43887.html?spm=a2c4g.11186623.6.601.7f91634dng6due) 

###Redis备份与恢复

#### 冷备策略

针对缓存数据，无需开启备份，针对非缓存数据需要落盘的情况，有如下建议：

- 通过阿里云控制台进行物理全备，物理备份的频率建议一周7次，并放在业务低峰期执行，具体步骤参考[Redis自动备份（设置备份策略）](https://help.aliyun.com/document_detail/43886.html?spm=a2c4g.11186623.6.598.3c324d00nunDjL)
- 在控制台上随机发起一次手工备份，具体步骤参见[手动备份（立即备份）](https://help.aliyun.com/document_detail/43886.html?spm=a2c4g.11186623.6.598.3c324d00nunDjL)
- 自动将自动备份或者手动备份文件保存至 OSS 上，具体步骤参见[备份存档](https://help.aliyun.com/document_detail/43886.html?spm=a2c4g.11186623.6.598.3c324d00nunDjL)

#### 热备策略

通过创建单机房双副本、跨可用区双副本、集群版均可做到双机热备，数据可靠性更高。 

####数据恢复

您可以通过以下方式恢复云Redis的数据：

- 方式一：Redis控制台中根据已有的备份数据进行恢复，具体参见[数据恢复](https://help.aliyun.com/document_detail/43886.html?spm=a2c4g.11186623.6.598.3c324d00nunDjL)
- 方式二：Redis控制台中根据已有的备份数据进行克隆实例，具体参见[克隆实例](https://help.aliyun.com/document_detail/43886.html?spm=a2c4g.11186623.6.598.3c324d00nunDjL)
- 方式三：通过阿里云OpenAPI下载备份文件，然后使用redis-port为目的数据库进行数据恢复。具体参见[使用redis-port恢复数据](https://help.aliyun.com/document_detail/90931.html?spm=a2c4g.11186623.6.599.44be1f54KsYUdp)

## SQL Server数据库配置指导

### SQL Server数据库实例开通

云数据库RDS有很多不同规格，不同数据库类型的配置，如果在选型和开通RDS实例上的问题，可以参考[数据库选型配置指导](https://confluence.jiagouyun.com/pages/viewpage.action?pageId=54894468)

下面根据不同的应用场景指导创建RDS实例，创建的基本步骤可以参考[创建实例](https://help.aliyun.com/document_detail/26117.html?spm=a2c4g.11186623.6.575.7a6ce5b9kbbM4v)

#### 创建隔离环境下的RDS实例

网络类型是ECS、RDS等产品功能上的区分，与运营商公网接入网络质量无关，任何网络类型的运营商接入均为BGP线路，请您放心使用，并根据自己需要进行选择。

- 经典网络：IP地址由阿里云统一分配，配置简便，使用方便，适合对操作易用性要求比较高、需要快速使用RDS、ECS的用户。
- 专有网络：逻辑隔离的私有网络，用户可以自定义网络拓扑和IP地址，支持通过专线连接。适合对网络管理熟悉了解的用户。

创建界面中“网络类型”选择专有网络。

#### 创建单机房双节点容灾RDS实例

单可用区：有效控制云产品间的网络延迟

- 可用区是指在同一地域下（如杭州地域），电力、网络隔离的物理区域，可用区之间内网互通，可用区内网络延时更小，不同可用区之间故障离。
- RDS单可用区是指RDS实例的主备节点处于相同的可用区。如果ECS和RDS署在相同的可用区， ECS和RDS间的网络延迟更小。

创建界面中“地域”选择单个可用区，例如：“华东2 可用区B”

####创建跨可用区双节点容灾（同城容灾）RDS实例

多可用区：轻松实现同城容灾

- 多可用区是指RDS实例的主备节点位于不同的可用区，当主节点所在可用区出现故障（如机房断电等），RDS进行主备切换后，会切换到备节点所在的可用区继续提供服务。多可用区的RDS轻松实现了同城容灾。

- 当RDS发生主备切换后，由于可用区的切换会造成ECS与RDS网络延迟突然加大，对业务的影响需要先行评估。

创建界面中“地域”选择多可用区，例如：“多可用区（可用区B+可用区C)”

目前SQL Server不支持创建异地容灾RDS实例。

单机房双节点容灾RDS实例基本可以满足大多数客户的需求，如果是金融类客户建议选择跨可用区双节点容灾（同城容灾）RDS实例。

### SQL Server数据库配置使用说明

SQL Server类型的RDS实例均自带微软SQL Server的license，不支持使用自己的license。 

#### 通过RDS控制台管理数据库

RDS控制台使用帮助参考[ RDS控制台](https://help.aliyun.com/knowledge_detail/41873.html?spm=5176.11065259.1996646101.searchclickresult.42714291PtoCv4)

快速入门请参考[快速入门SQL Server版](https://help.aliyun.com/document_detail/26141.html?spm=a2c4g.11174283.6.587.8ad14c226Go7Cj)

#### 开通RDS实例后延迟显示

在控制台上开通RDS实例后，会有分钟级别的延时，直到控制台显示出创建的MySQL实例列表，可以进入[RDS控制台](https://help.aliyun.com/knowledge_detail/41873.html?spm=5176.11065259.1996646101.searchclickresult.42714291PtoCv4)开始使用数据库。 

#### 获取数据连接地址

**设置白名单**

非生产环境主要是开发人员使用或测试环境，白名单可适当放宽，例如设置为%或者0.0.0.0/0，可允许任何IP地址访问该RDS实例。具体步骤参考[设置白名单](https://help.aliyun.com/document_detail/43185.html?spm=a2c4g.11186623.6.577.776f13ef8OgLzp)

生产环境必须保证数据库的安全性，白名单应该只开放业务访问的ip或者网段，并定期检查，不应该出现%或者0.0.0.0/0。具体步骤参考[设置白名单](https://help.aliyun.com/document_detail/43185.html?spm=a2c4g.11186623.6.577.776f13ef8OgLzp)

**申请连接地址**

RDS支持两种地址：内网地址和外网地址，具体区别参考[RDS内网和外网地址区别](https://help.aliyun.com/document_detail/26128.html?spm=a2c4g.11186623.6.578.681f13efBi0dyF)。

白名单设置好后，才能够申请连接地址。

- 非生产环境可同时申请内网地址和外网地址，方便开发人员从外网访问数据库。具体步骤参考[申请连接地址](https://help.aliyun.com/document_detail/26128.html?spm=a2c4g.11186623.6.578.640c2fe3BWbzur)
- 生产环境应只选择内网地址。如果有临时对外网的访问需求，可以在申请外网后进行释放。具体步骤参考[申请连接地址](https://help.aliyun.com/document_detail/26128.html?spm=a2c4g.11186623.6.578.640c2fe3BWbzur)

#### 创建账号和数据库

SQL Server帐号管理中需要特别注意：SQL Server 2008 R2版本和SQL Server 2012和2016版本帐号创建有所不同：

SQL Server 2008 R2，没有高权限帐号：[创建数据库和账号SQL Server 2008 R2版](https://help.aliyun.com/document_detail/26145.html?spm=a2c4g.11186623.2.9.12312af2kQGbDz#concept-n3n-1zz-vdb)

SQL Server 2012和2016版本，分高权限账号和普通账号。

可以通过控制台创建高权限账号和普通账号。其中高权限账号只能通过控制台创建。

高权限账号：仅当第一次为该实例创建账号时，才能选择高权限账号，因为实例的第一个账号必须是高权限账号。一个实例只能有一个高权限账号。高权限账号默认拥有所有数据库的权限，无需手动授权。 普通账号：仅当实例已经创建高权限账号时，才只能选择普通账号。一个实例可以有多个普通账号。您需要手动为普通账号授予数据库权限。

详细创建参考：[创建数据库和账号SQL Server 2012及以上版本](https://help.aliyun.com/document_detail/43164.html?spm=a2c4g.11186623.2.10.1b1220d88zu0FD#concept-is4-wb1-wdb)

#### 访问数据库

连接SQL Server数据库有两种方式，分别是DMS控制台连接和客户端连接。

通过DMS控制台连接方法见通过[DMS登录RDS数据库](https://help.aliyun.com/document_detail/64703.html?spm=a2c4g.11186623.2.13.60c07260zoPKal#concept-cml-x4v-ydb)

使用客户端连接实例，一般采用Microsoft SQL Server Management Studio（SSMS）客户端连接，需要注意的是，通过这种方式连接到SQL Server数据库，连接地址与端口号之间用英文逗号隔开，

如[rm-bptest.sqlserver.rds.aliyuncs.com](http://rm-bptest.sqlserver.rds.aliyuncs.com/),3433

####监控部署

设置监控频率和设置报警规则，详细请参见[监控告警](https://help.aliyun.com/document_detail/26200.html?spm=a2c4g.11186623.3.3.6ef42306IFQVWj) 

###SQL Server备份与恢复

#### 冷备策略

针对非生产环境，不仅仅对数据的全量备份有需求，还会有对表结构备份的需求，建议同时使用RDS备份工具和逻辑备份脚本：

- 通过阿里云控制台进行物理全备和逻辑全备，物理备份的频率建议一周一次，并放在业务低峰期执行，具体步骤参考阿[备份 RDS 数据](https://help.aliyun.com/document_detail/26206.html?spm=a2c4g.11186623.6.730.761642b5L8oa3t)
- 通过部署Bash脚本实现单库单表的逻辑备份，具体代码参考[SQL Server逻辑备份](https://confluence.jiagouyun.com/pages/viewpage.action?pageId=15506333)

针对生产环境，有如下建议：

- 通过阿里云控制台进行物理全备和逻辑全备，物理备份的频率建议一周7次，并放在业务低峰期执行，具体步骤参考[备份 RDS 数据](https://help.aliyun.com/document_detail/26206.html?spm=a2c4g.11186623.6.730.761642b5L8oa3t)
- 通过阿里云控制台进行二进制日志备份，建议本地保留时长设置24小时以上
- 通过部署Bash脚本实现单库单表的逻辑备份，具体代码参考[SQL Server逻辑备份](https://confluence.jiagouyun.com/pages/viewpage.action?pageId=15506333)

#### 热备策略

通过创建单机房双节点、跨可用区双节点均可做到热备，数据可靠性更高。

对于数据可靠性有强需求的业务场景或是有监管需求的金融业务场景，RDS 提供异地灾备实例，帮助用户提升数据可靠性。

#### 数据恢复

您可以通过以下方式恢复RDS for SQL Server的数据：

- 方式一：数据恢复到全备份时间点，直接恢复到原实例。具体请参见[直接恢复到原实例](https://help.aliyun.com/document_detail/53569.html)。
- 方式二：数据恢复到指定时间点，先恢复到一个新实例，经过验证后，再将数据迁移到原实例。具体请参见[克隆实例](https://help.aliyun.com/document_detail/44088.html?spm=a2c4g.11186623.6.737.3d321072fZbTPL)。

需要注意的是：针对线下自建的SQL Server数据库环境，迁移恢复到云RDS for SQL Server，一般建议采用[.bak增量备份上云](https://help.aliyun.com/document_detail/71614.html?spm=a2c4g.11174283.6.762.hCz9kf)的方式进行，通过[DTS方式](https://help.aliyun.com/document_detail/26622.html?spm=a2c4g.11174283.6.589.2a777b02Bn8ZRN)迁移恢复有时候会引起部分数据丢失。



