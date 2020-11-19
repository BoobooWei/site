---
title: 02-TICKscript语言-H-InfluxQL
categories:
  - - 培训教程
    - TICK技术栈之kapacitor
tags:
  - 监控告警
  - TICK技术栈
  - TICKscript语言
date: 2020-04-14 11:28:50
---

<!-- MDTOC maxdepth:6 firsth1:1 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

- [TICKscript InfluxQL](#tickscript-influxql)   
   - [示例30 - 一个简单的InfluxQL查询语句](#示例30-一个简单的influxql查询语句)   
   - [例31 - 带变量的简单InfluxQL查询语句](#例31-带变量的简单influxql查询语句)   
   - [示例32 - 带有函数调用的InfluxQL查询语句](#示例32-带有函数调用的influxql查询语句)   

<!-- /MDTOC -->

# TICKscript InfluxQL

InfluxQL主要在`query`节点中发生在TICKscript中，其链接方法采用InfluxQL查询字符串。这几乎总是一个`SELECT`声明。

InfluxQL的语法与SQL非常相似。当编写一个TICKscript查询字符串`query`节点，一般只有三个条款将被要求：`SELECT`，`FROM`和`WHERE`。一般模式如下：

```SQL
SELECT {<FIELD_KEY> | <TAG_KEY> | <FUNCTION>([<FIELD_KEY>|<TAG_KEY])} FROM <DATABASE>.<RETENTION_POLICY>.<MEASUREMENT> WHERE {<CONDITIONAL_EXPRESSION>}
```

- `SELECT`子句可以使用一个或多个字段或标记键或函数。这些可以与数学运算和字面值组合。它们的值或结果将添加到数据框中，并且可以使用`AS`子句别名。星号`*`外卡还可用于从测量中检索所有标签和字段。

- 使用该`AS`子句时，可以稍后使用双引号将别名标识符作为命名结果在TICKscript中访问。

- 该`FROM`子句需要数据库，保留策略和将从中选择值的度量名称。这些令牌中的每一个都用点分隔。需要使用双引号设置数据库和保留策略的值。

- 该`WHERE`子句需要条件表达式。这可能包括`AND`和`OR`布尔运算符以及数学运算。

## 示例30 - 一个简单的InfluxQL查询语句

```js
batch
    |query('SELECT cpu, usage_idle FROM "telegraf"."autogen".cpu WHERE time > now() - 10s')
        .period(10s)
        .every(10s)
    |httpOut('dump')
```

示例30显示了一个简单的`SELECT`语句，该语句从过去十秒内记录的cpu测量中获取`cpu`标记和`usage_idle`字段。

## 例31 - 带变量的简单InfluxQL查询语句

```js
var my_field = 'usage_idle'
var my_tag = 'cpu'

batch
    |query('SELECT ' + my_tag + ', ' + my_field + ' FROM "telegraf"."autogen".cpu WHERE time > now() - 10s')
        .period(10s)
        .every(10s)
    |httpOut('dump')
```

示例31重复了示例30中的相同查询，但显示了如何将变量添加到查询字符串。

## 示例32 - 带有函数调用的InfluxQL查询语句

```js
...
var data = batch
  |query('''SELECT 100 - mean(usage_idle) AS stat FROM "telegraf"."autogen"."cpu" WHERE cpu = 'cpu-total' ''')
    .period(period)
    .every(every)
    .groupBy('host')
...
```

示例32显示了一个`SELECT`语句，该语句包含`SELECT`子句中的函数和数学运算，以及`AS`alias子句。

请注意，select语句将直接传递给InfluxDB API。在InfluxQL查询字符串字段和标记名称中不需要使用双引号访问，就像TICKscript中的其他情况一样。但是，数据库名称和保留策略确实包含在双引号中。字符串文字，例如`'cpu-total'`在带有单引号的查询字符串中表示。

有关使用查询语言的完整介绍，请参阅[InfluxQL](https://docs.influxdata.com/influxdb/v1.3/query_language/)文档。
