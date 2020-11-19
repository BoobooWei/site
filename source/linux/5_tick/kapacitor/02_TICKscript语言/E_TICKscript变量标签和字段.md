---
title: 02-TICKscript语言-E-变量标签和字段
categories:
  - - 培训教程
    - TICK技术栈之kapacitor
tags:
  - 监控告警
  - TICK技术栈
  - TICKscript语言
date: 2020-04-14 11:28:47
---

<!-- MDTOC maxdepth:6 firsth1:1 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

- [TICKscript变量标签和字段](#tickscript变量标签和字段)   
   - [访问变量](#访问变量)   
   - [访问标签和字段](#访问标签和字段)   
   - [其他访问](#其他访问)   

<!-- /MDTOC -->

# TICKscript变量标签和字段

TICKscript不仅可以使用自定义的变量，还可以使用来自数据库中的Tag和Field。

## 访问变量

要访问TICKscript变量，只需使用其标识符（变量名）即可。

```js
var db = 'telegraf'
...
var data = stream
 |from()
  .database(db)
```

`db`变量的值是一个字符串'telegraf'，然后在`.database()`链接方法中访问该变量.

## 访问标签和字段

|对象|声明|访问|
|:--|:--|:--|
|字符串类型的变量|声明时使用单引号或三引号|访问时使用变量名即可|
|Tag和Field||访问Lambda表达式中的tag或field，必须使用`双引号`|
|Tag和Field||访问方法调用中的tag或field，必须使用`单引号`|

> **为什么调用Tag和Field的符号不同呢？**
>
> 在方法调用中，这些实质上是字符串文字，供节点用于匹配数据库中的标记Tag或字段值Field。


```js
   // Data frame
  var data = stream
     |from()
        .database('telegraf')
        .retentionPolicy('autogen')
        .measurement('cpu')
        .groupBy('host')
        .where(lambda: "cpu" == 'cpu-total')
     |eval(lambda: 100.0 - "usage_idle")
        .as('used')
   ...
```

在示例中，访问来自数据帧的两个值
* 在`where()`方法调用中，lambda表达式使用Tag标签`cpu`进行数据过滤，仅匹配` cpu = 'cpu-total'`的值。
* 链接方法`eval()`还采用lambda表达式访问Field字段`usage-idle`以计算`100.0 - "usage_idle"`来获取CPU当前的使用率`used`。
* `groupBy()`方法使用字符串`host`与数据系列中的标记名称Tag匹配。然后，它将按此标记对数据进行分组。

命名的lambda表达式结果:

* Lambda表达式结果被命名并使用`as()`方法作为字段添加到数据集中。
* as()方法的功能就像InfluxQL中的`AS`关键字一样。


## 其他访问

后续补充
