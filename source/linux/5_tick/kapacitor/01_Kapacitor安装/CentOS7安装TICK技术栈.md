---
title: 01-Kapacitor安装-CentOS 7 安装TICK技术栈
categories:
  - - 培训教程
    - TICK技术栈之kapacitor
tags:
  - 监控告警
  - TICK技术栈
  - TICK技术栈之kapacitor安装
date: 2020-04-14 11:28:42
---

<!-- MDTOC maxdepth:6 firsth1:1 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

   - [通过安装脚本一键安装](#通过安装脚本一键安装)   
   - [通过yum源手动安装](#通过yum源手动安装)   
   - [Kapacitor文件](#kapacitor文件)   
   - [进程和监听端口](#进程和监听端口)   

<!-- /MDTOC -->


|No.|功能|应用|服务器|
|:--|:--|:--|:--|
|1|数据采集|Telegraf|ECS|
|2|时序数据库|InfluxDB|ECS|
|3|监控展示|Chronograf|ECS|
|4|告警通知|kapacitor|ECS|

## 通过安装脚本一键安装


```bash
wget https://raw.githubusercontent.com/BoobooWei/booboo_TimeSeriesDBMS/master/scripts/auto_install_influxdb1.7.sh
bash auto_install_influxdb1.7.sh
wget https://raw.githubusercontent.com/BoobooWei/booboo_TimeSeriesDBMS/master/scripts/myinfluxdb1.7ctl.sh
bash myinfluxdb1.7ctl.sh all start
```


## 通过yum源手动安装

 ```bash
 # set repo
 cat > /etc/yum.repos.d/influxdb.repo << ENDF
 [influxdb]
 name = InfluxDB Repository - RHEL \$releasever
 baseurl = https://repos.influxdata.com/rhel/\$releasever/\$basearch/stable
 enabled = 1
 gpgcheck = 1
 gpgkey = https://repos.influxdata.com/influxdb.key
 ENDF

 yum install -y telegraf influxdb chronograf kapacitor
 chown telegraf. /var/log/telegraf -R
 ```

 ## Kapacitor文件

|文件|说明|
|:--|:--|
|`/etc/kapacitor/kapacitor.conf`|配置文件|
|`/etc/logrotate.d/kapacitor`|日志轮询配置|
|`/usr/bin/kapacitor`|客户端|
|`/usr/bin/tickfmt`|格式化TICKscript|
|`/usr/bin/kapacitord`|守护进程Daemon|
|`/usr/lib/kapacitor/scripts/kapacitor.service`|`Service`|
|`/var/lib/kapacitor`|数据目录|
|`/var/log/kapacitor`|日志目录|


## 进程和监听端口

默认监听端口`9092`

```bash
[root@node1 ~]# ps -ef|grep kapacitor
root       513   453  0 09:22 pts/0    00:00:00 grep --color=auto kapacitor
kapacit+  1024     1  0 8月15 ?       00:00:08 /usr/bin/kapacitord -config /etc/kapacitor/kapacitor.conf
[root@node1 ~]# ss -luntp|grep kapacitor
tcp    LISTEN     0      128                   :::9092                 :::*      users:(("kapacitord",1024,8))
```
