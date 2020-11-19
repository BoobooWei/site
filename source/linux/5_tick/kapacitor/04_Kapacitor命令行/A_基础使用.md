---
title: 04-Kapcitor命令行-A-基础命令
categories:
  - - 培训教程
    - TICK技术栈之kapacitor
tags:
  - 监控告警
  - TICK技术栈
  - Kapcitor命令行
date: 2020-04-14 11:30:00
---

<!-- MDTOC maxdepth:6 firsth1:1 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

- [kapacitor基础命令](#kapacitor基础命令)   
   - [基础命令](#基础命令)   
      - [课堂练习](#课堂练习)   
      - [课堂练习2](#课堂练习2)   

<!-- /MDTOC -->

# kapacitor基础命令

## 基础命令

```bash
kapacitor define cpu_alert -tick cpu_alert.tick
kapacitor list tasks
kapacitor enable cpu_alert
kapacitor show cpu_alert
kapacitor disable cpu_alert
kapacitor list tasks
```

### 课堂练习

```bash
[root@tick:/var/lib/kapacitor/task]# kapacitor define cpu_alert -tick cpu_alert.tick
[root@tick:/var/lib/kapacitor/task]# kapacitor list tasks
ID        Type      Status    Executing Databases and Retention Policies
cpu_alert stream    disabled  false     ["telegraf"."autogen"]
[root@tick:/var/lib/kapacitor/task]# kapacitor enable cpu_alert
[root@tick:/var/lib/kapacitor/task]# kapacitor list tasks
ID        Type      Status    Executing Databases and Retention Policies
cpu_alert stream    enabled   true      ["telegraf"."autogen"]
[root@tick:/var/lib/kapacitor/task]# kapacitor show cpu_alert
ID: cpu_alert
Error:
Template:
Type: stream
Status: enabled
Executing: true
Created: 06 Sep 19 14:17 CST
Modified: 06 Sep 19 14:18 CST
LastEnabled: 06 Sep 19 14:18 CST
Databases Retention Policies: ["telegraf"."autogen"]
TICKscript:
dbrp "telegraf"."autogen"

var data = stream
    |from()
        .database('telegraf')
        .retentionPolicy('autogen')
        .measurement('cpu')
        .groupBy('host')
        .where(lambda: "cpu" == 'cpu-total')
    |eval(lambda: 100.0 - "usage_idle")
        .as('used')
        .keep()

data
    |eval(lambda: float("used"))
        .as('float_value')
        .keep()
    |influxDBOut()
        .create()
        .database('out_booboo')
        .retentionPolicy('auto_gen')
        .measurement('booboo_t1_out')
        .tag('user', 'booboo')

DOT:
digraph cpu_alert {
graph [throughput="0.00 points/s"];

stream0 [avg_exec_time_ns="0s" errors="0" working_cardinality="0" ];
stream0 -> from1 [processed="0"];

from1 [avg_exec_time_ns="0s" errors="0" working_cardinality="0" ];
from1 -> eval2 [processed="0"];

eval2 [avg_exec_time_ns="0s" errors="0" working_cardinality="0" ];
eval2 -> eval3 [processed="0"];

eval3 [avg_exec_time_ns="0s" errors="0" working_cardinality="0" ];
eval3 -> influxdb_out4 [processed="0"];

influxdb_out4 [avg_exec_time_ns="0s" errors="0" points_written="0" working_cardinality="0" write_errors="0" ];
}

[root@tick:/var/lib/kapacitor/task]# zy_influx
Connected to http://localhost:8086 version 1.7.7
InfluxDB shell version: 1.7.7
> show databases
name: databases
name
----
telegraf
_internal
gitlab
chronograf
out_booboo
> use out_booboo
Using database out_booboo
> show measurements
name: measurements
name
----
booboo_t1_out
> select * from booboo_t1_out;
name: booboo_t1_out
time                cpu       float_value        host     usage_guest usage_guest_nice usage_idle        usage_iowait        usage_irq usage_nice usage_softirq        usage_steal usage_system       usage_user         used               user
----                ---       -----------        ----     ----------- ---------------- ----------        ------------        --------- ---------- -------------        ----------- ------------       ----------         ----               ----
1567750740000000000 cpu-total 2.618808569776064  influxdb 0           0                97.38119143022394 0.20917001338114363 0         0          0.008366800535238136 0           0.878514056191063  1.5227576974205699 2.618808569776064  booboo
1567750800000000000 cpu-total 2.2907783617255006 influxdb 0           0                97.7092216382745  0.20901262437006282 0         0          0.00836050497479491  0           0.9280160522035655 1.1453891815440511 2.2907783617255006 booboo


[root@tick:/var/lib/kapacitor/task]# kapacitor list tasks
ID        Type      Status    Executing Databases and Retention Policies
cpu_alert stream    disabled  false     ["telegraf"."autogen"]
```


### 课堂练习2

```bash
[root@tick:/var/lib/kapacitor/task]# kapacitor show  nginx_alert2
ID: nginx_alert2
Error:
Template:
Type: stream
Status: enabled
Executing: true
Created: 06 Sep 19 15:38 CST
Modified: 06 Sep 19 16:08 CST
LastEnabled: 06 Sep 19 16:08 CST
Databases Retention Policies: ["telegraf"."autogen"]
TICKscript:
dbrp "telegraf"."autogen"

var db = 'telegraf'

var rp = 'autogen'

var measurement = 'nginx'

var groupBy = ['host', 'server', 'port']

var name = '主机CPU使用率大于10%'

var idVar = name

var message = '{\'alert\': \'主机 {{ index .Tags }} Nginx2\',\'description\': \'主机 {{ index .Tags "host" }} CPU 使用率 {{ index .Fields "value" }}% ，CPU 使用率过高会导致系统运行缓慢，应用出现异常等问题\',\'suggestion\': \'请查看CPU使用率高的进程/应用是否为异常导致\'}'

var idTag = 'alertID'

var levelTag = 'level'

var messageField = 'message'

var durationField = 'duration'

var outputDB = 'chronograf'

var outputRP = 'autogen'

var outputMeasurement = 'alerts'

var triggerType = 'threshold'

var crit = 0

var data = stream
    |from()
        .database(db)
        .retentionPolicy(rp)
        .measurement(measurement)
        .groupBy(groupBy)
    |eval(lambda: "active")
        .as('active_value')

var datav = stream
    |from()
        .database(db)
        .retentionPolicy(rp)
        .measurement(measurement)
        .groupBy(groupBy)
    |eval(lambda: "accepts")
        .as('accepts_value')

var new = data
    |join(datav)
        .as('active', 'accepts')
    |eval(lambda: int("active.active_value") + int("accepts.accepts_value"))
        .as('new_value')

var trigger = new
    |alert()
        .crit(lambda: "new_value" > crit)
        .message(message)
        .id(idVar)
        .idTag(idTag)
        .levelTag(levelTag)
        .messageField(messageField)
        .durationField(durationField)
        .victorOps()

trigger
    |eval(lambda: float("new_value"))
        .as('value')
        .keep()
    |influxDBOut()
        .create()
        .database(outputDB)
        .retentionPolicy(outputRP)
        .measurement(outputMeasurement)

DOT:
digraph nginx_alert2 {
graph [throughput="0.00 points/s"];

stream0 [avg_exec_time_ns="0s" errors="0" working_cardinality="0" ];
stream0 -> from3 [processed="1"];
stream0 -> from1 [processed="1"];

from3 [avg_exec_time_ns="0s" errors="0" working_cardinality="0" ];
from3 -> eval4 [processed="1"];

eval4 [avg_exec_time_ns="0s" errors="0" working_cardinality="1" ];
eval4 -> join6 [processed="1"];

from1 [avg_exec_time_ns="0s" errors="0" working_cardinality="0" ];
from1 -> eval2 [processed="1"];

eval2 [avg_exec_time_ns="0s" errors="0" working_cardinality="1" ];
eval2 -> join6 [processed="1"];

join6 [avg_exec_time_ns="27.314µs" errors="0" working_cardinality="1" ];
join6 -> eval7 [processed="1"];

eval7 [avg_exec_time_ns="0s" errors="0" working_cardinality="1" ];
eval7 -> alert8 [processed="1"];

alert8 [alerts_inhibited="0" alerts_triggered="1" avg_exec_time_ns="8.445753ms" crits_triggered="1" errors="0" infos_triggered="0" oks_triggered="0" warns_triggered="0" working_cardinality="1" ];
alert8 -> eval9 [processed="1"];

eval9 [avg_exec_time_ns="0s" errors="0" working_cardinality="1" ];
eval9 -> influxdb_out10 [processed="1"];

influxdb_out10 [avg_exec_time_ns="0s" errors="0" points_written="0" working_cardinality="0" write_errors="0" ];
}
[root@tick:/var/lib/kapacitor/task]# zy_influx -database chronograf
Connected to http://localhost:8086 version 1.7.7
InfluxDB shell version: 1.7.7
> select * from alerts order by time desc limit 1;
name: alerts
time                active_value alertID       alertName cpu duration     host     level    message                                                                                                                                                                                   new_value port server    triggerType value
----                ------------ -------       --------- --- --------     ----     -----    -------                                                                                                                                                                                   --------- ---- ------    ----------- -----
1567757340000000000 1            主机CPU使用率大于10%               720000000000 influxdb CRITICAL {'alert': '主机 map[port:801 server:localhost host:influxdb] Nginx','description': '主机 influxdb CPU 使用率 <no value>% ，CPU 使用率过高会导致系统运行缓慢，应用出现异常等问题','suggestion': '请查看CPU使用率高的进程/应用是否为异常导致'} 65        801  localhost             1
>
```
