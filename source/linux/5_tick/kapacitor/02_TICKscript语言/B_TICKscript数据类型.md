---
title: 02-TICKscript语言-B-数据类型
categories:
  - - 培训教程
    - TICK技术栈之kapacitor
tags:
  - 监控告警
  - TICK技术栈
  - TICKscript语言
date: 2020-04-14 11:28:44
---

<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [TICKscript 数据类型](#tickscript-数据类型)
	- [布尔值 Booleans](#布尔值-booleans)
	- [数字型 Int64 AND Float64](#数字型-int64-and-float64)
	- [持续时间 Duration](#持续时间-duration)
	- [字符串](#字符串)
	- [字符串模板](#字符串模板)
	- [字符串列表](#字符串列表)
	- [正则表达式](#正则表达式)
	- [Lambda表达式](#lambda表达式)
				- [节点](#节点)
- [数据类型转换](#数据类型转换)

<!-- /TOC -->

# TICKscript 数据类型


TICKscript包含9种数据类型。

| **类型**   | **说明**                                     |
| ------------ | -------------------------------------------- |
| **Booleans**       | 布尔值 `TRUE`、`FALSE`    |
| **String**       | 字符串     |
| **Int64**      | 整型      |
| **Float64**     | 浮点型    |
| **Duration** | 持续时间   |
| **String Lists** | 字符串列表   |
| **Regular expressions** | 正则表达式   |
| **Lambda expressions**   | Lambda表达式 |
| **Nodes**   | 节点 |


## 布尔值 Booleans

使用布尔关键字生成布尔值：`TRUE`和`FALSE`。请注意，这些关键字使用全部大写字母。使用小写字符时，解析器将抛出错误，例如`True`或`true`。

### 示例3 - 布尔值

```js
var true_bool = TRUE
...
   |flatten()
       .on('host','port')
       .dropOriginalFieldName(FALSE)
...
```

在上面的示例3中，第一行显示了使用布尔值的简单赋值。第二个示例显示`FALSE`在方法调用中使用布尔值。

## 数字型 Int64 AND Float64

任何只包含数字和可选的小数的文字标记都将导致生成数字类型的实例。TICKscript了解基于Go的两种数字类型：`int64`和`float64`。任何包含小数点的数字标记都将导致创建`float64`值。任何以不包含小数点结束的数字标记都将导致创建`int64`值。如果整数以零字符为前缀`0`，则将其解释为八进制。

### 例4 - 数字型

```js
var my_int = 6
var my_float = 2.71828
var my_octal = 0400
...
```

在例4中`my_int`的类型的`int64`，`my_float`是类型`float64`和`my_octal`类型为`int64`八进制。

## 持续时间 Duration

持续时间文字定义了一段时间。它们的语法遵循[InfluxQL中](https://docs.influxdata.com/influxdb/v1.4/query_language/spec/#literals)的相同语法。持续时间文字由两部分组成：整数和持续时间单位。它本质上是一个由一个或一对保留字符终止的整数，表示一个时间单位。

下表显示了用于声明持续时间类型的时间单位。

表5 - 持续时间单位

| **单元** | **含义**             |
| -------- | -------------------- |
| u or μ   | 微秒（百万分之一秒） |
| ms       | 毫秒（千分之一秒）   |
| s        | 秒                   |
| m        | 分钟                 |
| H        | 小时                 |
| d        | 天                   |
| w        | 周                   |

### 示例5 - 持续时间表达式

```js
var span = 10s
var frequency = 10s
...
var views = batch
    |query('SELECT sum(value) FROM "pages"."default".views')
        .period(1h)
        .every(1h)
        .groupBy(time(1m), *)
        .fill(0)
```

在上面的示例5中，前两行显示了Duration类型的声明。第一个表示10秒的时间跨度，第二个表示10秒的时间范围。最后一个示例显示直接在方法调用中声明持续时间。

## 字符串

字符串以一个或三个单引号开头：`'`或`'''`。可以使用加法`+`运算符连接字符串。若要转义由单引号分隔的字符串中的引号，请使用反斜杠字符`\`。如果要预期在字符串中将遇到许多单引号，请使用三个单引号来分隔它。由三引号分隔的字符串不需要转义序列。在两个字符串分界情况下，可以使用双引号（用于访问字段和标记值）而无需转义。

### 例6 - 基本字符串

```js
var region1 = 'EMEA'
var old_standby = 'foo' + 'bar'
var query1 = 'SELECT 100 - mean(usage_idle) AS stat FROM "telegraf"."autogen"."cpu" WHERE cpu = \'cpu-total\' '
var query2 = '''SELECT 100 - mean(usage_idle) AS stat FROM "telegraf"."autogen"."cpu" WHERE cpu = 'cpu-total' '''
...
batch
   |query('''SELECT 100 - mean(usage_idle) AS stat FROM "telegraf"."autogen"."cpu" WHERE cpu = 'cpu-total' ''')
...
```

在上面的示例6中，第一行显示了使用字符串文字的简单字符串赋值。
第二行使用连接运算符。第三行和第四行显示了使用和不使用内部转义单引号来声明复杂字符串文字的两种不同方法。
最后一个示例显示了在方法调用中直接使用字符串文字。

要制作长复杂字符串，字符串中允许使用更易读的换行符。

### 例7 - 多行字符串

```js
batch
   |query('SELECT 100 - mean(usage_idle)
           AS stat
           FROM "telegraf"."autogen"."cpu"
           WHERE cpu = \'cpu-total\'
           ')
```

在上面的示例7中，字符串被分解以使查询更容易理解。

## 字符串模板

字符串模板允许将节点属性，标记和字段添加到字符串中。

格式遵循Go [text.template](https://golang.org/pkg/text/template/) 包提供的相同格式。

这在编写警报消息时很有用。

要将属性，标记或字段值添加到字符串模板，需要将其包装在双花括号内。

### 示例8 - 字符串模板内的变量

```js
|alert()
  .id('{{ index .Tags "host"}}/mem_used')
  .message('{{ .ID }}:{{ index .Fields "stat" }}')
```

在示例8中，将三个值添加到两个字符串模板中。在对setter的调用`id()`中，标记的值`"host"`被添加到字符串的开头。`message()`然后对setter的调用会添加`id`字段的值，然后再添加字段的值`"stat"`。

字符串模板当前适用于[Alert](https://docs.influxdata.com/kapacitor/v1.5/nodes/alert_node/)节点，将在下面的“ [访问字符串模板](https://docs.influxdata.com/kapacitor/v1.5/tick/syntax/#accessing-values-in-string-templates)中的[值](https://docs.influxdata.com/kapacitor/v1.5/tick/syntax/#accessing-values-in-string-templates) ”一节中进一步讨论。

字符串模板还可以包括流语句`if...else`，以及对内部格式化方法的调用。

```js
.message('{{ .ID }} is {{ if eq .Level "OK" }}alive{{ else }}dead{{ end }}: {{ index .Fields "emitted" | printf "%0.3f" }} points/10s.')
```

## 字符串列表

字符串列表是在两个括号之间声明的字符串的集合。可以使用文字，其他变量的标识符或星号通配符“\*”声明它们。它们可以传递给采用多个字符串参数的方法。它们在模板任务中特别有用。请注意，在函数调用中使用时，列表内容会爆炸，元素将用作函数的所有参数。给出列表时，可以理解列表包含函数的所有参数。

### 示例9 - 标准任务中的字符串列表

```js
var foo = 'foo'
var bar = 'bar'
var foobar_list = [foo, bar]
var cpu_groups = [ 'host', 'cpu' ]
...
stream
   |from()
      .measurement('cpu')
      .groupBy(cpu_groups)
...
```

例9声明了两个字符串列表。第一个包含其他变量的标识符。第二个包含字符串文字。该列表`cpu_groups`用于该方法`from.groupBy()`。

### 示例10 - 模板任务中的字符串列表

```js
dbrp "telegaf"."not_autogen"

var measurement string
var where_filter = lambda: TRUE
var groups = [*]
var field string
var warn lambda
var crit lambda
var window = 5m
var slack_channel = '#alerts'

stream
    |from()
        .measurement(measurement)
        .where(where_filter)
        .groupBy(groups)
    |window()
        .period(window)
        .every(window)
    |mean(field)
    |alert()
         .warn(warn)
         .crit(crit)
         .slack()
         .channel(slack_channel)
```

示例10取自[代码库中](https://github.com/influxdata/kapacitor/blob/1de435db363fa8ece4b50e26d618fc225b38c70f/examples/load/templates/implicit_template.tick)的示例，定义`implicit_template.tick`。它使用`groups`列表来保存要传递给`from.groupBy()`方法的变量参数。`groups`当模板用于创建新任务时，将确定列表的内容。


## 正则表达式

正则表达式以正斜杠开头和结尾：`/`。正则表达式语法与Perl，Python和其他语言相同。有关语法的详细信息，请参阅Go [正则表达式库](https://golang.org/pkg/regexp/syntax/)。

### 例11 - 正则表达式

```js
var cz_turbines = /^cz\d+/
var adr_senegal = /\.sn$/
var local_ips = /^192\.168\..*/
...
var locals = stream
   |from()
      .measurement('responses')
      .where(lambda: "node" =~ local_ips )

var south_afr = stream
   |from()
      .measurement('responses')
      .where(lambda: "dns_node" =~ /\.za$/ )
```

在例11中，前三行显示了正则表达式对变量的赋值。的`locals`流使用分配给该变量的正则表达式`local_ips`。该`south_afr`流使用正则表达式比较，该正则表达式与字面上声明的正则表达式一起作为lambda表达式的一部分。

## Lambda表达式

lambda表达式是一个参数，表示要传递给方法调用或保存在变量中的简短易理解函数。它可以包装布尔表达式，数学表达式，对内部函数的调用或这三者的组合。Lambda表达式始终对点数据进行操作。它们通常是紧凑的，因此用作文字，最终传递给节点方法。可以在Lambda表达式中使用的内部函数将在下面的[类型转换](https://docs.influxdata.com/kapacitor/v1.5/tick/syntax/#type-conversion)和[Lambda表达式一](https://docs.influxdata.com/kapacitor/v1.5/tick/syntax/#lambda-expressions)节中讨论。Lambda表达式在[Lambda表达式](https://docs.influxdata.com/kapacitor/v1.5/tick/expr/)主题中详细介绍。

Lambda表达式以令牌开头，`lambda`后跟冒号`：` - `lambda:`。

### 例12- Lambda表达式

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

上面的例12表明lambda表达式可以直接赋值给变量。在eval节点中，使用lambda语句调用sigma函数。警报节点使用lambda表达式来定义给定事件的日志级别。

## 节点

与更简单的类型一样，节点类型已声明，可以分配给变量。

### 例13 - 节点表达式

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

在上面的示例13中，在第一部分中，创建了五个节点。顶级节点`stream`分配给变量`data`。所述`stream`然后节点被用作管道输送到该节点的根`from`，`eval`，`window`和`mean`按顺序链的。在第二部分中，然后使用赋值给变量扩展管道`alert`，以便可以将第二个`eval`节点应用于数据。

# 数据类型转换

> 在lambda表达式中，可以使用无状态转换函数在类型之间转换值。

* `bool()` - converts a `string`, `int64` or `float64` to `Boolean`.
* `int()` - converts a `string`, `float64`, `Boolean` or `duration type` to an `int64`.
* `float()` - converts a `string`, `int64` or `Boolean` to `float64`.
* `string()` - converts an `int64`, `float64`, `Boolean` or `duration value` to a `string`.
* `duration()` - converts an `int64`, `float64` or `string` to a `duration type`.
