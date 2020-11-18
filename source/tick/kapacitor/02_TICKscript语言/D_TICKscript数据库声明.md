---
title: 02-TICKscript语言-D-数据库声明
categories:
  - - 培训教程
    - TICK技术栈之kapacitor
tags:
  - 监控告警
  - TICK技术栈
  - TICKscript语言
date: 2020-04-14 11:28:46
---

<!-- MDTOC maxdepth:6 firsth1:1 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

- [TICKscript数据库声明](#tickscript数据库声明)   
   - [语法](#语法)   
   - [注意](#注意)   

<!-- /MDTOC -->

# TICKscript数据库声明

## 语法

例22 - 数据库声明

```
dbrp "telegraf"."autogen"
```

示例22声明TICKscript将用于其数据库telegraf及其保留策略autogen。

## 注意

数据库声明关键字`dbrp`，随后是用`句点`分隔两个字符串。

* 第一个字符串声明`默认数据库`，将使用该数据库。
* 第二个字符串声明`其保留策略`。
* 该语句为可选
* 如果使用则为脚本的第一个声明

请注意，在命令行上使用命令`-dbrp`定义任务时，也可以使用`kapacitor define`标志声明数据库和保留策略。
