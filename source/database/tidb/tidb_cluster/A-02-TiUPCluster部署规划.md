---
title: TiUP Cluster 部署规划
---

# 参考文档

* [TiUP cluster 部署生产环境集群](https://book.tidb.io/session2/chapter1/tiup-cluster-deployment.html)

### 规划文档模版

```bash
---
title: xx 环境 TiUP Cluster 部署规划
---

### 操作系统版本要求

- 仅支持 CentOS 7.3 及以上 Linux 操作系统。

### 生产环境服务器数量和配置参数

| 组件       | CPU        | 内存     | 硬盘类型 | 磁盘大小  | 网络                        | 机器数量 |
| ---------- | ---------- | -------- | -------- | --------- | --------------------------- | -------- |
| PD         | >= 4 Core  | >= 8 GB  | SSD      | >= 300 GB | >= 1 块万兆网卡             | 3        |
| TiDB       | >= 16 Core | >= 32 GB | SAS/SSD  | >= 300 GB | >= 1 块万兆网卡             | 2        |
| TiKV       | >= 16 Core | >= 32 GB | SSD      | <= 2 TB   | >= 1 块万兆网卡             | 3        |
| Prometheus | >= 8 Core  | >= 16 GB | SAS/SSD  | >= 300 GB | >= 1 块千兆网卡或者万兆网卡 | 1        |

### 注意事项

- TiDB
  - 机器数量 >= 2 台，若每台机器资源较丰富，则建议部署多个 tidb-server
  - 磁盘建议 SAS/SSD，建议容量 >= 300 GB
- TiKV
  - 机器数量 >= 3 台，若每台机器有多块磁盘，建议部署多个 tikv-server
  - 部署推荐配置 label，配置后系统才会将根据 label 将数据分布在不同的机房、机架、机器防止有单个机房或者机架或者机器宕机时，系统才具备容灾能力
  - 磁盘建议采用 SSD，其中建议 PCI-E SSD 容量 <= 2 TB, 普通 SSD 容量 <= 1.5 TB
- PD
  - 机器数量 >= 3 台
  - 磁盘建议采用 SSD，容量建议 >= 300 GB
- Prometheus
  - 机器数量 1 台，若资源较紧张，可与其他组件混合部署
- Other
  - 若资源较紧张时 TiDB、PD、Prometheus 可混合部署在同一台服务器上
  - 若对性能、可靠性有更高的要求，建议按组件分别部署
  - 生产环境强烈推荐使用更高的配置

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

\```yaml
---

pd_servers:
  - ip: 10.9.1.1
  - ip: 10.9.1.2
  - ip: 10.9.1.3

tidb_servers:
  - ip: 10.9.1.2
  - ip: 10.9.1.3

tikv_servers:
  - ip: 10.9.1.4
    ## The value of label can be customized, for example: 'zone=z1,rack=r1,host=h1' or 'a=a1,b=b1,c=c1', etc
    ## Can only set in tikv_servers
    # label: host=h1
  - ip: 10.9.1.5
  - ip: 10.9.1.6

monitoring_server:
  - ip: 10.9.1.7

grafana_server:
  - ip: 10.9.1.7
\```

label 可以自定义，例如：`'zone=z1,rack=r1,host=h1'` 或 `'a=a1,b=b1,c=c1'`。label 只能在 tikv_servers 上设置。

### 部署方法

TiUP
```


### 思维导图


<!DOCTYPE html>
<!-- saved from url=(0016)http://localhost -->
<html>
  <head>
    <meta charset="utf-8"/>
    <meta content="IE=edge" http-equiv="X-UA-Compatible"/>
    <title>TiDB 学习笔记</title>
    <style>
        body{
            margin: 0;
        }
        #content-info{
            width: auto;
            margin: 0 auto;
            text-align: center;
        }
        #author-info{
            white-space: nowrap;
            text-overflow: ellipsis;
            overflow: hidden;
        }
        #title{
            text-overflow: ellipsis;
            white-space: nowrap;
            overflow: hidden;
            padding-top: 10px;
            margin-bottom: 2px;
            font-size: 34px;
            color: #505050;
        }
        .text{
            white-space:nowrap;
            text-overflow: ellipsis;
            display: inline-block;
            margin-right: 20px;
            margin-bottom: 2px;
            font-size: 20px;
            color: #8c8c8c;
        }
        #navBar{
            width: auto;
            height: auto;
            position: fixed;
            right:0;
            bottom: 0;
            background-color: #f0f3f4;
            overflow-y: auto;
            text-align: center;
        }
        #svg-container{
            width: 100%;
            overflow-x: scroll;
            min-width: 0px;
            margin: 0 10px;
        }
        #nav-thumbs{
            overflow-y: scroll;
            padding: 0 5px;
        }
        .nav-thumb{
            position: relative;
            margin: 10px auto;
        }
        .nav-thumb >p{
            text-align: center;
            font-size: 12px;
            margin: 4px 0 0 0;
        }
        .nav-thumb >div{
            position: relative;
            display: inline-block;
            border: 1px solid #c6cfd5;
        }
        .nav-thumb img{
            display: block;
        }
        #main-content{
            bottom: 0;
            left: 0;
            right: 0;
            background-color: #d0cfd8;
            display: flex;
            height: auto;
            flex-flow: row wrap;
            text-align:center;
        }
        #svg-container >svg{
            display: block;
            margin:10px auto;
            margin-bottom: 0;
        }
        #copyright{
            bottom: 0;
            left: 50%;
            margin: 5px auto;
            font-size: 16px;
            color: #515151;
        }
        #copyright >a{
            text-decoration: none;
            color: #77C;
        }
        .number{
            position: absolute;
            top:0;
            left:0;
            border-top:22px solid #08a1ef;
            border-right: 22px solid transparent;
        }
        .pagenum{
            font-size: 12px;
            color: #fff;
            position: absolute;
            top: -23px;
            left: 2px;
        }
            #navBar::-webkit-scrollbar{
            width: 8px;
            background-color: #f5f5f5;
        }
            #navBar::-webkit-scrollbar-track{
            -webkit-box-shadow: inset 0 0 4px rgba(0,0,0,.3);
            border-radius: 8px;
            background-color: #fff;
        }
            #navBar::-webkit-scrollbar-thumb{
            border-radius: 8px;
            -webkit-box-shadow: inset 0 0 4px rgba(0,0,0,.3);
            background-color: #6b6b70;
        }
        #navBar::-webkit-scrollbar-thumb:hover{
            background-color: #4a4a4f;
        }    
    </style>
  </head>
  <body>
    <div id="main-area">
      <div id="main-content">
        <div id="svg-container"><svg ed:vSpacing="30" height="728" viewBox="0 0 1394 728" xmlns:ed="https://www.edrawsoft.cn/xml/2017/SVGExtensions/" ed:hSpacing="30" xmlns:xlink="http://www.w3.org/1999/xlink" preserveAspectRadio="xMinYMin meet" ed:name="TiUPCluster部署规划" id="page2" xmlns="http://www.w3.org/2000/svg" width="1394" xmlns:ev="http://www.w3.org/2001/xml-events">
    <style type="text/css"><![CDATA[
g[ed\:togtopicid],g[ed\:hyperlink],g[ed\:comment],g[ed\:note] {cursor:pointer;}
g[id] {-moz-user-select: none;-ms-user-select: none;user-select: none;}
svg text::selection,svg tspan::selection{background-color: #4285f4;color: #ffffff;fill: #ffffff;}
.st4 {fill:#303030;font-family:;font-size:11.25pt}
.st3 {fill:#454545;font-family:;font-size:9pt}
.st1 {fill:#ffffff;font-family:;font-size:14.25pt}
.st2 {font-family:.AppleSystemUIFont}
]]></style>
    <defs/>
    <rect x="0" height="728" y="0" fill="#ffffff" width="1394"/>
    <path d="M-13.5,110.6C5.4,110.6,-13.5,-110.6,13.5,-110.6" stroke-linecap="round" stroke="#696969" id="105" ed:parentid="169" transform="matrix(1,0,0,1,468.16,284.63)" ed:tosuperid="104" fill="none" stroke-linejoin="round"/>
    <path d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" stroke="#696969" id="107" ed:parentid="104" transform="matrix(1,0,0,1,609.16,174)" ed:tosuperid="106" fill="none" stroke-linejoin="round"/>
    <path d="M-13.5,1.6C-2.3,1.6,4.5,-1.6,13.5,-1.6" stroke-linecap="round" stroke="#696969" id="109" ed:parentid="169" transform="matrix(1,0,0,1,468.16,393.63)" ed:tosuperid="108" fill="none" stroke-linejoin="round"/>
    <path d="M-13.5,61.5L3.7,61.5L3.7,-61.5L13.5,-61.5" stroke-linecap="round" stroke="#696969" id="111" ed:parentid="108" transform="matrix(1,0,0,1,681.16,330.5)" ed:tosuperid="110" fill="none" stroke-linejoin="round"/>
    <path d="M3.7,-23.8L3.7,23.8L13.5,23.8" stroke-linecap="round" stroke="#696969" id="113" ed:parentid="108" transform="matrix(1,0,0,1,681.16,415.75)" ed:tosuperid="112" fill="none" stroke-linejoin="round"/>
    <path d="M-13.5,65.5L3.7,65.5L3.7,-65.5L13.5,-65.5" stroke-linecap="round" stroke="#696969" id="117" ed:parentid="112" transform="matrix(1,0,0,1,774.16,374)" ed:tosuperid="116" fill="none" stroke-linejoin="round"/>
    <path d="M-13.5,6.3L3.7,6.3L3.7,-6.3L13.5,-6.3" stroke-linecap="round" stroke="#696969" id="119" ed:parentid="116" transform="matrix(1,0,0,1,846.08,302.25)" ed:tosuperid="118" fill="none" stroke-linejoin="round"/>
    <path d="M3.7,-7.3L3.7,7.3L13.5,7.3" stroke-linecap="round" stroke="#696969" id="121" ed:parentid="116" transform="matrix(1,0,0,1,846.08,315.75)" ed:tosuperid="120" fill="none" stroke-linejoin="round"/>
    <path d="M3.7,27.5L3.7,-27.5L13.5,-27.5" stroke-linecap="round" stroke="#696969" id="123" ed:parentid="112" transform="matrix(1,0,0,1,774.16,412)" ed:tosuperid="122" fill="none" stroke-linejoin="round"/>
    <path d="M-13.5,17.3L3.7,17.3L3.7,-17.3L13.5,-17.3" stroke-linecap="round" stroke="#696969" id="125" ed:parentid="122" transform="matrix(1,0,0,1,845.87,367.25)" ed:tosuperid="124" fill="none" stroke-linejoin="round"/>
    <path d="M3.7,-4.8L3.7,4.8L13.5,4.8" stroke-linecap="round" stroke="#696969" id="127" ed:parentid="122" transform="matrix(1,0,0,1,845.88,389.25)" ed:tosuperid="126" fill="none" stroke-linejoin="round"/>
    <path d="M3.7,-18.3L3.7,18.3L13.5,18.3" stroke-linecap="round" stroke="#696969" id="129" ed:parentid="122" transform="matrix(1,0,0,1,845.88,402.75)" ed:tosuperid="128" fill="none" stroke-linejoin="round"/>
    <path d="M3.7,-10.5L3.7,10.5L13.5,10.5" stroke-linecap="round" stroke="#696969" id="131" ed:parentid="112" transform="matrix(1,0,0,1,774.16,450)" ed:tosuperid="130" fill="none" stroke-linejoin="round"/>
    <path d="M-13.5,6.3L3.7,6.3L3.7,-6.3L13.5,-6.3" stroke-linecap="round" stroke="#696969" id="133" ed:parentid="130" transform="matrix(1,0,0,1,834.76,454.25)" ed:tosuperid="132" fill="none" stroke-linejoin="round"/>
    <path d="M3.7,-7.3L3.7,7.3L13.5,7.3" stroke-linecap="round" stroke="#696969" id="135" ed:parentid="130" transform="matrix(1,0,0,1,834.77,467.75)" ed:tosuperid="134" fill="none" stroke-linejoin="round"/>
    <path d="M3.7,-30.8L3.7,30.8L13.5,30.8" stroke-linecap="round" stroke="#696969" id="137" ed:parentid="112" transform="matrix(1,0,0,1,774.16,470.25)" ed:tosuperid="136" fill="none" stroke-linejoin="round"/>
    <path d="M-13.5,-0.5L3.7,-0.5L3.7,0.5L13.5,0.5" stroke-linecap="round" stroke="#696969" id="139" ed:parentid="136" transform="matrix(1,0,0,1,888.06,501.5)" ed:tosuperid="138" fill="none" stroke-linejoin="round"/>
    <path d="M3.7,-57.8L3.7,57.8L13.5,57.8" stroke-linecap="round" stroke="#696969" id="141" ed:parentid="112" transform="matrix(1,0,0,1,774.15,497.25)" ed:tosuperid="140" fill="none" stroke-linejoin="round"/>
    <path d="M-13.5,13L3.7,13L3.7,-13L13.5,-13" stroke-linecap="round" stroke="#696969" id="143" ed:parentid="140" transform="matrix(1,0,0,1,851.97,542)" ed:tosuperid="142" fill="none" stroke-linejoin="round"/>
    <path d="M3.7,-0.5L3.7,0.5L13.5,0.5" stroke-linecap="round" stroke="#696969" id="145" ed:parentid="140" transform="matrix(1,0,0,1,851.97,555.5)" ed:tosuperid="144" fill="none" stroke-linejoin="round"/>
    <path d="M3.7,-14L3.7,14L13.5,14" stroke-linecap="round" stroke="#696969" id="147" ed:parentid="140" transform="matrix(1,0,0,1,851.97,569)" ed:tosuperid="146" fill="none" stroke-linejoin="round"/>
    <path d="M-13.5,-107.4C5.4,-107.4,-13.5,107.4,13.5,107.4" stroke-linecap="round" stroke="#696969" id="149" ed:parentid="169" transform="matrix(1,0,0,1,468.16,502.63)" ed:tosuperid="148" fill="none" stroke-linejoin="round"/>
    <path d="M-13.5,-120.9C5.4,-120.9,-13.5,120.9,13.5,120.9" stroke-linecap="round" stroke="#696969" id="158" ed:parentid="169" transform="matrix(1,0,0,1,468.16,516.13)" ed:tosuperid="157" fill="none" stroke-linejoin="round"/>
    <path d="M28.4,-162.8L56.4,-162.8L56.4,162.8L91.4,162.8" stroke-linecap="round" stroke="#696969" id="164" ed:parentid="103" transform="matrix(1,0,0,1,234.24,524.75)" ed:tosuperid="163" fill="none" stroke-linejoin="round"/>
    <path d="M-13.5,-5.1C-1.5,-5.1,2.6,5.1,13.5,5.1" stroke-linecap="round" stroke="#696969" id="166" ed:parentid="163" transform="matrix(1,0,0,1,438.16,692.63)" ed:tosuperid="165" fill="none" stroke-linejoin="round"/>
    <path d="M28.4,146.1L56.4,146.1L56.4,-146.1L91.4,-146.1" stroke-linecap="round" stroke="#696969" id="168" ed:parentid="103" transform="matrix(1,0,0,1,234.24,215.88)" ed:tosuperid="167" fill="none" stroke-linejoin="round"/>
    <path d="M28.4,-16.6L56.4,-16.6L56.4,16.6L91.4,16.6" stroke-linecap="round" stroke="#696969" id="170" ed:parentid="103" transform="matrix(1,0,0,1,234.24,378.63)" ed:tosuperid="169" fill="none" stroke-linejoin="round"/>
    <path d="M-13.5,15.1C0.8,15.1,-2.7,-15.1,13.5,-15.1" stroke-linecap="round" stroke="#696969" id="172" ed:parentid="167" transform="matrix(1,0,0,1,483.16,54.63)" ed:tosuperid="171" fill="none" stroke-linejoin="round"/>
    <path d="M-13.5,1.6C-2.3,1.6,4.5,-1.6,13.5,-1.6" stroke-linecap="round" stroke="#696969" id="174" ed:parentid="167" transform="matrix(1,0,0,1,483.16,68.13)" ed:tosuperid="173" fill="none" stroke-linejoin="round"/>
    <path d="M-13.5,-11.9C0,-11.9,-1,11.9,13.5,11.9" stroke-linecap="round" stroke="#696969" id="176" ed:parentid="167" transform="matrix(1,0,0,1,483.16,81.63)" ed:tosuperid="175" fill="none" stroke-linejoin="round"/>
    <path d="M-13.5,-25.4C2.8,-25.4,-7.3,25.4,13.5,25.4" stroke-linecap="round" stroke="#696969" id="178" ed:parentid="167" transform="matrix(1,0,0,1,483.16,95.13)" ed:tosuperid="177" fill="none" stroke-linejoin="round"/>
    <g ed:topictype="mainidea" ed:height="57" id="103" ed:layout="map" transform="matrix(1,0,0,1,23,333.5)" ed:width="239.656">
        <path d="M4,0L235.7,0C238.3,0,239.7,1.3,239.7,4L239.7,53C239.7,55.7,238.3,57,235.7,57L4,57C1.3,57,0,55.7,0,53L0,4C0,1.3,1.3,0,4,0z" stroke-width="3" stroke="#435fbc" fill="#435fbc" stroke-linejoin="round"/>
        <text class="st1">
            <tspan x="26" style="white-space:pre" class="st2" y="36.5">TiUP Cluster 部署规划</tspan>
        </text>
    </g>
    <g ed:height="20.5" id="104" ed:parentid="169" ed:layout="rightmap" transform="matrix(1,0,0,1,481.66,153.5)" ed:width="114">
        <path d="M0,20.5L114,20.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="8" style="white-space:pre" class="st2" y="14.7">操作系统版本要求</tspan>
        </text>
    </g>
    <g ed:height="20.5" id="106" ed:parentid="104" ed:layout="rightmap" transform="matrix(1,0,0,1,622.65,153.5)" ed:width="262.688">
        <path d="M0,20.5L262.7,20.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="11" style="white-space:pre" class="st2" y="14.7">仅支持 CentOS 7.3 及以上 Linux 操作系统。</tspan>
        </text>
    </g>
    <g ed:height="20.5" id="108" ed:parentid="169" ed:layout="rightmap" transform="matrix(1,0,0,1,481.66,371.5)" ed:width="186">
        <path d="M0,20.5L186,20.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="8" style="white-space:pre" class="st2" y="14.7">生产环境服务器数量和配置参数</tspan>
        </text>
    </g>
    <g ed:height="88.5" id="110" ed:parentid="108" ed:layout="rightmap" transform="matrix(1,0,0,1,694.66,180.5)" ed:width="665.578">
        <path d="M0,88.5L665.6,88.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="8" style="white-space:pre" class="st2" y="14.6">组件	CPU	内存	硬盘类型	磁盘大小	网络	机器数量</tspan>
            <tspan x="8" style="white-space:pre" class="st2" y="31.6">PD	>= 4 Core	>= 8 GB	SSD	>= 300 GB	>= 1 块万兆网卡	3</tspan>
            <tspan x="8" style="white-space:pre" class="st2" y="48.6">TiDB	>= 16 Core	>= 32 GB	SAS/SSD	>= 300 GB	>= 1 块万兆网卡	2</tspan>
            <tspan x="8" style="white-space:pre" class="st2" y="65.7">TiKV	>= 16 Core	>= 32 GB	SSD	&lt;= 2 TB	>= 1 块万兆网卡	3</tspan>
            <tspan x="8" style="white-space:pre" class="st2" y="82.7">Prometheus	>= 8 Core	>= 16 GB	SAS/SSD	>= 300 GB	>= 1 块千兆网卡或者万兆网卡	1</tspan>
        </text>
    </g>
    <g ed:height="20.5" id="112" ed:parentid="108" ed:layout="rightmap" transform="matrix(1,0,0,1,694.66,419)" ed:width="66">
        <path d="M0,20.5L66,20.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="8" style="white-space:pre" class="st2" y="14.7">注意事项</tspan>
        </text>
    </g>
    <g ed:height="18.5" id="116" ed:parentid="112" ed:layout="rightmap" transform="matrix(1,0,0,1,787.66,290)" ed:width="44.9219">
        <path d="M0,18.5L44.9,18.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="8" style="white-space:pre" class="st2" y="12.7">TiDB</tspan>
        </text>
    </g>
    <g ed:height="20.5" id="118" ed:parentid="116" ed:layout="rightmap" transform="matrix(1,0,0,1,859.58,275.5)" ed:width="413.281">
        <path d="M0,20.5L413.3,20.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="11" style="white-space:pre" class="st2" y="14.7">机器数量 >= 2 台，若每台机器资源较丰富，则建议部署多个 tidb-server</tspan>
        </text>
    </g>
    <g ed:height="20.5" id="120" ed:parentid="116" ed:layout="rightmap" transform="matrix(1,0,0,1,859.58,302.5)" ed:width="247.891">
        <path d="M0,20.5L247.9,20.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="9" style="white-space:pre" class="st2" y="14.7">磁盘建议 SAS/SSD，建议容量 >= 300 GB</tspan>
        </text>
    </g>
    <g ed:height="18.5" id="122" ed:parentid="112" ed:layout="rightmap" transform="matrix(1,0,0,1,787.66,366)" ed:width="44.7188">
        <path d="M0,18.5L44.7,18.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="8" style="white-space:pre" class="st2" y="12.7">TiKV</tspan>
        </text>
    </g>
    <g ed:height="20.5" id="124" ed:parentid="122" ed:layout="rightmap" transform="matrix(1,0,0,1,859.37,329.5)" ed:width="399.391">
        <path d="M0,20.5L399.4,20.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="12" style="white-space:pre" class="st2" y="14.7">机器数量 >= 3 台，若每台机器有多块磁盘，建议部署多个 tikv-server</tspan>
        </text>
    </g>
    <g ed:height="37.5" id="126" ed:parentid="122" ed:layout="rightmap" transform="matrix(1,0,0,1,859.38,356.5)" ed:width="514">
        <path d="M0,37.5L514,37.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="8" style="white-space:pre" class="st2" y="14.6">部署推荐配置 label，配置后系统才会将根据 label 将数据分布在不同的机房、机架、机器防止</tspan>
            <tspan x="8" style="white-space:pre" class="st2" y="31.6">有单个机房或者机架或者机器宕机时，系统才具备容灾能力</tspan>
        </text>
    </g>
    <g ed:height="20.5" id="128" ed:parentid="122" ed:layout="rightmap" transform="matrix(1,0,0,1,859.38,400.5)" ed:width="462.438">
        <path d="M0,20.5L462.4,20.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="12" style="white-space:pre" class="st2" y="14.7">磁盘建议采用 SSD，其中建议 PCI-E SSD 容量 >= 2 TB 普通 SSD 容量 >= 1.5 TB</tspan>
        </text>
    </g>
    <g ed:height="18.5" id="130" ed:parentid="112" ed:layout="rightmap" transform="matrix(1,0,0,1,787.65,442)" ed:width="33.6094">
        <path d="M0,18.5L33.6,18.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="7" style="white-space:pre" class="st2" y="12.7">PD</tspan>
        </text>
    </g>
    <g ed:height="20.5" id="132" ed:parentid="130" ed:layout="rightmap" transform="matrix(1,0,0,1,848.26,427.5)" ed:width="116.031">
        <path d="M0,20.5L116,20.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="10" style="white-space:pre" class="st2" y="14.7">机器数量 >= 3 台</tspan>
        </text>
    </g>
    <g ed:height="20.5" id="134" ed:parentid="130" ed:layout="rightmap" transform="matrix(1,0,0,1,848.27,454.5)" ed:width="244.438">
        <path d="M0,20.5L244.4,20.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="9" style="white-space:pre" class="st2" y="14.7">磁盘建议采用 SSD，容量建议 >= 300 GB</tspan>
        </text>
    </g>
    <g ed:height="18.5" id="136" ed:parentid="112" ed:layout="rightmap" transform="matrix(1,0,0,1,787.65,482.5)" ed:width="86.9063">
        <path d="M0,18.5L86.9,18.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="8" style="white-space:pre" class="st2" y="12.7">Prometheus</tspan>
        </text>
    </g>
    <g ed:height="20.5" id="138" ed:parentid="136" ed:layout="rightmap" transform="matrix(1,0,0,1,901.56,481.5)" ed:width="309.172">
        <path d="M0,20.5L309.2,20.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="9" style="white-space:pre" class="st2" y="14.7">机器数量 1 台，若资源较紧张，可与其他组件混合部署</tspan>
        </text>
    </g>
    <g ed:height="18.5" id="140" ed:parentid="112" ed:layout="rightmap" transform="matrix(1,0,0,1,787.65,536.5)" ed:width="50.8125">
        <path d="M0,18.5L50.8,18.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="8" style="white-space:pre" class="st2" y="12.7">Other</tspan>
        </text>
    </g>
    <g ed:height="20.5" id="142" ed:parentid="140" ed:layout="rightmap" transform="matrix(1,0,0,1,865.47,508.5)" ed:width="401.031">
        <path d="M0,20.5L401,20.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="8" style="white-space:pre" class="st2" y="14.7">若资源较紧张时 TiDB、PD、Prometheus 可混合部署在同一台服务器上</tspan>
        </text>
    </g>
    <g ed:height="20.5" id="144" ed:parentid="140" ed:layout="rightmap" transform="matrix(1,0,0,1,865.47,535.5)" ed:width="306">
        <path d="M0,20.5L306,20.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="8" style="white-space:pre" class="st2" y="14.7">若对性能、可靠性有更高的要求，建议按组件分别部署</tspan>
        </text>
    </g>
    <g ed:height="20.5" id="146" ed:parentid="140" ed:layout="rightmap" transform="matrix(1,0,0,1,865.47,562.5)" ed:width="198">
        <path d="M0,20.5L198,20.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="8" style="white-space:pre" class="st2" y="14.7">生产环境强烈推荐使用更高的配置</tspan>
        </text>
    </g>
    <g ed:height="20.5" id="148" ed:parentid="169" ed:layout="rightmap" transform="matrix(1,0,0,1,481.66,589.5)" ed:width="114">
        <path d="M0,20.5L114,20.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="8" style="white-space:pre" class="st2" y="14.7">生产环境网络要求</tspan>
        </text>
    </g>
    <g style="display:none" ed:height="37.5" id="150" ed:parentid="148" ed:layout="rightmap" transform="matrix(1,0,0,1,622.66,208.5)" ed:width="514">
        <path d="M0,37.5L514,37.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="8" style="white-space:pre" class="st2" y="14.6">TiDB 正常运行需要网络环境提供如下的网络端口配置，管理员可根据实际 TiDB 组件部署的需</tspan>
            <tspan x="8" style="white-space:pre" class="st2" y="31.6">要进行调整</tspan>
        </text>
    </g>
    <g style="display:none" ed:height="258.5" id="155" ed:parentid="148" ed:layout="rightmap" transform="matrix(1,0,0,1,622.66,252.5)" ed:width="561.984">
        <path d="M0,258.5L562,258.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="12" style="white-space:pre" class="st2" y="14.7">组件	默认端口	说明</tspan>
            <tspan x="12" style="white-space:pre" class="st2" y="31.7">TiDB	4000	应用及 DBA 工具访问通信端口</tspan>
            <tspan x="12" style="white-space:pre" class="st2" y="48.7">TiDB	10080	TiDB 状态信息上报通信端口</tspan>
            <tspan x="12" style="white-space:pre" class="st2" y="65.7">TiKV	20160	TiKV 通信端口</tspan>
            <tspan x="12" style="white-space:pre" class="st2" y="82.7">PD	2379	提供 TiDB 和 PD 通信端口</tspan>
            <tspan x="12" style="white-space:pre" class="st2" y="99.7">PD	2380	PD 集群节点间通信端口</tspan>
            <tspan x="12" style="white-space:pre" class="st2" y="116.7">Pump	8250	Pump 通信端口</tspan>
            <tspan x="12" style="white-space:pre" class="st2" y="133.7">Drainer	8249	Drainer 通信端口</tspan>
            <tspan x="12" style="white-space:pre" class="st2" y="150.7">Prometheus	9090	Prometheus 服务通信端口</tspan>
            <tspan x="12" style="white-space:pre" class="st2" y="167.7">Pushgateway	9091	TiDB，TiKV，PD 监控聚合和上报端口</tspan>
            <tspan x="12" style="white-space:pre" class="st2" y="184.7">Node_exporter	9100	TiDB 集群每个节点的系统信息上报通信端口</tspan>
            <tspan x="12" style="white-space:pre" class="st2" y="201.7">Blackbox_exporter	9115	Blackbox_exporter 通信端口，用于 TiDB 集群端口监控</tspan>
            <tspan x="12" style="white-space:pre" class="st2" y="218.7">Grafana	3000	Web 监控服务对外服务和客户端(浏览器)访问端口</tspan>
            <tspan x="12" style="white-space:pre" class="st2" y="235.7">Grafana	8686	grafana_collector 通信端口，用于将 Dashboard 导出为 PDF 格式</tspan>
            <tspan x="12" style="white-space:pre" class="st2" y="252.7">Kafka_exporter	9308	Kafka_exporter 通信端口，用于监控 binlog kafka 集群</tspan>
        </text>
    </g>
    <g ed:height="20.5" id="157" ed:parentid="169" ed:layout="rightmap" transform="matrix(1,0,0,1,481.65,616.5)" ed:width="97.10939999999999">
        <path d="M0,20.5L97.1,20.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="9" style="white-space:pre" class="st2" y="14.7">topology 配置</tspan>
        </text>
    </g>
    <g style="display:none" ed:height="363.5" id="159" ed:parentid="157" ed:layout="rightmap" transform="matrix(1,0,0,1,605.76,198.75)" ed:width="665.266">
        <path d="M0,363.5L665.3,363.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="32" style="white-space:pre" class="st2" y="12.7">---</tspan>
            <tspan x="32" style="white-space:pre" class="st2" y="42.7">pd_servers:</tspan>
            <tspan x="32" style="white-space:pre" class="st2" y="57.7">  - ip: 10.9.1.1</tspan>
            <tspan x="32" style="white-space:pre" class="st2" y="72.7">  - ip: 10.9.1.2</tspan>
            <tspan x="32" style="white-space:pre" class="st2" y="87.7">  - ip: 10.9.1.3</tspan>
            <tspan x="32" style="white-space:pre" class="st2" y="117.7">tidb_servers:</tspan>
            <tspan x="32" style="white-space:pre" class="st2" y="132.7">  - ip: 10.9.1.2</tspan>
            <tspan x="32" style="white-space:pre" class="st2" y="147.7">  - ip: 10.9.1.3</tspan>
            <tspan x="32" style="white-space:pre" class="st2" y="177.7">tikv_servers:</tspan>
            <tspan x="32" style="white-space:pre" class="st2" y="192.7">  - ip: 10.9.1.4</tspan>
            <tspan x="32" style="white-space:pre" class="st2" y="207.7">    ## The value of label can be customized, for example: 'zone=z1,rack=r1,host=h1' or 'a=a1,b=b1,c=c1', etc</tspan>
            <tspan x="32" style="white-space:pre" class="st2" y="222.7">    ## Can only set in tikv_servers</tspan>
            <tspan x="32" style="white-space:pre" class="st2" y="237.7">    # label: host=h1</tspan>
            <tspan x="32" style="white-space:pre" class="st2" y="252.7">  - ip: 10.9.1.5</tspan>
            <tspan x="32" style="white-space:pre" class="st2" y="267.6">  - ip: 10.9.1.6</tspan>
            <tspan x="32" style="white-space:pre" class="st2" y="297.6">monitoring_server:</tspan>
            <tspan x="32" style="white-space:pre" class="st2" y="312.6">  - ip: 10.9.1.7</tspan>
            <tspan x="32" style="white-space:pre" class="st2" y="342.6">grafana_server:</tspan>
            <tspan x="32" style="white-space:pre" class="st2" y="357.6">  - ip: 10.9.1.7</tspan>
        </text>
    </g>
    <g style="display:none" ed:height="37.5" id="161" ed:parentid="157" ed:layout="rightmap" transform="matrix(1,0,0,1,605.77,568.75)" ed:width="514">
        <path d="M0,37.5L514,37.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="8" style="white-space:pre" class="st2" y="14.6">label 可以自定义，例如：'zone=z1,rack=r1,host=h1' 或 'a=a1,b=b1,c=c1'。label 只能在 </tspan>
            <tspan x="8" style="white-space:pre" class="st2" y="31.6">tikv_servers 上设置。</tspan>
        </text>
    </g>
    <g ed:height="35" id="163" ed:parentid="103" ed:layout="rightmap" transform="matrix(1,0,0,1,325.66,670)" ed:width="99">
        <path d="M4,0L95,0C97.7,0,99,1.3,99,4L99,31C99,33.7,97.7,35,95,35L4,35C1.3,35,0,33.7,0,31L0,4C0,1.3,1.3,0,4,0z" stroke="#ebecf3" fill="#ebecf3" stroke-linejoin="round"/>
        <text class="st4">
            <tspan x="18" style="white-space:pre" class="st2" y="24">部署方法</tspan>
        </text>
    </g>
    <g ed:height="20.5" id="165" ed:parentid="163" ed:layout="rightmap" transform="matrix(1,0,0,1,451.66,677.25)" ed:width="156.578">
        <path d="M0,20.5L156.6,20.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="8" style="white-space:pre" class="st2" y="14.7">用 TiUP 部署生产集群</tspan>
        </text>
        <g ed:hyperlink="[&#xa;    {&#xa;        &quot;desc&quot;: &quot;用 TiUP 部署生产集群&quot;,&#xa;        &quot;link&quot;: &quot;366&quot;&#xa;    }&#xa;]&#xa;">
            <use xlink:href="" transform="translate(131,4)"/>
        </g>
    </g>
    <symbol id="imghyperlink">
        <image height="16" xlink:href="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAAAAXNSR0IArs4c6QAAAERlWElmTU0AKgAAAAgAAYdpAAQAAAABAAAAGgAAAAAAA6ABAAMAAAABAAEAAKACAAQAAAABAAAAEKADAAQAAAABAAAAEAAAAAA0VXHyAAACQElEQVQ4EWNgIAJ8//U/Ze2Jd5/XnXz3/+O3P5HIWliQOdjY777+rkqbea/1338GBhZmRoaVx94te/3pt6koH2sRNvUoYh++/W6PmnT3T8Wyx////fsPBg2rn/4P6bvz583nP5NRFKNzfv7+Xxk18c5vZM0QI/7/BxkSCjQEqKYGXR+cv/7Uu0+RE+/AbX7w6uf/bz+hzgCalDDt3v+DVz9+ZYLrQGf8Z/jPyACEjAwMZ+99Y4iYeIfh2ftfcFVAYTBAMQAY2qlrTr77shYY2m4GAjuBZvxJn/WAIWfeA4a+ODkGZXF2sKbmtc8YPn3/99dMjWcCzCAGYGhX58171PIfGto/fv1jiLMXvV6y+KGGlToP45x0Rbjmiw+//52RojBLlJ8lC2wAMLQ7suY8KpEXYWNui5QBK8yZ+4hh39WP/9sipe+sPv5BmYmJgQkUjV9//GOYmaqwQYiXJRCsEBbalcCogoEzd7/+t6i++j9j9n1YaK89fuvLn8PXP3//9uNvMVgjjFh/8t2nqIl3YXr/3weGNkjzsZufwWKg0D507fN3IIcTpgeZBqVEoK8RQIyfhWFxjjKDigQkwEB+ZGT8z4FQgcpi8jIS7AKFdvWKJ2AZLjYmuOamNc8Yvvz4989YmbuAkZHxO6pWJN77L797Iife/VO1HBEOTWue/g/uvfPn9cc/05GUYjDh0QgyJGfeo0JgwmFiYWJk+Pj9789ZqYrzgVGViaELl8D3X3+SQaF95Mbnb8DYacalDlkcACu8ZG0/oWGoAAAAAElFTkSuQmCC" width="16"/>
    </symbol>
    <g ed:height="35" id="167" ed:parentid="103" ed:layout="rightmap" transform="matrix(1,0,0,1,325.66,52.25)" ed:width="144">
        <path d="M4,0L140,0C142.7,0,144,1.3,144,4L144,31C144,33.7,142.7,35,140,35L4,35C1.3,35,0,33.7,0,31L0,4C0,1.3,1.3,0,4,0z" stroke="#ebecf3" fill="#ebecf3" stroke-linejoin="round"/>
        <text class="st4">
            <tspan x="18" style="white-space:pre" class="st2" y="24">跨数据中心方案</tspan>
        </text>
    </g>
    <g ed:height="35" id="169" ed:parentid="103" ed:layout="rightmap" transform="matrix(1,0,0,1,325.66,377.75)" ed:width="129">
        <path d="M4,0L125,0C127.7,0,129,1.3,129,4L129,31C129,33.7,127.7,35,125,35L4,35C1.3,35,0,33.7,0,31L0,4C0,1.3,1.3,0,4,0z" stroke="#ebecf3" fill="#ebecf3" stroke-linejoin="round"/>
        <text class="st4">
            <tspan x="18" style="white-space:pre" class="st2" y="24">系统网络要求</tspan>
        </text>
    </g>
    <g ed:height="20.5" id="171" ed:parentid="167" ed:layout="rightmap" transform="matrix(1,0,0,1,496.65,19)" ed:width="214.547">
        <path d="M0,20.5L214.5,20.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="9" style="white-space:pre" class="st2" y="14.7">两中心异步复制方案（binlog 复制）</tspan>
        </text>
    </g>
    <g ed:height="20.5" id="173" ed:parentid="167" ed:layout="rightmap" transform="matrix(1,0,0,1,496.65,46)" ed:width="240.688">
        <path d="M0,20.5L240.7,20.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="8" style="white-space:pre" class="st2" y="14.7">两中心同步复制方案（Raft 算法 三副本）</tspan>
        </text>
    </g>
    <g ed:height="20.5" id="175" ed:parentid="167" ed:layout="rightmap" transform="matrix(1,0,0,1,496.65,73)" ed:width="178.969">
        <path d="M0,20.5L179,20.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="10" style="white-space:pre" class="st2" y="14.7">两地三中心（binlog + Raft）</tspan>
        </text>
    </g>
    <g ed:height="20.5" id="177" ed:parentid="167" ed:layout="rightmap" transform="matrix(1,0,0,1,496.65,100)" ed:width="136.609">
        <path d="M0,20.5L136.6,20.5" stroke="#696969" fill="none" stroke-linejoin="round"/>
        <text class="st3">
            <tspan x="7" style="white-space:pre" class="st2" y="14.7">AWS 跨 AZ 部署 TiDB</tspan>
        </text>
    </g>
</svg>
</div>
        <div id="copyright">Created With  <a href="https://www.edrawsoft.com/" target="_blank" title="edrawsoft">MindMaster</a></div>
      </div>
    </div>
    <script>eval(atob('dmFyIG11YT13aW5kb3cubmF2aWdhdG9yLnVzZXJBZ2VudDsNCnZhciB1YT0obXVhLmluZGV4T2YoJ3J2OjExJykrbXVhLmluZGV4T2YoJ01TSUUnKSk+PTA7DQpkb2N1bWVudC5nZXRFbGVtZW50QnlJZCgnc3ZnLWNvbnRhaW5lcicpLm9uY29udGV4dG1lbnUgPSBmdW5jdGlvbiAoZXZlbnQpIHsNCiAgICBldmVudC5wcmV2ZW50RGVmYXVsdCgpOw0KfQ0KZG9jdW1lbnQuZ2V0RWxlbWVudEJ5SWQoJ3N2Zy1jb250YWluZXInKS5vbm1vdXNlZG93biA9IGZ1bmN0aW9uIChldmVudCkgew0KICAgIGlmKGV2ZW50LndoaWNoID09Myl7DQogICAgICAgIHRoaXMuc3R5bGUuY3Vyc29yID0gJ3BvaW50ZXInOw0KICAgICAgICB0aGlzLm9ubW91c2Vtb3ZlID0gZnVuY3Rpb24gKGV2KSB7DQogICAgICAgICAgICB0aGlzLnNjcm9sbEJ5KC0oZXYubW92ZW1lbnRYKSwgMCk7DQogICAgICAgICAgICB3aW5kb3cuc2Nyb2xsQnkoMCwtKGV2Lm1vdmVtZW50WSkpDQogICAgICAgIH0NCiAgICAgICAgdGhpcy5vbm1vdXNldXAgPSBmdW5jdGlvbiAoKSB7DQogICAgICAgICAgICB0aGlzLnN0eWxlLmN1cnNvciA9ICBudWxsOw0KICAgICAgICAgICAgdGhpcy5vbm1vdXNldXAgPSBudWxsOw0KICAgICAgICAgICAgdGhpcy5vbm1vdXNlbW92ZSA9IG51bGw7DQogICAgICAgIH0NCiAgICB9DQp9DQpOdW1iZXIucHJvdG90eXBlLnRvc3VpdHN2Zz1mdW5jdGlvbiAoKSB7DQogICAgdmFyIG51bT10aGlzLnZhbHVlT2YoKTsNCiAgICBpZihudW0lMT09PTApew0KICAgICAgICByZXR1cm4gbnVtKzAuNQ0KICAgIH1lbHNlIHJldHVybiBudW07DQp9Ow0KTnVtYmVyLnByb3RvdHlwZS5wbHVzej1mdW5jdGlvbigpIHsNCiAgICB2YXIgbnVtPXRoaXMudmFsdWVPZigpOw0KICAgIHJldHVybiBudW08MTA/JzAnK251bTpudW07DQp9Ow0KZnVuY3Rpb24gcGFyc2VEYXRlKG51bSkgew0KICAgIHZhciBkYXRlID0gbmV3IERhdGUobnVtKTsNCiAgICB2YXIgWSA9IGRhdGUuZ2V0RnVsbFllYXIoKSArICctJzsNCiAgICB2YXIgTSA9IChkYXRlLmdldE1vbnRoKCkrMSkucGx1c3ooKSArICctJzsNCiAgICB2YXIgRCA9IGRhdGUuZ2V0RGF0ZSgpLnBsdXN6KCkgKyAnICc7DQogICAgdmFyIGggPSBkYXRlLmdldEhvdXJzKCkucGx1c3ooKSArICc6JzsNCiAgICB2YXIgbW0gPSBkYXRlLmdldE1pbnV0ZXMoKS5wbHVzeigpICsgJzonOw0KICAgIHZhciBzID0gZGF0ZS5nZXRTZWNvbmRzKCkucGx1c3ooKTsNCiAgICByZXR1cm4gWStNK0QraCttbStzOw0KfQ0KLy8tLXByZWRlZmluZWQNCi8vY29tbWVudC0tDQp2YXIgY29tbWVudHM9ZG9jdW1lbnQucXVlcnlTZWxlY3RvckFsbCgnZz5nW2VkXFw6Y29tbWVudF0nKTsNCmZ1bmN0aW9uIGdldGN3aChwb3B1cCkgew0KICAgIGRvY3VtZW50LmJvZHkuZ2V0RWxlbWVudHNCeVRhZ05hbWUoJ3N2ZycpWzBdLmFwcGVuZENoaWxkKHBvcHVwKTsNCiAgICB2YXIgdz1wb3B1cC5nZXRCb3VuZGluZ0NsaWVudFJlY3QoKS53aWR0aDsNCiAgICB2YXIgaD1wb3B1cC5nZXRCb3VuZGluZ0NsaWVudFJlY3QoKS5oZWlnaHQ7DQogICAgcmV0dXJuIFt3LGhdDQp9DQpmb3IodmFyIGk9MDtpPGNvbW1lbnRzLmxlbmd0aDtpKyspew0KICAgIHZhciBwb3B1cD1kb2N1bWVudC5jcmVhdGVFbGVtZW50TlMoJ2h0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnJywnZycpOw0KICAgIHZhciBwb3B1cFI9IGRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdyZWN0Jyk7DQogICAgdmFyIGhvdmVyPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdyZWN0Jyk7DQogICAgdmFyIG9saW5lPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdyZWN0Jyk7DQogICAgaG92ZXIuc2V0QXR0cmlidXRlKCdmaWxsJywnI2NkY2RmZicpOw0KICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgneCcsJzAnKTsNCiAgICBob3Zlci5zZXRBdHRyaWJ1dGUoJ3knLCcwJyk7DQogICAgaG92ZXIuc2V0QXR0cmlidXRlKCdoZWlnaHQnLCcxNicpOw0KICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgnd2lkdGgnLCcxNicpOw0KICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgnZmlsbC1vcGFjaXR5JywnMC42Jyk7DQogICAgaG92ZXIuc2V0QXR0cmlidXRlKCd0cmFuc2Zvcm0nLGNvbW1lbnRzW2ldLnF1ZXJ5U2VsZWN0b3IoJ3VzZScpLmdldEF0dHJpYnV0ZSgndHJhbnNmb3JtJykpOw0KICAgIGhvdmVyLnN0eWxlLmRpc3BsYXk9J25vbmUnOw0KICAgIGNvbW1lbnRzW2ldLmFwcGVuZENoaWxkKGhvdmVyKTsNCiAgICB2YXIgYT1KU09OLnBhcnNlKGNvbW1lbnRzW2ldLmdldEF0dHJpYnV0ZSgnZWQ6Y29tbWVudCcpKTsNCiAgICB2YXIgaGVpZ2h0PTA7DQogICAgdmFyIGNhcnI9W107DQogICAgZm9yKHZhciBqPTA7ajxhLmxlbmd0aDtqKyspew0KICAgICAgICB2YXIgc3RhbXA9TnVtYmVyKGFbal0uRGF0ZSkqMTAwMDsNCiAgICAgICAgdmFyIHRpbWU9cGFyc2VEYXRlKHN0YW1wKTsNCiAgICAgICAgdmFyIG5hbWU9YVtqXS5OYW1lOw0KICAgICAgICB2YXIgbWVzc2FnZT1hW2pdLk1lc3NhZ2U7DQogICAgICAgIHZhciBtZXNzYWdlQXJyPW1lc3NhZ2Uuc3BsaXQoL1xuLyk7DQogICAgICAgIHZhciBvPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdnJyk7DQogICAgICAgIHZhciBuPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCd0ZXh0Jyk7DQogICAgICAgIHZhciB0PWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCd0ZXh0Jyk7DQogICAgICAgIHZhciBtPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCd0ZXh0Jyk7DQogICAgICAgIG4uc2V0QXR0cmlidXRlKCd4Jyw1KTsNCiAgICAgICAgbi5zZXRBdHRyaWJ1dGUoJ3knLDEyKTsNCiAgICAgICAgbi5zZXRBdHRyaWJ1dGUoJ2ZpbGwnLCcjMDA2ZWZmJyk7DQogICAgICAgIG4udGV4dENvbnRlbnQ9bmFtZSsnOiAnOw0KICAgICAgICBuLnNldEF0dHJpYnV0ZSgnZm9udC1zaXplJywnMTInKTsNCiAgICAgICAgdC5zZXRBdHRyaWJ1dGUoJ3gnLDIwMCk7DQogICAgICAgIHQuc2V0QXR0cmlidXRlKCd5JywxMik7DQogICAgICAgIHQuc2V0QXR0cmlidXRlKCdmaWxsJywnIzk2OTY5NicpOw0KICAgICAgICB0LnRleHRDb250ZW50PXRpbWU7DQogICAgICAgIHQuc2V0QXR0cmlidXRlKCdmb250LXNpemUnLCcxMCcpOw0KICAgICAgICBtLnNldEF0dHJpYnV0ZSgndHJhbnNmb3JtJywndHJhbnNsYXRlKDIwLDI3KScpOw0KICAgICAgICBtLnNldEF0dHJpYnV0ZSgnZm9udC1zaXplJywnMTInKTsNCiAgICAgICAgZm9yKHZhciBrPTA7azxtZXNzYWdlQXJyLmxlbmd0aDtrKyspew0KICAgICAgICAgICAgdmFyIHRzPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCd0c3BhbicpOw0KICAgICAgICAgICAgdHMuc2V0QXR0cmlidXRlKCd4JywnMCcpOw0KICAgICAgICAgICAgdHMuc2V0QXR0cmlidXRlKCd5JyxrKjE2KTsNCiAgICAgICAgICAgIHRzLnRleHRDb250ZW50PW1lc3NhZ2VBcnJba107DQogICAgICAgICAgICBtLmFwcGVuZENoaWxkKHRzKTsNCiAgICAgICAgfQ0KICAgICAgICBvLnNldEF0dHJpYnV0ZSgndHJhbnNmb3JtJywndHJhbnNsYXRlKDAsJytoZWlnaHQrJyknKTsNCiAgICAgICAgby5hcHBlbmRDaGlsZChuKTsNCiAgICAgICAgby5hcHBlbmRDaGlsZCh0KTsNCiAgICAgICAgby5hcHBlbmRDaGlsZChtKTsNCiAgICAgICAgY2Fyci5wdXNoKG8pOw0KICAgICAgICBwb3B1cC5hcHBlbmRDaGlsZChvKTsNCiAgICAgICAgaGVpZ2h0PShtZXNzYWdlQXJyLmxlbmd0aCsxKSoxNitoZWlnaHQ7DQogICAgfQ0KICAgIHZhciB3YXJyPWdldGN3aChwb3B1cCk7DQogICAgb2xpbmUuc2V0QXR0cmlidXRlKCd4JywnMCcpOw0KICAgIG9saW5lLnNldEF0dHJpYnV0ZSgneScsJzAnKTsNCiAgICB2YXIgb3c9d2FyclswXSsxMC41Ow0KICAgIHZhciBvaD13YXJyWzFdKzM7DQogICAgb2xpbmUuc2V0QXR0cmlidXRlKCd3aWR0aCcsb3cpOw0KICAgIG9saW5lLnNldEF0dHJpYnV0ZSgnaGVpZ2h0JyxvaCk7DQogICAgb2xpbmUuc2V0QXR0cmlidXRlKCdmaWxsJywnd2hpdGUnKTsNCiAgICBvbGluZS5zZXRBdHRyaWJ1dGUoJ3N0cm9rZScsJyM2NTY1NjUnKTsNCiAgICBwb3B1cC5hcHBlbmRDaGlsZChvbGluZSk7DQogICAgdmFyIGw9Y2Fyci5sZW5ndGg7DQogICAgd2hpbGUobC0tKXsNCiAgICAgICAgcG9wdXAuYXBwZW5kQ2hpbGQoY2FycltsXSk7DQogICAgfQ0KICAgIHBvcHVwLm9ubW91c2VvdmVyPWZ1bmN0aW9uICgpIHsNCiAgICAgICAgdGhpcy5zdHlsZS5kaXNwbGF5PSdibG9jayc7DQogICAgfTsNCiAgICBwb3B1cC5vbm1vdXNlb3V0PWZ1bmN0aW9uICgpIHsNCiAgICAgICAgdGhpcy5zdHlsZS5kaXNwbGF5ID0gJ25vbmUnOw0KICAgIH07DQogICAgdmFyIGNzPWNvbW1lbnRzW2ldLnF1ZXJ5U2VsZWN0b3IoJ3VzZScpLmdldEF0dHJpYnV0ZSgndHJhbnNmb3JtJykubWF0Y2goL1woKFxTKnxcUypcc1xTKilcKS8pWzFdLnNwbGl0KC8gfCwvKTsNCiAgICB2YXIgcHM9Y29tbWVudHNbaV0ucGFyZW50Tm9kZS5nZXRBdHRyaWJ1dGUoJ3RyYW5zZm9ybScpOw0KICAgIGlmKHBzLnN1YnN0cigwLDIpID09ICd0cicpew0KICAgICAgICB2YXIgcHBzID0gcHMubWF0Y2goL1woKFxTKnxcUypcc1xTKilcKS8pWzFdLnNwbGl0KC8gfCwvKTsNCiAgICAgICAgdmFyIHg9cGFyc2VGbG9hdChjc1swXSkrcGFyc2VGbG9hdChwcHNbMF0pOw0KICAgICAgICB2YXIgeT1wYXJzZUZsb2F0KHBwc1sxXSk7DQogICAgICAgIHg9eC50b3N1aXRzdmcoKTsNCiAgICAgICAgeT15LnRvc3VpdHN2ZygpOw0KICAgICAgICB2YXIgdHJzdHIgPSAndHJhbnNsYXRlKCcreCsnLCcreSsnKSc7DQogICAgfQ0KICAgIGVsc2UgaWYocHMuc3Vic3RyKDAsMikgPT0gJ21hJyl7DQogICAgICAgIHZhciBwcHMgPSBwcy5tYXRjaCgvKFwtP1xkKyhcLlxkKyk/KVtcLCBdKFwtP1xkKyhcLlxkKyk/KVtcLCBdKFwtP1xkKyhcLlxkKyk/KVtcLCBdKFwtP1xkKyhcLlxkKyk/KVtcLCBdKFwtP1xkKyhcLlxkKyk/KVtcLCBdKFwtP1xkKyhcLlxkKyk/KVwpJC8pOw0KICAgICAgICB2YXIgbWFBcnIgPSBbcGFyc2VGbG9hdChwcHNbMV0pLHBhcnNlRmxvYXQocHBzWzNdKSxwYXJzZUZsb2F0KHBwc1s1XSkscGFyc2VGbG9hdChwcHNbN10pLHBhcnNlRmxvYXQocHBzWzldKSxwYXJzZUZsb2F0KHBwc1sxMV0pXTsNCiAgICAgICAgaWYobWFBcnJbMV0gPT0gMCl7DQogICAgICAgICAgICB2YXIgeCA9IHBhcnNlRmxvYXQoY3NbMF0pOw0KICAgICAgICAgICAgdmFyIHk9IHBhcnNlRmxvYXQoY3NbMV0pKzE2Ow0KICAgICAgICAgICAgdmFyIHgxID0geCAqIG1hQXJyWzBdICsgeSAqIG1hQXJyWzJdICsgbWFBcnJbNF07DQogICAgICAgICAgICB2YXIgeTEgPSB4ICogbWFBcnJbMV0gKyB5ICogbWFBcnJbM10gKyBtYUFycls1XTsNCiAgICAgICAgICAgIHgxPXgxLnRvc3VpdHN2ZygpOw0KICAgICAgICAgICAgeTE9eTEudG9zdWl0c3ZnKCk7DQogICAgICAgICAgICB2YXIgdHJzdHIgPSAgJ3RyYW5zbGF0ZSgnK3gxKycsJyt5MSsnKSc7DQogICAgICAgIH1lbHNlew0KICAgICAgICAgICAgdmFyIHggPSBwYXJzZUZsb2F0KGNzWzBdKSsxNjsNCiAgICAgICAgICAgIHZhciB5ID0gcGFyc2VGbG9hdChjc1sxXSkrMTY7DQogICAgICAgICAgICB2YXIgeDEgPSB4ICogbWFBcnJbMF0gKyB5ICogbWFBcnJbMl0gKyBtYUFycls0XTsNCiAgICAgICAgICAgIHZhciB5MSA9IHggKiBtYUFyclsxXSArIHkgKiBtYUFyclszXSArIG1hQXJyWzVdOw0KICAgICAgICAgICAgeCA9IHBhcnNlRmxvYXQoY3NbMF0pKzE2Ow0KICAgICAgICAgICAgeSA9IHBhcnNlRmxvYXQoY3NbMV0pOw0KICAgICAgICAgICAgdmFyIHgyID0geCAqIG1hQXJyWzBdICsgeSAqIG1hQXJyWzJdICsgbWFBcnJbNF07DQogICAgICAgICAgICB2YXIgeTIgPSB4ICogbWFBcnJbMV0gKyB5ICogbWFBcnJbM10gKyBtYUFycls1XTsNCiAgICAgICAgICAgIHZhciBmeCA9IHgxPHgyP3gxLnRvc3VpdHN2ZygpOiB4Mi50b3N1aXRzdmcoKTsNCiAgICAgICAgICAgIHZhciBmeSA9IHkxPnkyP3kxLnRvc3VpdHN2ZygpOiB5Mi50b3N1aXRzdmcoKTsNCiAgICAgICAgICAgIHZhciBvZmZ5ID0gTWF0aC5hYnMoeTEteTIpOw0KICAgICAgICAgICAgdmFyIHRyc3RyID0gICd0cmFuc2xhdGUoJytmeCsnLCcrZnkrJyknOw0KICAgICAgICAgICAgcG9wdXBSLnNldEF0dHJpYnV0ZSgnaGVpZ2h0JyxvZmZ5LnRvU3RyaW5nKCkpOw0KICAgICAgICAgICAgcG9wdXBSLnNldEF0dHJpYnV0ZSgnd2lkdGgnLCcxNicpOw0KICAgICAgICAgICAgcG9wdXBSLnNldEF0dHJpYnV0ZSgneScsKC1vZmZ5KS50b1N0cmluZygpKTsNCiAgICAgICAgICAgIHBvcHVwUi5zZXRBdHRyaWJ1dGUoJ2ZpbGwnLCd0cmFuc3BhcmVudCcpOw0KICAgICAgICAgICAgcG9wdXAuYXBwZW5kQ2hpbGQocG9wdXBSKTsNCiAgICAgICAgfQ0KICAgIH0NCiAgICBwb3B1cC5zZXRBdHRyaWJ1dGUoJ3RyYW5zZm9ybScsdHJzdHIpOw0KICAgIHBvcHVwLnNldEF0dHJpYnV0ZSgnY29tbWVudCcsJycpOw0KICAgIHBvcHVwLnN0eWxlLmRpc3BsYXk9J25vbmUnOw0KICAgIHBvcHVwLnNldEF0dHJpYnV0ZSgnZWQ6Y29tbWVudGlkJyxjb21tZW50c1tpXS5wYXJlbnROb2RlLmlkKTsNCiAgICBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCcjc3ZnLWNvbnRhaW5lciA+IHN2ZycpLmFwcGVuZENoaWxkKHBvcHVwKTsNCiAgICBjb21tZW50c1tpXS5vbm1vdXNlb3Zlcj1mdW5jdGlvbiAoKSB7DQogICAgICAgIHZhciBjb21tZW50aWQ9dGhpcy5wYXJlbnROb2RlLmlkOw0KICAgICAgICB0aGlzLnF1ZXJ5U2VsZWN0b3IoJ3JlY3QnKS5zdHlsZS5kaXNwbGF5PSdibG9jayc7DQogICAgICAgIGRvY3VtZW50LnF1ZXJ5U2VsZWN0b3IoImdbZWRcXDpjb21tZW50aWQ9JyIrY29tbWVudGlkKyInXVtjb21tZW50XSIpLnN0eWxlLmRpc3BsYXk9J2Jsb2NrJzsNCiAgICB9Ow0KICAgIGNvbW1lbnRzW2ldLm9ubW91c2VvdXQ9ZnVuY3Rpb24gKCkgew0KICAgICAgICB2YXIgY29tbWVudGlkPXRoaXMucGFyZW50Tm9kZS5pZDsNCi8vICAgICAgICB3aW5kb3cuZ2V0U2VsZWN0aW9uKCkucmVtb3ZlQWxsUmFuZ2VzKCk7DQogICAgICAgIHRoaXMucXVlcnlTZWxlY3RvcigncmVjdCcpLnN0eWxlLmRpc3BsYXk9J25vbmUnOw0KICAgICAgICBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCJnW2VkXFw6Y29tbWVudGlkPSciK2NvbW1lbnRpZCsiJ11bY29tbWVudF0iKS5zdHlsZS5kaXNwbGF5PSdub25lJzsNCiAgICB9DQp9DQovLy0tY29tbWVudA0KLy9ub3RlLS0NCmlmKCF1YSl7DQogICAgdmFyIG5vdGVzPWRvY3VtZW50LnF1ZXJ5U2VsZWN0b3JBbGwoJ2c+Z1tlZFxcOm5vdGVdJyk7DQogICAgZnVuY3Rpb24gZ2V0d2gocyxwKSB7DQogICAgICAgIHZhciBtYWlucD1kb2N1bWVudC5jcmVhdGVFbGVtZW50KCdkaXYnKTsNCiAgICAgICAgbWFpbnAuc3R5bGUuY3NzVGV4dD1zOw0KICAgICAgICBtYWlucC5zdHlsZS5kaXNwbGF5PSdpbmxpbmUtYmxvY2snOw0KCQltYWlucC5zdHlsZS5tYXhXaWR0aCA9ICc0MDBweCc7DQoJCW1haW5wLnN0eWxlLndvcmRCcmVhayA9ICdicmVhay1hbGwnOw0KICAgICAgICBtYWlucC5pbm5lckhUTUw9cDsNCiAgICAgICAgZG9jdW1lbnQuYm9keS5hcHBlbmRDaGlsZChtYWlucCk7DQogICAgICAgIHZhciB3PW1haW5wLmNsaWVudFdpZHRoOw0KICAgICAgICB2YXIgaD1tYWlucC5jbGllbnRIZWlnaHQ7DQogICAgICAgIGRvY3VtZW50LmJvZHkucmVtb3ZlQ2hpbGQobWFpbnApOw0KICAgICAgICByZXR1cm4gW3csaF0NCiAgICB9DQogICAgZm9yKHZhciBpPTA7aTxub3Rlcy5sZW5ndGg7aSsrKXsNCiAgICAgICAgdmFyIGE9bm90ZXNbaV0uZ2V0QXR0cmlidXRlKCdlZDpub3RlJyk7DQoJCXZhciBub3RlTG9jayA9IG5vdGVzW2ldLmdldEF0dHJpYnV0ZSgnZWQ6bm90ZWxvY2snKTsNCiAgICAgICAgaWYobm90ZUxvY2sgPT0gJ3RydWUnKXsNCiAgICAgICAgICAgIGNvbnRpbnVlOw0KICAgICAgICB9DQogICAgICAgIHZhciBtYWlucD1hLm1hdGNoKC88Ym9keVtePl0qPiguKik8XC9ib2R5Pi8pWzFdOw0KICAgICAgICB2YXIgbWFpbnM9YS5tYXRjaCgvc3R5bGU9IiguKj8pIi8pWzFdOw0KICAgICAgICB2YXIgb3V0PWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdnJyk7DQogICAgICAgIHZhciBvbGluZT1kb2N1bWVudC5jcmVhdGVFbGVtZW50TlMoJ2h0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnJywncmVjdCcpOw0KICAgICAgICB2YXIgcG9wdXA9ZG9jdW1lbnQuY3JlYXRlRWxlbWVudE5TKCdodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZycsJ2ZvcmVpZ25PYmplY3QnKTsNCiAgICAgICAgdmFyIHBvcHVwUj0gZG9jdW1lbnQuY3JlYXRlRWxlbWVudE5TKCdodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZycsJ3JlY3QnKTsNCiAgICAgICAgdmFyIGhvdmVyPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdyZWN0Jyk7DQogICAgICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgnZmlsbCcsJyNjZGNkZmYnKTsNCiAgICAgICAgaG92ZXIuc2V0QXR0cmlidXRlKCd4JywnMCcpOw0KICAgICAgICBob3Zlci5zZXRBdHRyaWJ1dGUoJ3knLCcwJyk7DQogICAgICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgnaGVpZ2h0JywnMTYnKTsNCiAgICAgICAgaG92ZXIuc2V0QXR0cmlidXRlKCd3aWR0aCcsJzE2Jyk7DQogICAgICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgnZmlsbC1vcGFjaXR5JywnMC42Jyk7DQogICAgICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgndHJhbnNmb3JtJyxub3Rlc1tpXS5xdWVyeVNlbGVjdG9yKCd1c2UnKS5nZXRBdHRyaWJ1dGUoJ3RyYW5zZm9ybScpKTsNCiAgICAgICAgaG92ZXIuc3R5bGUuZGlzcGxheT0nbm9uZSc7DQogICAgICAgIG5vdGVzW2ldLmFwcGVuZENoaWxkKGhvdmVyKTsNCiAgICAgICAgcG9wdXAuc3R5bGUuY3NzVGV4dD1tYWluczsNCiAgICAgICAgcG9wdXAuaW5uZXJIVE1MPW1haW5wOw0KICAgICAgICB2YXIgd2g9Z2V0d2gobWFpbnMsbWFpbnApOw0KICAgICAgICBwb3B1cC5zZXRBdHRyaWJ1dGUoJ3dpZHRoJyx3aFswXSk7DQogICAgICAgIHBvcHVwLnNldEF0dHJpYnV0ZSgnaGVpZ2h0Jyx3aFsxXSk7DQogICAgICAgIHBvcHVwLnNldEF0dHJpYnV0ZSgndHJhbnNmb3JtJywndHJhbnNsYXRlKDgsNCknKTsNCgkJcG9wdXAuc3R5bGUud29yZEJyZWFrID0gJ2JyZWFrLWFsbCc7DQogICAgICAgIHBvcHVwLnN0eWxlLnRleHRBbGlnbj0nbGVmdCc7DQogICAgICAgIG9saW5lLnNldEF0dHJpYnV0ZSgneCcsJzAnKTsNCiAgICAgICAgb2xpbmUuc2V0QXR0cmlidXRlKCd5JywnMCcpOw0KICAgICAgICBvbGluZS5zZXRBdHRyaWJ1dGUoJ3dpZHRoJyx3aFswXSsxNik7DQogICAgICAgIG9saW5lLnNldEF0dHJpYnV0ZSgnaGVpZ2h0Jyx3aFsxXSs4KTsNCiAgICAgICAgb2xpbmUuc2V0QXR0cmlidXRlKCdzdHJva2UnLCcjYTI3YTAwJyk7DQogICAgICAgIG9saW5lLnNldEF0dHJpYnV0ZSgnZmlsbCcsJyNmZmU3OWQnKTsNCiAgICAgICAgb3V0LmFwcGVuZENoaWxkKG9saW5lKTsNCiAgICAgICAgb3V0LmFwcGVuZENoaWxkKHBvcHVwKTsNCiAgICAgICAgb3V0LnNldEF0dHJpYnV0ZSgnbm90ZScsJycpOw0KICAgICAgICBvdXQuc3R5bGUuZGlzcGxheT0nbm9uZSc7DQogICAgICAgIG91dC5zZXRBdHRyaWJ1dGUoJ2VkOm5vdGVpZCcsbm90ZXNbaV0ucGFyZW50Tm9kZS5pZCk7DQogICAgICAgIG91dC5vbm1vdXNlb3Zlcj1mdW5jdGlvbiAoKSB7DQogICAgICAgICAgICB0aGlzLnN0eWxlLmRpc3BsYXk9J2Jsb2NrJzsNCiAgICAgICAgfTsNCiAgICAgICAgb3V0Lm9ubW91c2VvdXQ9ZnVuY3Rpb24gKCkgew0KLy8gICAgICAgIHdpbmRvdy5nZXRTZWxlY3Rpb24gPyB3aW5kb3cuZ2V0U2VsZWN0aW9uKCkucmVtb3ZlUmFuZ2Uod2luZG93LmdldFNlbGVjdGlvbigpLnJlKTpkb2N1bWVudC5zZWxlY3Rpb24uZW1wdHkoKTsNCg0KICAgICAgICAgICAgdGhpcy5zdHlsZS5kaXNwbGF5PSdub25lJzsNCiAgICAgICAgfTsNCiAgICAgICAgdmFyIGNzPW5vdGVzW2ldLnF1ZXJ5U2VsZWN0b3IoJ3VzZScpLmdldEF0dHJpYnV0ZSgndHJhbnNmb3JtJykubWF0Y2goL1woKFxTKnxcUypcc1xTKilcKS8pWzFdLnNwbGl0KC8gfCwvKTsNCiAgICAgICAgdmFyIHBzPW5vdGVzW2ldLnBhcmVudE5vZGUuZ2V0QXR0cmlidXRlKCd0cmFuc2Zvcm0nKTsNCiAgICAgICAgaWYocHMuc3Vic3RyKDAsMikgPT0gJ3RyJyl7DQogICAgICAgICAgICB2YXIgcHBzID0gcHMubWF0Y2goL1woKFxTKnxcUypcc1xTKilcKS8pWzFdLnNwbGl0KC8gfCwvKTsNCiAgICAgICAgICAgIHZhciB4PXBhcnNlRmxvYXQoY3NbMF0pK3BhcnNlRmxvYXQocHBzWzBdKTsNCiAgICAgICAgICAgIHZhciB5PXBhcnNlRmxvYXQocHBzWzFdKTsNCiAgICAgICAgICAgIHg9eC50b3N1aXRzdmcoKTsNCiAgICAgICAgICAgIHk9eS50b3N1aXRzdmcoKTsNCiAgICAgICAgICAgIHZhciB0cnN0ciA9ICd0cmFuc2xhdGUoJyt4KycsJyt5KycpJzsNCiAgICAgICAgfWVsc2UgaWYocHMuc3Vic3RyKDAsMikgPT0gJ21hJyl7DQogICAgICAgICAgICB2YXIgcHBzID0gcHMubWF0Y2goLyhcLT9cZCsoXC5cZCspPylbXCwgXShcLT9cZCsoXC5cZCspPylbXCwgXShcLT9cZCsoXC5cZCspPylbXCwgXShcLT9cZCsoXC5cZCspPylbXCwgXShcLT9cZCsoXC5cZCspPylbXCwgXShcLT9cZCsoXC5cZCspPylcKSQvKTsNCiAgICAgICAgICAgIHZhciBtYUFyciA9IFtwYXJzZUZsb2F0KHBwc1sxXSkscGFyc2VGbG9hdChwcHNbM10pLHBhcnNlRmxvYXQocHBzWzVdKSxwYXJzZUZsb2F0KHBwc1s3XSkscGFyc2VGbG9hdChwcHNbOV0pLHBhcnNlRmxvYXQocHBzWzExXSldOw0KICAgICAgICAgICAgaWYobWFBcnJbMV0gPT0gMCl7DQogICAgICAgICAgICAgICAgdmFyIHggPSBwYXJzZUZsb2F0KGNzWzBdKTsNCiAgICAgICAgICAgICAgICB2YXIgeSA9IHBhcnNlRmxvYXQoY3NbMV0pKzE2Ow0KICAgICAgICAgICAgICAgIHZhciB4MSA9IHggKiBtYUFyclswXSArIHkgKiBtYUFyclsyXSArIG1hQXJyWzRdOw0KICAgICAgICAgICAgICAgIHZhciB5MSA9IHggKiBtYUFyclsxXSArIHkgKiBtYUFyclszXSArIG1hQXJyWzVdOw0KICAgICAgICAgICAgICAgIHgxPXgxLnRvc3VpdHN2ZygpOw0KICAgICAgICAgICAgICAgIHkxPXkxLnRvc3VpdHN2ZygpOw0KICAgICAgICAgICAgICAgIHZhciB0cnN0ciA9ICAndHJhbnNsYXRlKCcreDErJywnK3kxKycpJzsNCiAgICAgICAgICAgIH1lbHNlew0KICAgICAgICAgICAgICAgIHZhciB4ID0gcGFyc2VGbG9hdChjc1swXSkrMTY7DQogICAgICAgICAgICAgICAgdmFyIHkgPSBwYXJzZUZsb2F0KGNzWzFdKSsxNjsNCiAgICAgICAgICAgICAgICB2YXIgeDEgPSB4ICogbWFBcnJbMF0gKyB5ICogbWFBcnJbMl0gKyBtYUFycls0XTsNCiAgICAgICAgICAgICAgICB2YXIgeTEgPSB4ICogbWFBcnJbMV0gKyB5ICogbWFBcnJbM10gKyBtYUFycls1XTsNCiAgICAgICAgICAgICAgICB4ID0gcGFyc2VGbG9hdChjc1swXSkrMTY7DQogICAgICAgICAgICAgICAgeSA9IHBhcnNlRmxvYXQoY3NbMV0pOw0KICAgICAgICAgICAgICAgIHZhciB4MiA9IHggKiBtYUFyclswXSArIHkgKiBtYUFyclsyXSArIG1hQXJyWzRdOw0KICAgICAgICAgICAgICAgIHZhciB5MiA9IHggKiBtYUFyclsxXSArIHkgKiBtYUFyclszXSArIG1hQXJyWzVdOw0KICAgICAgICAgICAgICAgIHZhciBmeCA9IHgxPHgyP3gxLnRvc3VpdHN2ZygpOiB4Mi50b3N1aXRzdmcoKTsNCiAgICAgICAgICAgICAgICB2YXIgZnkgPSB5MT55Mj95MS50b3N1aXRzdmcoKTogeTIudG9zdWl0c3ZnKCk7DQoJCQkJdmFyIG9mZnkgPSBNYXRoLmFicyh5MS15Mik7CQkJCQkJCQkJCSAgDQogICAgICAgICAgICAgICAgdmFyIHRyc3RyID0gICd0cmFuc2xhdGUoJytmeCsnLCcrZnkrJyknOw0KICAgICAgICAgICAgICAgIHBvcHVwUi5zZXRBdHRyaWJ1dGUoJ2hlaWdodCcsb2ZmeS50b1N0cmluZygpKTsNCiAgICAgICAgICAgICAgICBwb3B1cFIuc2V0QXR0cmlidXRlKCd3aWR0aCcsJzE2Jyk7DQogICAgICAgICAgICAgICAgcG9wdXBSLnNldEF0dHJpYnV0ZSgneScsKC1vZmZ5KS50b1N0cmluZygpKTsNCiAgICAgICAgICAgICAgICBwb3B1cFIuc2V0QXR0cmlidXRlKCdmaWxsJywndHJhbnNwYXJlbnQnKTsNCiAgICAgICAgICAgICAgICBwb3B1cC5hcHBlbmRDaGlsZChwb3B1cFIpOw0KICAgICAgICAgICAgfQ0KICAgICAgICB9DQogICAgICAgIG91dC5zZXRBdHRyaWJ1dGUoJ3RyYW5zZm9ybScsdHJzdHIpOw0KICAgICAgICBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCcjc3ZnLWNvbnRhaW5lciA+IHN2ZycpLmFwcGVuZENoaWxkKG91dCk7DQogICAgICAgIG5vdGVzW2ldLm9ubW91c2VvdmVyPWZ1bmN0aW9uICgpIHsNCiAgICAgICAgICAgIHZhciBub3RlaWQ9dGhpcy5wYXJlbnROb2RlLmlkOw0KICAgICAgICAgICAgdGhpcy5xdWVyeVNlbGVjdG9yKCdyZWN0Jykuc3R5bGUuZGlzcGxheT0nYmxvY2snOw0KICAgICAgICAgICAgZG9jdW1lbnQucXVlcnlTZWxlY3RvcigiZ1tlZFxcOm5vdGVpZD0nIitub3RlaWQrIiddW25vdGVdIikuc3R5bGUuZGlzcGxheT0nYmxvY2snOw0KICAgICAgICB9Ow0KICAgICAgICBub3Rlc1tpXS5vbm1vdXNlb3V0PWZ1bmN0aW9uICgpIHsNCiAgICAgICAgICAgIHZhciBub3RlaWQ9dGhpcy5wYXJlbnROb2RlLmlkOw0KLy8gICAgICAgIHdpbmRvdy5nZXRTZWxlY3Rpb24oKS5yZW1vdmVBbGxSYW5nZXMoKTsNCiAgICAgICAgICAgIHRoaXMucXVlcnlTZWxlY3RvcigncmVjdCcpLnN0eWxlLmRpc3BsYXk9J25vbmUnOw0KICAgICAgICAgICAgZG9jdW1lbnQucXVlcnlTZWxlY3RvcigiZ1tlZFxcOm5vdGVpZD0nIitub3RlaWQrIiddW25vdGVdIikuc3R5bGUuZGlzcGxheT0nbm9uZSc7DQogICAgICAgIH0NCiAgICB9DQp9ZWxzZXsNCiAgICBjb25zb2xlLmxvZygn5oqx5q2J77yMSUXmtY/op4jlmajkuI3mlK/mjIFub3Rl6Kej5p6Q77yM6K+35L2/55So5YW25LuW5YaF5qC45rWP6KeI5Zmo44CC6LCi6LCi77yBJykNCn0NCi8vLS1ub3RlDQovL2h5cGVybGluay0tDQp2YXIgbGlua3M9ZG9jdW1lbnQucXVlcnlTZWxlY3RvckFsbCgnZz5nW2VkXFw6aHlwZXJsaW5rXScpOw0KZnVuY3Rpb24gZ2V0bWF4bGVuKGFycixicnIpIHsNCiAgICB2YXIgbD0wOw0KICAgIHZhciBsbD0wOw0KICAgIGZvcih2YXIgaj0wO2o8YXJyLmxlbmd0aDtqKyspew0KICAgICAgICB2YXIgZT1kb2N1bWVudC5jcmVhdGVFbGVtZW50TlMoJ2h0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnJywndGV4dCcpOw0KICAgICAgICBpZighaXNOYU4obGlua2FycltqXSkpew0KICAgICAgICAgICAgZS50ZXh0Q29udGVudD0nUGFnZS0nK2FycltqXTsNCiAgICAgICAgfWVsc2V7DQogICAgICAgICAgICBlLnRleHRDb250ZW50PWFycltqXTsNCiAgICAgICAgfQ0KICAgICAgICBlLnN0eWxlLmZvbnRTaXplPScxMnB4JzsNCiAgICAgICAgZG9jdW1lbnQuYm9keS5nZXRFbGVtZW50c0J5VGFnTmFtZSgnc3ZnJylbMF0uYXBwZW5kQ2hpbGQoZSk7DQogICAgICAgIHZhciBldz1lLmdldEJCb3goKS53aWR0aDsNCiAgICAgICAgZG9jdW1lbnQuYm9keS5nZXRFbGVtZW50c0J5VGFnTmFtZSgnc3ZnJylbMF0ucmVtb3ZlQ2hpbGQoZSk7DQogICAgICAgIHZhciBoPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCd0ZXh0Jyk7DQogICAgICAgIGgudGV4dENvbnRlbnQ9YnJyW2pdOw0KICAgICAgICBoLnN0eWxlLmZvbnRTaXplPScxMnB4JzsNCiAgICAgICAgaC5zdHlsZS5mb250V2VpZ2h0PSdib2xkJzsNCiAgICAgICAgZG9jdW1lbnQuYm9keS5nZXRFbGVtZW50c0J5VGFnTmFtZSgnc3ZnJylbMF0uYXBwZW5kQ2hpbGQoaCk7DQogICAgICAgIHZhciBodz1oLmdldEJCb3goKS53aWR0aDsNCiAgICAgICAgZG9jdW1lbnQuYm9keS5nZXRFbGVtZW50c0J5VGFnTmFtZSgnc3ZnJylbMF0ucmVtb3ZlQ2hpbGQoaCk7DQogICAgICAgIGw9ZXc+aHc/ZXc6aHc7DQogICAgICAgIGxsPWw+bGw/bDpsbDsNCiAgICB9DQogICAgcmV0dXJuIGxsOw0KfQ0KZm9yKHZhciBpPTA7aTxsaW5rcy5sZW5ndGg7aSsrKXsNCiAgICB2YXIgcG9wdXA9ZG9jdW1lbnQuY3JlYXRlRWxlbWVudE5TKCdodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZycsJ2cnKTsNCiAgICB2YXIgcG9wdXBSPSBkb2N1bWVudC5jcmVhdGVFbGVtZW50TlMoJ2h0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnJywncmVjdCcpOw0KICAgIHZhciBob3Zlcj1kb2N1bWVudC5jcmVhdGVFbGVtZW50TlMoJ2h0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnJywncmVjdCcpOw0KICAgIHZhciBkZXNjYXJyPVtdOw0KICAgIHZhciBsaW5rYXJyPVtdOw0KICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgnZmlsbCcsJyNjZGNkZmYnKTsNCiAgICBob3Zlci5zZXRBdHRyaWJ1dGUoJ3gnLCcwJyk7DQogICAgaG92ZXIuc2V0QXR0cmlidXRlKCd5JywnMCcpOw0KICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgnaGVpZ2h0JywnMTYnKTsNCiAgICBob3Zlci5zZXRBdHRyaWJ1dGUoJ3dpZHRoJywnMTYnKTsNCiAgICBob3Zlci5zZXRBdHRyaWJ1dGUoJ2ZpbGwtb3BhY2l0eScsJzAuNicpOw0KICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgndHJhbnNmb3JtJyxsaW5rc1tpXS5xdWVyeVNlbGVjdG9yKCd1c2UnKS5nZXRBdHRyaWJ1dGUoJ3RyYW5zZm9ybScpKTsNCiAgICBob3Zlci5zdHlsZS5kaXNwbGF5PSdub25lJzsNCiAgICBsaW5rc1tpXS5hcHBlbmRDaGlsZChob3Zlcik7DQogICAgLy8gY29uc29sZS5sb2cobGlua3NbaV0uZ2V0QXR0cmlidXRlKCdlZDpoeXBlcmxpbmsnKSk7DQogICAgdmFyIGE9SlNPTi5wYXJzZShsaW5rc1tpXS5nZXRBdHRyaWJ1dGUoJ2VkOmh5cGVybGluaycpKTsNCiAgICB2YXIgY3M9bGlua3NbaV0ucXVlcnlTZWxlY3RvcigndXNlJykuZ2V0QXR0cmlidXRlKCd0cmFuc2Zvcm0nKS5tYXRjaCgvXCgoXFMqfFxTKlxzXFMqKVwpLylbMV0uc3BsaXQoLyB8LC8pOw0KICAgIHZhciBwcz1saW5rc1tpXS5wYXJlbnROb2RlLmdldEF0dHJpYnV0ZSgndHJhbnNmb3JtJyk7DQogICAgaWYocHMuc3Vic3RyKDAsMikgPT0gJ3RyJyl7DQogICAgICAgIHZhciBwcHMgPSBwcy5tYXRjaCgvXCgoXFMqfFxTKlxzXFMqKVwpLylbMV0uc3BsaXQoLyB8LC8pOw0KICAgICAgICB2YXIgeD1wYXJzZUZsb2F0KGNzWzBdKStwYXJzZUZsb2F0KHBwc1swXSk7DQogICAgICAgIHZhciB5PXBhcnNlRmxvYXQocHBzWzFdKTsNCiAgICAgICAgeD14LnRvc3VpdHN2ZygpOw0KICAgICAgICB5PXkudG9zdWl0c3ZnKCk7DQogICAgICAgIHZhciB0cnN0ciA9ICd0cmFuc2xhdGUoJyt4KycsJyt5KycpJzsNCiAgICB9ZWxzZSBpZihwcy5zdWJzdHIoMCwyKSA9PSAnbWEnKXsNCiAgICAgICAgdmFyIHBwcyA9IHBzLm1hdGNoKC8oXC0/XGQrKFwuXGQrKT8pW1wsIF0oXC0/XGQrKFwuXGQrKT8pW1wsIF0oXC0/XGQrKFwuXGQrKT8pW1wsIF0oXC0/XGQrKFwuXGQrKT8pW1wsIF0oXC0/XGQrKFwuXGQrKT8pW1wsIF0oXC0/XGQrKFwuXGQrKT8pXCkkLyk7DQogICAgICAgIHZhciBtYUFyciA9IFtwYXJzZUZsb2F0KHBwc1sxXSkscGFyc2VGbG9hdChwcHNbM10pLHBhcnNlRmxvYXQocHBzWzVdKSxwYXJzZUZsb2F0KHBwc1s3XSkscGFyc2VGbG9hdChwcHNbOV0pLHBhcnNlRmxvYXQocHBzWzExXSldOw0KICAgICAgICBpZihtYUFyclsxXSA9PSAwKXsNCiAgICAgICAgICAgIHZhciB4ID0gcGFyc2VGbG9hdChjc1swXSk7DQogICAgICAgICAgICB2YXIgeSA9IHBhcnNlRmxvYXQoY3NbMV0pKzE2Ow0KICAgICAgICAgICAgdmFyIHgxID0geCAqIG1hQXJyWzBdICsgeSAqIG1hQXJyWzJdICsgbWFBcnJbNF07DQogICAgICAgICAgICB2YXIgeTEgPSB4ICogbWFBcnJbMV0gKyB5ICogbWFBcnJbM10gKyBtYUFycls1XTsNCiAgICAgICAgICAgIHgxPXgxLnRvc3VpdHN2ZygpOw0KICAgICAgICAgICAgeTE9eTEudG9zdWl0c3ZnKCk7DQogICAgICAgICAgICB2YXIgdHJzdHIgPSAgJ3RyYW5zbGF0ZSgnK3gxKycsJyt5MSsnKSc7DQogICAgICAgIH1lbHNlew0KICAgICAgICAgICAgdmFyIHggPSBwYXJzZUZsb2F0KGNzWzBdKSsxNjsNCiAgICAgICAgICAgIHZhciB5ID0gcGFyc2VGbG9hdChjc1sxXSkrMTY7DQogICAgICAgICAgICB2YXIgeDEgPSB4ICogbWFBcnJbMF0gKyB5ICogbWFBcnJbMl0gKyBtYUFycls0XTsNCiAgICAgICAgICAgIHZhciB5MSA9IHggKiBtYUFyclsxXSArIHkgKiBtYUFyclszXSArIG1hQXJyWzVdOw0KICAgICAgICAgICAgeCA9IHBhcnNlRmxvYXQoY3NbMF0pKzE2Ow0KICAgICAgICAgICAgeSA9IHBhcnNlRmxvYXQoY3NbMV0pOw0KICAgICAgICAgICAgdmFyIHgyID0geCAqIG1hQXJyWzBdICsgeSAqIG1hQXJyWzJdICsgbWFBcnJbNF07DQogICAgICAgICAgICB2YXIgeTIgPSB4ICogbWFBcnJbMV0gKyB5ICogbWFBcnJbM10gKyBtYUFycls1XTsNCiAgICAgICAgICAgIHZhciBmeCA9IHgxPHgyP3gxLnRvc3VpdHN2ZygpOiB4Mi50b3N1aXRzdmcoKTsNCiAgICAgICAgICAgIHZhciBmeSA9IHkxPnkyP3kxLnRvc3VpdHN2ZygpOiB5Mi50b3N1aXRzdmcoKTsNCgkJCXZhciBvZmZ5ID0gTWF0aC5hYnMoeTEteTIpOwkJCQkJCQkJCQkgIA0KICAgICAgICAgICAgdmFyIHRyc3RyID0gICd0cmFuc2xhdGUoJytmeCsnLCcrZnkrJyknOw0KICAgICAgICAgICAgcG9wdXBSLnNldEF0dHJpYnV0ZSgnaGVpZ2h0JyxvZmZ5LnRvU3RyaW5nKCkpOw0KICAgICAgICAgICAgcG9wdXBSLnNldEF0dHJpYnV0ZSgnd2lkdGgnLCcxNicpOw0KICAgICAgICAgICAgcG9wdXBSLnNldEF0dHJpYnV0ZSgneScsKC1vZmZ5KS50b1N0cmluZygpKTsNCiAgICAgICAgICAgIHBvcHVwUi5zZXRBdHRyaWJ1dGUoJ2ZpbGwnLCd0cmFuc3BhcmVudCcpOw0KICAgICAgICAgICAgcG9wdXAuYXBwZW5kQ2hpbGQocG9wdXBSKTsNCiAgICAgICAgfQ0KICAgIH0NCiAgICB2YXIgYWw9YS5sZW5ndGg7DQogICAgZm9yKHZhciBqPTA7ajxhbDtqKyspew0KICAgICAgICBsaW5rYXJyLnB1c2goYVtqXS5saW5rKTsNCiAgICAgICAgZGVzY2Fyci5wdXNoKGFbal0uZGVzYyk7DQogICAgfQ0KICAgIHBvcHVwLnNldEF0dHJpYnV0ZSgndHJhbnNmb3JtJyx0cnN0cik7DQogICAgdmFyIG1heD1nZXRtYXhsZW4obGlua2FycixkZXNjYXJyKTsNCiAgICBmb3IodmFyIGs9MDtrPGFsO2srKyl7DQogICAgICAgIHZhciBjPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdhJyk7DQogICAgICAgIHZhciBkPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdyZWN0Jyk7DQogICAgICAgIHZhciBlPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCd0ZXh0Jyk7DQogICAgICAgIHZhciBmPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCd0ZXh0Jyk7DQogICAgICAgIGlmKGlzTmFOKGxpbmthcnJba10pKXsNCiAgICAgICAgICAgIGMuc2V0QXR0cmlidXRlTlMoImh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiwgInhsaW5rIiwgImh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiKTsNCiAgICAgICAgICAgIGMuc2V0QXR0cmlidXRlTlMoImh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiLCAiaHJlZiIsIGxpbmthcnJba10pOw0KICAgICAgICAgICAgYy5zZXRBdHRyaWJ1dGUoJ3RhcmdldCcsJ19ibGFuaycpOw0KICAgICAgICAgICAgZS50ZXh0Q29udGVudD1saW5rYXJyW2tdOw0KICAgICAgICB9ZWxzZXsNCiAgICAgICAgICAgIGUudGV4dENvbnRlbnQ9J1BhZ2UtJytsaW5rYXJyW2tdOw0KICAgICAgICAgICAgYy5zZXRBdHRyaWJ1dGVOUygiaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciLCAieGxpbmsiLCAiaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayIpOw0KICAgICAgICAgICAgYy5zZXRBdHRyaWJ1dGVOUygiaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayIsICJocmVmIiwgIiMiK2xpbmthcnJba10pOw0KICAgICAgICB9DQogICAgICAgIGQuc2V0QXR0cmlidXRlKCd3aWR0aCcsbWF4KzEwKTsNCiAgICAgICAgZC5zZXRBdHRyaWJ1dGUoJ2hlaWdodCcsJzMzJyk7DQogICAgICAgIGQuc2V0QXR0cmlidXRlKCdzdHJva2UnLCcjOTk5OTk5Jyk7DQogICAgICAgIGQuc2V0QXR0cmlidXRlKCdmaWxsJywnd2hpdGUnKTsNCiAgICAgICAgZC5zZXRBdHRyaWJ1dGUoJ3knLDMzKmspOw0KICAgICAgICBmLnRleHRDb250ZW50PWRlc2NhcnJba107DQogICAgICAgIGYuc3R5bGUuZm9udFNpemU9JzEycHgnOw0KICAgICAgICBmLnN0eWxlLmZvbnRXZWlnaHQ9J2JvbGQnOw0KICAgICAgICBmLnNldEF0dHJpYnV0ZSgneCcsNSk7DQogICAgICAgIGYuc2V0QXR0cmlidXRlKCd5JywzMyprKzEyKTsNCiAgICAgICAgZS5zdHlsZS5mb250U2l6ZT0nMTJweCc7DQogICAgICAgIGUuc2V0QXR0cmlidXRlKCd5JywzMyprKzI4KTsNCiAgICAgICAgZS5zZXRBdHRyaWJ1dGUoJ3gnLDUpOw0KICAgICAgICBjLmFwcGVuZENoaWxkKGQpOw0KICAgICAgICBjLmFwcGVuZENoaWxkKGYpOw0KICAgICAgICBjLmFwcGVuZENoaWxkKGUpOw0KICAgICAgICBjLm9ubW91c2VvdmVyPWZ1bmN0aW9uICgpIHsNCiAgICAgICAgICAgIHRoaXMucXVlcnlTZWxlY3RvcigncmVjdCcpLnN0eWxlLmZpbGw9JyNlMWUxZmYnDQogICAgICAgIH07DQogICAgICAgIGMub25tb3VzZW91dD1mdW5jdGlvbiAoKSB7DQogICAgICAgICAgICB0aGlzLnF1ZXJ5U2VsZWN0b3IoJ3JlY3QnKS5zdHlsZS5maWxsPSd3aGl0ZScNCiAgICAgICAgfTsNCiAgICAgICAgcG9wdXAuYXBwZW5kQ2hpbGQoYyk7DQogICAgfQ0KICAgIHBvcHVwLnN0eWxlLmRpc3BsYXk9J25vbmUnOw0KICAgIHBvcHVwLnNldEF0dHJpYnV0ZSgnaHlwZXJsaW5rJywnJyk7DQogICAgcG9wdXAuc2V0QXR0cmlidXRlKCdlZDpsaW5raWQnLGxpbmtzW2ldLnBhcmVudE5vZGUuaWQpOw0KICAgIHBvcHVwLm9ubW91c2VvdmVyPWZ1bmN0aW9uICgpIHsNCiAgICAgICAgdGhpcy5zdHlsZS5kaXNwbGF5PSdibG9jayc7DQogICAgfTsNCiAgICBwb3B1cC5vbmNsaWNrPWZ1bmN0aW9uICgpIHsNCiAgICAgICAgdGhpcy5zdHlsZS5kaXNwbGF5PSdub25lJzsNCiAgICB9Ow0KICAgIHBvcHVwLm9ubW91c2VvdXQ9ZnVuY3Rpb24gKCkgew0KICAgICAgICB0aGlzLnN0eWxlLmRpc3BsYXk9J25vbmUnOw0KICAgIH07DQogICAgZG9jdW1lbnQucXVlcnlTZWxlY3RvcignI3N2Zy1jb250YWluZXIgPiBzdmcnKS5hcHBlbmRDaGlsZChwb3B1cCk7DQogICAgbGlua3NbaV0ub25tb3VzZW92ZXI9ZnVuY3Rpb24gKCkgew0KICAgICAgICB2YXIgbGlua2lkPXRoaXMucGFyZW50Tm9kZS5pZDsNCiAgICAgICAgdGhpcy5xdWVyeVNlbGVjdG9yKCdyZWN0Jykuc3R5bGUuZGlzcGxheT0nYmxvY2snOw0KICAgICAgICBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCJnW2VkXFw6bGlua2lkPSciK2xpbmtpZCsiJ11baHlwZXJsaW5rXSIpLnN0eWxlLmRpc3BsYXk9J2Jsb2NrJzsNCiAgICB9DQogICAgbGlua3NbaV0ub25tb3VzZW91dD1mdW5jdGlvbiAoKSB7DQogICAgICAgIHZhciBsaW5raWQ9dGhpcy5wYXJlbnROb2RlLmlkOw0KICAgICAgICB0aGlzLnF1ZXJ5U2VsZWN0b3IoJ3JlY3QnKS5zdHlsZS5kaXNwbGF5PSdub25lJzsNCiAgICAgICAgZG9jdW1lbnQucXVlcnlTZWxlY3RvcigiZ1tlZFxcOmxpbmtpZD0nIitsaW5raWQrIiddW2h5cGVybGlua10iKS5zdHlsZS5kaXNwbGF5PSdub25lJzsNCiAgICB9DQp9DQovLy0taHlwZXJsaW5rDQovL2luaXRpYWxpemUtLQ0KdmFyIHNoYXBlPWRvY3VtZW50LnF1ZXJ5U2VsZWN0b3JBbGwoJ2dbZWRcXDp0b2d0b3BpY2lkXScpOw0KdmFyIG1JZD1kb2N1bWVudC5xdWVyeVNlbGVjdG9yQWxsKCdnW2VkXFw6dG9waWN0eXBlXScpOw0KdmFyIGRhdGFUcmVlPXt9Ow0KdmFyIGV4dHJhUmVsYT17fTsNCnZhciBjaGVja0lEPScnOw0KZm9yKHZhciBpPTA7aTxtSWQubGVuZ3RoO2krKyl7DQogICAgdmFyIHR5cGU9bUlkW2ldLmdldEF0dHJpYnV0ZSgnZWQ6dG9waWN0eXBlJyk7DQogICAgdmFyIHNpZD1tSWRbaV0uaWQ7DQogICAgaWYodHlwZSE9PSdjYWxsb3V0Jyl7DQogICAgICAgIGluaXQoc2lkLGRhdGFUcmVlKQ0KICAgIH0NCn0NCmZ1bmN0aW9uIGluaXQoaWQsIG9iaikgew0KICAgIHZhciBjaGlsZHMgPSBkb2N1bWVudC5xdWVyeVNlbGVjdG9yQWxsKCJnW2VkXFw6cGFyZW50aWQ9JyIgKyBpZCArICInXTpub3QoW2VkXFw6dG9waWN0eXBlXSkiKTsNCiAgICB2YXIgY2FsbHMgPSBkb2N1bWVudC5xdWVyeVNlbGVjdG9yQWxsKCJnW2VkXFw6cGFyZW50aWQ9JyIgKyBpZCArICInXVtlZFxcOnRvcGljdHlwZV0iKTsNCiAgICB2YXIgc3VtbWFyeSA9IGRvY3VtZW50LnF1ZXJ5U2VsZWN0b3JBbGwoInBhdGhbZWRcXDpwYXJlbnRpZCo9JyIgKyBpZCArICInXVtlZFxcOnR5cGU9J3N1bW1hcnknXSIpOw0KICAgIHZhciBib3VuZGFyeT0gZG9jdW1lbnQucXVlcnlTZWxlY3RvckFsbCgicGF0aFtlZFxcOnBhcmVudGlkKj0nIiArIGlkICsgIiddW2VkXFw6dHlwZT0nYm91bmRhcnknXSIpOw0KICAgIHZhciByZWxhZnJvbT1kb2N1bWVudC5xdWVyeVNlbGVjdG9yQWxsKCJnW2VkXFw6ZnJvbWlkKj0nIiArIGlkICsgIiddW2VkXFw6dHlwZT0ncmVsYXRpb24nXSIpOw0KICAgIHZhciByZWxhdG89ZG9jdW1lbnQucXVlcnlTZWxlY3RvckFsbCgiZ1tlZFxcOnRvaWQqPSciICsgaWQgKyAiJ11bZWRcXDp0eXBlPSdyZWxhdGlvbiddIik7DQogICAgb2JqWyJtIiArIGlkXSA9IHt9Ow0KICAgIHZhciB0eXBlID0gZG9jdW1lbnQuZ2V0RWxlbWVudEJ5SWQoaWQpLmdldEF0dHJpYnV0ZSgnZWQ6dG9waWN0eXBlJyk7DQogICAgdmFyIGl3PWRvY3VtZW50LmdldEVsZW1lbnRCeUlkKGlkKS5nZXRBdHRyaWJ1dGUoJ2VkOndpZHRoJyk7DQogICAgdmFyIGloPWRvY3VtZW50LmdldEVsZW1lbnRCeUlkKGlkKS5nZXRBdHRyaWJ1dGUoJ2VkOmhlaWdodCcpOw0KICAgIGlmICh0eXBlKSB7DQogICAgICAgIG9ialsibSIgKyBpZF0udHlwZSA9IHR5cGU7DQogICAgfQ0KICAgIGlmKGl3JiZpaCl7DQogICAgICAgIG9ialsibSIgKyBpZF0ud2lkdGggPWl3Ow0KICAgICAgICBvYmpbIm0iICsgaWRdLmhlaWdodCA9aWg7DQogICAgfQ0KICAgIGlmIChyZWxhZnJvbS5sZW5ndGggIT09IDApIHsNCiAgICAgICAgb2JqWyJtIiArIGlkXS5yZWxhZnJvbSA9IHt9Ow0KICAgICAgICBmb3IgKHZhciBpID0gMDsgaSA8IHJlbGFmcm9tLmxlbmd0aDsgaSsrKSB7DQogICAgICAgICAgICB2YXIgaW5kZXhpZCA9IHJlbGFmcm9tW2ldLmlkOw0KICAgICAgICAgICAgdmFyIHRvaWQgPSBkb2N1bWVudC5nZXRFbGVtZW50QnlJZChpbmRleGlkKS5nZXRBdHRyaWJ1dGUoJ2VkOnRvaWQnKTsNCiAgICAgICAgICAgIGlmIChleHRyYVJlbGFbaW5kZXhpZF0gPT09IHVuZGVmaW5lZCkgew0KICAgICAgICAgICAgICAgIGV4dHJhUmVsYVtpbmRleGlkXSA9IHsNCiAgICAgICAgICAgICAgICAgICAgaWQ6IGluZGV4aWQsDQogICAgICAgICAgICAgICAgICAgIGZyb21pZDogaWQsDQogICAgICAgICAgICAgICAgICAgIHRvaWQ6IHRvaWQsDQogICAgICAgICAgICAgICAgICAgIGlzQzogZmFsc2UNCiAgICAgICAgICAgICAgICB9Ow0KICAgICAgICAgICAgfQ0KICAgICAgICAgICAgb2JqWyJtIiArIGlkXS5yZWxhZnJvbVtpbmRleGlkXT17fTsNCiAgICAgICAgICAgIG9ialsibSIgKyBpZF0ucmVsYWZyb20uZGlzcGxheT1kb2N1bWVudC5nZXRFbGVtZW50QnlJZChpZCkuc3R5bGUuZGlzcGxheSAhPT0gJ25vbmUnPydibG9jayc6J25vbmUnOw0KICAgICAgICB9DQogICAgfQ0KICAgIGlmIChyZWxhdG8ubGVuZ3RoICE9PSAwKSB7DQogICAgICAgIG9ialsibSIgKyBpZF0ucmVsYXRvID0ge307DQogICAgICAgIGZvciAodmFyIGkgPSAwOyBpIDwgcmVsYXRvLmxlbmd0aDsgaSsrKSB7DQogICAgICAgICAgICB2YXIgaW5kZXhpZD1yZWxhdG9baV0uaWQ7DQogICAgICAgICAgICB2YXIgZnJvbWlkPWRvY3VtZW50LmdldEVsZW1lbnRCeUlkKGluZGV4aWQpLmdldEF0dHJpYnV0ZSgnZWQ6ZnJvbWlkJyk7DQogICAgICAgICAgICBpZihleHRyYVJlbGFbaW5kZXhpZF0gPT09IHVuZGVmaW5lZCl7DQogICAgICAgICAgICAgICAgZXh0cmFSZWxhW2luZGV4aWRdPXsNCiAgICAgICAgICAgICAgICAgICAgaWQ6aW5kZXhpZCwNCiAgICAgICAgICAgICAgICAgICAgZnJvbWlkOmZyb21pZCwNCiAgICAgICAgICAgICAgICAgICAgdG9pZDppZCwNCiAgICAgICAgICAgICAgICAgICAgaXNDOmZhbHNlDQogICAgICAgICAgICAgICAgfTsNCiAgICAgICAgICAgIH0NCiAgICAgICAgICAgIG9ialsibSIgKyBpZF0ucmVsYXRvW2luZGV4aWRdPXt9Ow0KICAgICAgICAgICAgb2JqWyJtIiArIGlkXS5yZWxhdG8uZGlzcGxheT1kb2N1bWVudC5nZXRFbGVtZW50QnlJZChpZCkuc3R5bGUuZGlzcGxheSAhPT0gJ25vbmUnPydibG9jayc6J25vbmUnOw0KICAgICAgICB9DQogICAgfQ0KICAgIGlmIChjaGlsZHMubGVuZ3RoICE9PSAwKSB7DQogICAgICAgIG9ialsibSIgKyBpZF0uY2hpbGQgPSB7fTsNCiAgICAgICAgaWYgKGRvY3VtZW50LnF1ZXJ5U2VsZWN0b3IoImdbZWRcXDp0b2d0b3BpY2lkPSciICsgaWQgKyAiJ10iKSkgew0KICAgICAgICAgICAgLy8gY29uc29sZS5sb2coZG9jdW1lbnQucXVlcnlTZWxlY3RvcigiZ1tlZFxcOnRvZ3RvcGljaWQ9JyIgKyBpZCArICInXSIpLmNoaWxkTm9kZXNbMF0uZ2V0QXR0cmlidXRlKCd4bGluazpocmVmJykpOw0KICAgICAgICAgICAgdmFyIHRvZyA9IGRvY3VtZW50LnF1ZXJ5U2VsZWN0b3IoImdbZWRcXDp0b2d0b3BpY2lkPSciICsgaWQgKyAiJ10iKS5nZXRFbGVtZW50c0J5VGFnTmFtZSgndXNlJylbMF0uZ2V0QXR0cmlidXRlKCd4bGluazpocmVmJykuc2xpY2UoMSk7DQogICAgICAgICAgICBvYmpbIm0iICsgaWRdLnRvZ3R5cGUgPSB0b2c7DQogICAgICAgIH0NCiAgICAgICAgZm9yICh2YXIgaSA9IDA7IGkgPCBjaGlsZHMubGVuZ3RoOyBpKyspIHsNCiAgICAgICAgICAgIHZhciBjaWQgPSBjaGlsZHNbaV0uaWQ7DQogICAgICAgICAgICBpbml0KGNpZCwgb2JqWyJtIiArIGlkXS5jaGlsZCk7DQogICAgICAgIH0NCiAgICB9DQogICAgaWYgKGNhbGxzLmxlbmd0aCAhPT0gMCkgew0KICAgICAgICBvYmpbIm0iICsgaWRdLmNhbGwgPSB7fTsNCiAgICAgICAgZm9yICh2YXIgaSA9IDA7IGkgPCBjYWxscy5sZW5ndGg7IGkrKykgew0KICAgICAgICAgICAgdmFyIGNpZCA9IGNhbGxzW2ldLmlkOw0KICAgICAgICAgICAgaW5pdChjaWQsIG9ialsibSIgKyBpZF0uY2FsbCk7DQogICAgICAgIH0NCiAgICB9DQogICAgaWYgKGJvdW5kYXJ5Lmxlbmd0aCAhPT0gMCkgew0KICAgICAgICBvYmpbIm0iICsgaWRdLmJvdW5kYXJ5ID0ge307DQogICAgICAgIGZvciAodmFyIGkgPSAwOyBpIDwgYm91bmRhcnkubGVuZ3RoOyBpKyspIHsNCiAgICAgICAgICAgIHZhciBjaWQgPWJvdW5kYXJ5W2ldLmlkOw0KICAgICAgICAgICAgaW5pdChjaWQsIG9ialsibSIgKyBpZF0uYm91bmRhcnkpOw0KICAgICAgICB9DQogICAgfQ0KICAgIGlmIChzdW1tYXJ5Lmxlbmd0aCAhPT0gMCkgew0KICAgICAgICBvYmpbIm0iICsgaWRdLnN1bW1hcnkgPSB7fTsNCiAgICAgICAgZm9yICh2YXIgaSA9IDA7IGkgPCBzdW1tYXJ5Lmxlbmd0aDsgaSsrKSB7DQogICAgICAgICAgICB2YXIgY2lkID0gc3VtbWFyeVtpXS5pZDsNCiAgICAgICAgICAgIGluaXQoY2lkLCBvYmpbIm0iICsgaWRdLnN1bW1hcnkpOw0KICAgICAgICB9DQogICAgfQ0KfQ0KLy8tLWluaXRpYWxpemUNCi8vdG9nZ2xlZGlzcGxheS0tDQp2YXIgY2hhaW5BcnI9W107DQpmdW5jdGlvbiBnZXRjaGFpbihpZCl7DQogICAgY2hhaW5BcnIudW5zaGlmdCgnbScraWQpOw0KICAgIHZhciBwYXJlbnQ9ZG9jdW1lbnQuZ2V0RWxlbWVudEJ5SWQoaWQpLmdldEF0dHJpYnV0ZSgnZWQ6cGFyZW50aWQnKTsNCiAgICBpZighcGFyZW50KXsNCiAgICAgICAgcmV0dXJuOw0KICAgIH0NCglpZihwYXJlbnQubWF0Y2goL1wsLykpew0KICAgICAgICBwYXJlbnQgPSBwYXJlbnQubWF0Y2goL1xkKyg/PVwsKS8pWzBdDQogICAgfQ0KICAgIGdldGNoYWluKHBhcmVudCk7DQp9DQpmdW5jdGlvbiBnZXRvYmooaWQpIHsNCiAgICBjaGFpbkFycj1bXTsNCiAgICBnZXRjaGFpbihpZCk7DQogICAgdmFyIG1haW49Y2hhaW5BcnJbMF07DQogICAgaWYoY2hhaW5BcnIubGVuZ3RoPjEpew0KICAgICAgICB2YXIgb2JqPWRhdGFUcmVlW21haW5dOw0KICAgICAgICAvLyBjb25zb2xlLmxvZyhjaGFpbkFycik7DQogICAgICAgIGZvcih2YXIgaT0xO2k8Y2hhaW5BcnIubGVuZ3RoO2krKykgew0KICAgICAgICAgICAgdmFyIGEgPSBjaGFpbkFycltpXTsNCiAgICAgICAgICAgIGZvcih2YXIgaj0wO2o8T2JqZWN0LmtleXMob2JqKS5sZW5ndGg7aisrKXsNCiAgICAgICAgICAgICAgICB2YXIgY29iaj0gb2JqW09iamVjdC5rZXlzKG9iailbal1dW2FdOw0KICAgICAgICAgICAgICAgIGlmKGNvYmopew0KICAgICAgICAgICAgICAgICAgICBvYmo9Y29iajsNCiAgICAgICAgICAgICAgICAgICAgY29udGludWUNCiAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICB9DQogICAgICAgIH0NCiAgICAgICAgcmV0dXJuIG9iag0KICAgIH1lbHNlew0KICAgICAgICB2YXIgb2JqPWRhdGFUcmVlW21haW5dOw0KICAgICAgICByZXR1cm4gb2JqDQogICAgfQ0KDQp9DQpmb3IodmFyIGk9MDtpPHNoYXBlLmxlbmd0aDtpKyspew0KICAgIHNoYXBlW2ldLm9uY2xpY2s9ZnVuY3Rpb24gKCkgew0KICAgICAgICB2YXIgaWQ9TnVtYmVyKHRoaXMuZ2V0QXR0cmlidXRlKCdlZDp0b2d0b3BpY2lkJykpOw0KICAgICAgICB2YXIgb2JqPWdldG9iaihpZCk7DQoNCiAgICAgICAgdmFyIHR5cGU9b2JqLnRvZ3R5cGU9PT0nbWludXMnPydwbHVzJzonbWludXMnOw0KICAgICAgICB2YXIgZGlzcGxheT1vYmoudG9ndHlwZT09PSdtaW51cyc/J25vbmUnOidibG9jayc7DQogICAgICAgIHRoaXMuZ2V0RWxlbWVudHNCeVRhZ05hbWUoJ3VzZScpWzBdLnNldEF0dHJpYnV0ZSgneGxpbms6aHJlZicsJyMnK3R5cGUpOw0KICAgICAgICBvYmoudG9ndHlwZT10eXBlOw0KICAgICAgICBjaGVja0lEPW9iajsNCg0KICAgICAgICB1dGQob2JqLGlkLGRpc3BsYXkpOw0KICAgICAgICBleHRyYVJlbGFGaW4oKTsNCiAgICB9DQp9DQpmdW5jdGlvbiB1dGQob2JqLGlkLHNob3csb2MpIHsNCg0KICAgIHZhciBwc2hvdz1kb2N1bWVudC5nZXRFbGVtZW50QnlJZChpZCkuc3R5bGUuZGlzcGxheSE9PSAnbm9uZSc/J2Jsb2NrJzonbm9uZSc7DQogICAgaWYgKG9iai5yZWxhZnJvbSl7DQogICAgICAgIGlmKG9iai5yZWxhZnJvbS5kaXNwbGF5IT09IHBzaG93KXsNCiAgICAgICAgICAgIHZhciByZWxhZnJvbXM9T2JqZWN0LmtleXMob2JqLnJlbGFmcm9tKTsNCiAgICAgICAgICAgIHJlbGFmcm9tcy5zcGxpY2UocmVsYWZyb21zLmluZGV4T2YoJ2Rpc3BsYXknKSwxKTsNCiAgICAgICAgICAgIGZvcih2YXIgaz0wO2s8cmVsYWZyb21zLmxlbmd0aDtrKyspew0KICAgICAgICAgICAgICAgIHZhciBkPXJlbGFmcm9tc1trXTsNCiAgICAgICAgICAgICAgICBleHRyYVJlbGFbZF0uaXNDPXRydWU7DQogICAgICAgICAgICB9DQogICAgICAgICAgICBvYmoucmVsYWZyb20uZGlzcGxheSA9IHBzaG93Ow0KICAgICAgICB9DQogICAgfQ0KICAgIGlmIChvYmoucmVsYXRvKXsNCiAgICAgICAgaWYob2JqLnJlbGF0by5kaXNwbGF5IT09IHBzaG93KXsNCiAgICAgICAgICAgIHZhciByZWxhdG9zPU9iamVjdC5rZXlzKG9iai5yZWxhdG8pOw0KICAgICAgICAgICAgcmVsYXRvcy5zcGxpY2UocmVsYXRvcy5pbmRleE9mKCdkaXNwbGF5JyksMSk7DQogICAgICAgICAgICBmb3IodmFyIGs9MDtrPHJlbGF0b3MubGVuZ3RoO2srKyl7DQogICAgICAgICAgICAgICAgdmFyIGQ9cmVsYXRvc1trXTsNCiAgICAgICAgICAgICAgICBleHRyYVJlbGFbZF0uaXNDPXRydWU7DQogICAgICAgICAgICB9DQogICAgICAgICAgICBvYmoucmVsYXRvLmRpc3BsYXkgPSBwc2hvdzsNCiAgICAgICAgfQ0KICAgIH0NCiAgICBpZihvYmouY2FsbCl7DQogICAgICAgIHZhciBjYWxscz1PYmplY3Qua2V5cyhvYmouY2FsbCk7DQogICAgICAgIGlmKGNoZWNrSUQhPT1vYmopew0KICAgICAgICAgICAgZm9yKHZhciBpPTA7aSA8IGNhbGxzLmxlbmd0aDtpKyspew0KICAgICAgICAgICAgICAgIHZhciBhPWNhbGxzW2ldLnNsaWNlKDEpOw0KICAgICAgICAgICAgICAgIHZhciBiPW9iai5jYWxsW2NhbGxzW2ldXTsNCiAgICAgICAgICAgICAgICB2YXIgYz1iLnRvZ3R5cGU7DQogICAgICAgICAgICAgICAgZG9jdW1lbnQuZ2V0RWxlbWVudEJ5SWQoYSkuc3R5bGUuZGlzcGxheT1zaG93Ow0KICAgICAgICAgICAgICAgIGlmIChiLnJlbGFmcm9tJiYhYyl7DQogICAgICAgICAgICAgICAgICAgIGlmKGIucmVsYWZyb20uZGlzcGxheSE9PSBzaG93KXsNCiAgICAgICAgICAgICAgICAgICAgICAgIHZhciByZWxhZnJvbXM9T2JqZWN0LmtleXMoYi5yZWxhZnJvbSk7DQogICAgICAgICAgICAgICAgICAgICAgICByZWxhZnJvbXMuc3BsaWNlKHJlbGFmcm9tcy5pbmRleE9mKCdkaXNwbGF5JyksMSk7DQogICAgICAgICAgICAgICAgICAgICAgICBmb3IodmFyIGs9MDtrPHJlbGFmcm9tcy5sZW5ndGg7aysrKXsNCiAgICAgICAgICAgICAgICAgICAgICAgICAgICB2YXIgZD1yZWxhZnJvbXNba107DQogICAgICAgICAgICAgICAgICAgICAgICAgICAgZXh0cmFSZWxhW2RdLmlzQz10cnVlOw0KICAgICAgICAgICAgICAgICAgICAgICAgfQ0KICAgICAgICAgICAgICAgICAgICAgICAgYi5yZWxhZnJvbS5kaXNwbGF5ID0gc2hvdzsNCiAgICAgICAgICAgICAgICAgICAgfQ0KICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgICAgICBpZiAoYi5yZWxhdG8mJiFjKXsNCiAgICAgICAgICAgICAgICAgICAgaWYoYi5yZWxhdG8uZGlzcGxheSE9PSBzaG93KXsNCiAgICAgICAgICAgICAgICAgICAgICAgIHZhciByZWxhdG9zPU9iamVjdC5rZXlzKGIucmVsYXRvKTsNCiAgICAgICAgICAgICAgICAgICAgICAgIHJlbGF0b3Muc3BsaWNlKHJlbGF0b3MuaW5kZXhPZignZGlzcGxheScpLDEpOw0KICAgICAgICAgICAgICAgICAgICAgICAgZm9yKHZhciBrPTA7azxyZWxhdG9zLmxlbmd0aDtrKyspew0KICAgICAgICAgICAgICAgICAgICAgICAgICAgIHZhciBkPXJlbGF0b3Nba107DQogICAgICAgICAgICAgICAgICAgICAgICAgICAgZXh0cmFSZWxhW2RdLmlzQz10cnVlOw0KICAgICAgICAgICAgICAgICAgICAgICAgfQ0KICAgICAgICAgICAgICAgICAgICAgICAgYi5yZWxhdG8uZGlzcGxheSA9IHNob3c7DQogICAgICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgaWYoYyl7DQogICAgICAgICAgICAgICAgICAgIGRvY3VtZW50LnF1ZXJ5U2VsZWN0b3IoImdbZWRcXDp0b2d0b3BpY2lkPSciK2ErIiddIikuc3R5bGUuZGlzcGxheT1zaG93Ow0KICAgICAgICAgICAgICAgICAgICBpZihjPT09J21pbnVzJyl7DQogICAgICAgICAgICAgICAgICAgICAgICB1dGQoYixhLHNob3cpDQogICAgICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgICAgICAgICAgaWYgKChiLmNhbGx8fGIuYm91bmRhcnl8fGIuc3VtbWFyeSkmJmM9PT0ncGx1cycpIHsNCiAgICAgICAgICAgICAgICAgICAgICAgIHV0ZChiLGEsc2hvdyx0cnVlKQ0KICAgICAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgfQ0KICAgICAgICAgICAgICAgIGlmKGIuY2FsbCYmIWMpIHsNCiAgICAgICAgICAgICAgICAgICAgdXRkKGIsYSxzaG93LHRydWUpDQogICAgICAgICAgICAgICAgfQ0KICAgICAgICAgICAgICAgIGlmIChiLnN1bW1hcnkmJiFjKSB7DQogICAgICAgICAgICAgICAgICAgIHV0ZChiLGEsc2hvdykNCiAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgaWYgKGIuYm91bmRhcnkmJiFjKSB7DQogICAgICAgICAgICAgICAgICAgIHV0ZChiLGEsc2hvdykNCiAgICAgICAgICAgICAgICB9DQoNCiAgICAgICAgICAgIH0NCiAgICAgICAgfQ0KICAgIH0NCiAgICBpZihvYmouc3VtbWFyeSl7DQogICAgICAgIHZhciBzdW1tYXJ5cz1PYmplY3Qua2V5cyhvYmouc3VtbWFyeSk7DQogICAgICAgIGlmKChjaGVja0lEIT09b2JqJiYob2JqLnRvZ3R5cGU9PT0nbWludXMnfHwhb2JqLnRvZ3R5cGUpKXx8Y2hlY2tJRD09PW9iail7DQogICAgICAgICAgICBmb3IodmFyIGk9MDtpPHN1bW1hcnlzLmxlbmd0aDtpKyspew0KICAgICAgICAgICAgICAgIHZhciBhPXN1bW1hcnlzW2ldLnNsaWNlKDEpOw0KCQkJCXZhciBvc3AgPSBkb2N1bWVudC5nZXRFbGVtZW50QnlJZChhKS5nZXRBdHRyaWJ1dGUoJ2VkOnBhcmVudGlkJyk7DQogICAgICAgICAgICAgICAgaWYob3NwLm1hdGNoKC9cLC8pKXsNCiAgICAgICAgICAgICAgICAgICAgdmFyIG9zcGEgPSBvc3Auc3BsaXQoJywnKTsNCiAgICAgICAgICAgICAgICAgICAgdmFyIG9zcEw9MDsNCg0KICAgICAgICAgICAgICAgICAgICBmb3IodmFyIGo9MDtqPG9zcGEubGVuZ3RoO2orKyl7DQogICAgICAgICAgICAgICAgICAgICAgICBpZihzaG93ID09ICdub25lJyl7DQogICAgICAgICAgICAgICAgICAgICAgICAgICAgaWYoZG9jdW1lbnQuZ2V0RWxlbWVudEJ5SWQob3NwYVtqXSkuc3R5bGUuZGlzcGxheSAhPSAnbm9uZScpew0KICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBicmVhazsNCiAgICAgICAgICAgICAgICAgICAgICAgICAgICB9ZWxzZXsNCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgb3NwTCsrOw0KICAgICAgICAgICAgICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgICAgICAgICAgICAgIH1lbHNlew0KICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBpZihkb2N1bWVudC5nZXRFbGVtZW50QnlJZChhKS5zdHlsZS5kaXNwbGF5ICE9ICdub25lJyl7DQogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBicmVhazsNCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgfWVsc2V7DQogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBvc3BMKys7DQogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgICAgICAgICAgICAgIH0NCg0KICAgICAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgICAgIGlmKG9zcEwgIT09IG9zcGEubGVuZ3RoKXsNCiAgICAgICAgICAgICAgICAgICAgICAgIGNvbnRpbnVlOw0KICAgICAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgfQ0KICAgICAgICAgICAgICAgIHZhciBiPW9iai5zdW1tYXJ5W3N1bW1hcnlzW2ldXTsNCiAgICAgICAgICAgICAgICAvLyBjb25zb2xlLmxvZyhhKTsNCiAgICAgICAgICAgICAgICBkb2N1bWVudC5nZXRFbGVtZW50QnlJZChhKS5zdHlsZS5kaXNwbGF5PXNob3c7DQovLyAgICAgICAgICAgICAgICBpZihjKXsNCi8vICAgICAgICAgICAgICAgICAgICBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCJnW2VkXFw6dG9ndG9waWNpZD0nIithKyInXSIpLnN0eWxlLmRpc3BsYXk9c2hvdzsNCi8vICAgICAgICAgICAgICAgICAgICBpZihjPT09J21pbnVzJyl7DQovLyAgICAgICAgICAgICAgICAgICAgICAgIHV0ZChiLHNob3cpDQovLyAgICAgICAgICAgICAgICAgICAgfQ0KLy8gICAgICAgICAgICAgICAgICAgIGlmIChiLmNhbGwmJmM9PT0ncGx1cycpIHsNCi8vICAgICAgICAgICAgICAgICAgICAgICAgdXRkKGIsc2hvdyx0cnVlKQ0KLy8gICAgICAgICAgICAgICAgICAgIH0NCi8vICAgICAgICAgICAgICAgIH0NCi8vICAgICAgICAgICAgICAgIGlmKGIuY2FsbCYmIWMpIHsNCi8vICAgICAgICAgICAgICAgICAgICB1dGQoYixzaG93LHRydWUpDQovLyAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgaWYoT2JqZWN0LmtleXMoYikubGVuZ3RoIT09MCl7DQogICAgICAgICAgICAgICAgICAgIHV0ZChiLGEsc2hvdykNCiAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICB9DQogICAgICAgIH0NCiAgICB9DQogICAgaWYob2JqLmJvdW5kYXJ5KXsNCiAgICAgICAgdmFyIGJvdW5kYXJ5cz1PYmplY3Qua2V5cyhvYmouYm91bmRhcnkpOw0KICAgICAgICBpZihjaGVja0lEIT09b2JqKXsNCiAgICAgICAgICAgIGZvcih2YXIgaT0wO2k8Ym91bmRhcnlzLmxlbmd0aDtpKyspew0KICAgICAgICAgICAgICAgIHZhciBhPWJvdW5kYXJ5c1tpXS5zbGljZSgxKTsNCiAgICAgICAgICAgICAgICB2YXIgYj1vYmouYm91bmRhcnlbYm91bmRhcnlzW2ldXTsNCiAgICAgICAgICAgICAgICAvLyBjb25zb2xlLmxvZyhhKTsNCiAgICAgICAgICAgICAgICBkb2N1bWVudC5nZXRFbGVtZW50QnlJZChhKS5zdHlsZS5kaXNwbGF5PXNob3c7DQovLyAgICAgICAgICAgICAgICBpZihjKXsNCi8vICAgICAgICAgICAgICAgICAgICBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCJnW2VkXFw6dG9ndG9waWNpZD0nIithKyInXSIpLnN0eWxlLmRpc3BsYXk9c2hvdzsNCi8vICAgICAgICAgICAgICAgICAgICBpZihjPT09J21pbnVzJyl7DQovLyAgICAgICAgICAgICAgICAgICAgICAgIHV0ZChiLHNob3cpDQovLyAgICAgICAgICAgICAgICAgICAgfQ0KLy8gICAgICAgICAgICAgICAgICAgIGlmIChiLmNhbGwmJmM9PT0ncGx1cycpIHsNCi8vICAgICAgICAgICAgICAgICAgICAgICAgdXRkKGIsc2hvdyx0cnVlKQ0KLy8gICAgICAgICAgICAgICAgICAgIH0NCi8vICAgICAgICAgICAgICAgIH0NCi8vICAgICAgICAgICAgICAgIGlmKGIuY2FsbCYmIWMpIHsNCi8vICAgICAgICAgICAgICAgICAgICB1dGQoYixzaG93LHRydWUpDQovLyAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgaWYoT2JqZWN0LmtleXMoYikubGVuZ3RoIT09MCl7DQogICAgICAgICAgICAgICAgICAgIHV0ZChiLGEsc2hvdykNCiAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICB9DQogICAgICAgIH0NCiAgICB9DQogICAgaWYoIW9jJiZvYmouY2hpbGQpIHsNCiAgICAgICAgdmFyIGNoaWxkcyA9IE9iamVjdC5rZXlzKG9iai5jaGlsZCk7DQogICAgICAgIGZvciAodmFyIGkgPSAwOyBpIDwgY2hpbGRzLmxlbmd0aDsgaSsrKSB7DQogICAgICAgICAgICB2YXIgYSA9IGNoaWxkc1tpXS5zbGljZSgxKTsNCiAgICAgICAgICAgIHZhciBiID0gb2JqLmNoaWxkW2NoaWxkc1tpXV07DQogICAgICAgICAgICB2YXIgYyA9IGIudG9ndHlwZTsNCiAgICAgICAgICAgIGRvY3VtZW50LmdldEVsZW1lbnRCeUlkKGEpLnN0eWxlLmRpc3BsYXkgPSBzaG93Ow0KCQkJdmFyIHRTUGF0aCA9IGRvY3VtZW50LnF1ZXJ5U2VsZWN0b3IoInBhdGhbZWRcXDp0b3N1cGVyaWQ9JyIrYSsiJ10iKTsNCiAgICAgICAgICAgIGlmKHRTUGF0aCl7DQogICAgICAgICAgICAgICAgdFNQYXRoLnN0eWxlLmRpc3BsYXkgPSBzaG93Ow0KICAgICAgICAgICAgfQ0KCQkJdmFyIG5vdGVUaXAgPSBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCJnW2VkXFw6bm90ZXRvPSciK2ErIiddIik7DQogICAgICAgICAgICBpZihub3RlVGlwKXsNCiAgICAgICAgICAgICAgICBub3RlVGlwLnN0eWxlLmRpc3BsYXkgPSBzaG93Ow0KICAgICAgICAgICAgfQ0KICAgICAgICAgICAgaWYgKGIucmVsYWZyb20mJiFjKXsNCiAgICAgICAgICAgICAgICBpZihiLnJlbGFmcm9tLmRpc3BsYXkhPT0gc2hvdyl7DQogICAgICAgICAgICAgICAgICAgIHZhciByZWxhZnJvbXM9T2JqZWN0LmtleXMoYi5yZWxhZnJvbSk7DQogICAgICAgICAgICAgICAgICAgIHJlbGFmcm9tcy5zcGxpY2UocmVsYWZyb21zLmluZGV4T2YoJ2Rpc3BsYXknKSwxKTsNCiAgICAgICAgICAgICAgICAgICAgZm9yKHZhciBrPTA7azxyZWxhZnJvbXMubGVuZ3RoO2srKyl7DQogICAgICAgICAgICAgICAgICAgICAgICB2YXIgZD1yZWxhZnJvbXNba107DQogICAgICAgICAgICAgICAgICAgICAgICBleHRyYVJlbGFbZF0uaXNDPXRydWU7DQogICAgICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgICAgICAgICAgYi5yZWxhZnJvbS5kaXNwbGF5ID0gc2hvdzsNCiAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICB9DQogICAgICAgICAgICBpZiAoYi5yZWxhdG8mJiFjKXsNCiAgICAgICAgICAgICAgICBpZihiLnJlbGF0by5kaXNwbGF5IT09IHNob3cpew0KICAgICAgICAgICAgICAgICAgICB2YXIgcmVsYXRvcz1PYmplY3Qua2V5cyhiLnJlbGF0byk7DQogICAgICAgICAgICAgICAgICAgIHJlbGF0b3Muc3BsaWNlKHJlbGF0b3MuaW5kZXhPZignZGlzcGxheScpLDEpOw0KICAgICAgICAgICAgICAgICAgICBmb3IodmFyIGs9MDtrPHJlbGF0b3MubGVuZ3RoO2srKyl7DQogICAgICAgICAgICAgICAgICAgICAgICB2YXIgZD1yZWxhdG9zW2tdOw0KICAgICAgICAgICAgICAgICAgICAgICAgZXh0cmFSZWxhW2RdLmlzQz10cnVlOw0KICAgICAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgICAgIGIucmVsYXRvLmRpc3BsYXkgPSBzaG93Ow0KICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgIH0NCiAgICAgICAgICAgIGlmIChjKSB7DQogICAgICAgICAgICAgICAgZG9jdW1lbnQucXVlcnlTZWxlY3RvcigiZ1tlZFxcOnRvZ3RvcGljaWQ9JyIgKyBhICsgIiddIikuc3R5bGUuZGlzcGxheSA9IHNob3c7DQogICAgICAgICAgICAgICAgaWYgKGMgPT09ICdtaW51cycpIHsNCiAgICAgICAgICAgICAgICAgICAgdXRkKGIsYSxzaG93KQ0KICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgICAgICBpZiAoKGIuY2FsbHx8Yi5ib3VuZGFyeXx8Yi5zdW1tYXJ5KSYmYz09PSdwbHVzJykgew0KICAgICAgICAgICAgICAgICAgICB1dGQoYixhLHNob3csdHJ1ZSkNCiAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICB9DQogICAgICAgICAgICBpZiAoYi5jYWxsJiYhYykgew0KICAgICAgICAgICAgICAgIHV0ZChiLGEsc2hvdyx0cnVlKQ0KICAgICAgICAgICAgfQ0KICAgICAgICAgICAgaWYgKGIuc3VtbWFyeSYmIWMpIHsNCiAgICAgICAgICAgICAgICB1dGQoYixhLHNob3cpDQogICAgICAgICAgICB9DQogICAgICAgICAgICBpZiAoYi5ib3VuZGFyeSYmIWMpIHsNCiAgICAgICAgICAgICAgICB1dGQoYixhLHNob3cpDQogICAgICAgICAgICB9DQogICAgICAgIH0NCiAgICB9DQp9DQoNCmZ1bmN0aW9uIGV4dHJhUmVsYUZpbigpIHsNCiAgICB2YXIgZXh0cmFrZXlzPU9iamVjdC5rZXlzKGV4dHJhUmVsYSk7DQogICAgZm9yKHZhciBpPTA7aTxleHRyYWtleXMubGVuZ3RoO2krKyl7DQogICAgICAgIHZhciBleHRyYU9iaj1leHRyYVJlbGFbZXh0cmFrZXlzW2ldXTsNCiAgICAgICAgaWYoZXh0cmFPYmouaXNDID09PSB0cnVlKXsNCiAgICAgICAgICAgIHZhciBmc2hvdz1kb2N1bWVudC5nZXRFbGVtZW50QnlJZChleHRyYU9iai5mcm9taWQpLnN0eWxlLmRpc3BsYXkgIT09J25vbmUnPyB0cnVlOiBmYWxzZTsNCiAgICAgICAgICAgIHZhciB0c2hvdz1kb2N1bWVudC5nZXRFbGVtZW50QnlJZChleHRyYU9iai50b2lkKS5zdHlsZS5kaXNwbGF5ICE9PSdub25lJz8gdHJ1ZTogZmFsc2U7DQogICAgICAgICAgICBkb2N1bWVudC5nZXRFbGVtZW50QnlJZChleHRyYU9iai5pZCkuc3R5bGUuZGlzcGxheT1mc2hvdyAmJiB0c2hvdz8gJ2Jsb2NrJzogJ25vbmUnOw0KICAgICAgICAgICAgZXh0cmFSZWxhW2V4dHJha2V5c1tpXV0uaXNDID0gZmFsc2U7DQogICAgICAgIH0NCiAgICB9DQp9'))</script>
  </body>
</html>

