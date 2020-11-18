---
title: 02-TICKscript语言-C-变量声明
categories:
  - - 培训教程
    - TICK技术栈之kapacitor
tags:
  - 监控告警
  - TICK技术栈
  - TICKscript语言
date: 2020-04-14 11:28:45
---

<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [TICKscript 变量声明](#tickscript-变量声明)
	- [语句](#语句)
	- [变量声明](#变量声明)
		- [布尔值声明](#布尔值声明)
		- [字符串声明](#字符串声明)
		- [整数声明](#整数声明)
		- [浮点型声明](#浮点型声明)
		- [持续时间声明](#持续时间声明)
		- [字符串列表](#字符串列表)
		- [正则表达式声明](#正则表达式声明)
		- [lambda表达式声明](#lambda表达式声明)
		- [节点声明](#节点声明)

<!-- /TOC -->

# TICKscript 变量声明

## 语句

TICKscript中有两种类型的语句：声明和表达式。

* 声明可以声明变量或数据库；
* 表达式表示方法调用的管道（又称链），它创建处理节点并设置其属性。

![](pic/dag_02.jpg)

## 变量声明

### 布尔值声明

> 布尔值必须为大写

```js
var is_true = TRUE
var is_false = FALSE
```

### 字符串声明

> 字符串定义：单引号和三引号

```js
var db = 'telegraf'
var rp = 'autogen'
var name = 'booboo'
var idName = name + 'wei'
var query = '''select host from telegraf.autogen where cpu='total' '''
```

### 整数声明

```js
var int_num = 1
```

### 浮点型声明

```js  
var float_num = 1.1
```

### 持续时间声明

```js
var period = 5m
```

### 字符串列表

```js
var cpu_groups = [ 'host', 'cpu' ]
```

### 正则表达式声明

```js
var cz_turbines = /^cz\d+/
var adr_senegal = /\.sn$/
var local_ips = /^192\.168\..*/
var locals = lambda: "192.168.1.1" =~ local_ips
```

### lambda表达式声明

> * Lambda表达式始终对点数据进行操作;
> * 它可以包装布尔表达式，数学表达式，对内部函数的调用或这三者的组合

```js
var my_lambda = lambda: 1 > 0
var lazy_lambda = lambda: "usage_idle" < 95
...
var data = stream
  |from()
...
var alert = data
  |eval(lambda: sigma("stat"))
    .as('sigma')
    .keep()
  |alert()
    .id('{{ index .Tags "host"}}/cpu_used')
    .message('{{ .ID }}:{{ index .Fields "stat" }}')
    .info(lambda: "stat" > 70 OR "sigma" > 2.5)
    .warn(lambda: "stat" > 80 OR "sigma" > 3.0)
    .crit(lambda: "stat" > 90 OR "sigma" > 3.5)
```

### 节点声明

节点表达式：

```js
var data = stream
  |from()
    .database('telegraf')
    .retentionPolicy('autogen')
    .measurement('cpu')
    .groupBy('host')
    .where(lambda: "cpu" == 'cpu-total')
  |eval(lambda: 100.0 - "usage_idle")
    .as('used')
  |window()
    .period(span)
    .every(frequency)
  |mean('used')
    .as('stat')
...
var alert = data
  |eval(lambda: sigma("stat"))
    .as('sigma')
    .keep()
  |alert()
    .id('{{ index .Tags "host"}}/cpu_used')
...
```

在第一部分中，创建了五个节点:

`stream --> from --> eval --> window --> mean`

* 顶级节点stream分配给变量data;
* stream节点通过管道将数据输送到from节点，后续一次是eval，window和mean节点。

在第二部分中，创建了两个节点：

`eval --> alert`

* 声明变量alert，将data赋值给alert；
* 通过管道将数据输送到eval节点，后续是alert节点。
