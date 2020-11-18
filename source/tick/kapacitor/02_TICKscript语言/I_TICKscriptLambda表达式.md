---
title: 02-TICKscript语言-I-Lambda表达式
categories:
  - - 培训教程
    - TICK技术栈之kapacitor
tags:
  - 监控告警
  - TICK技术栈
  - TICKscript语言
date: 2020-04-14 11:28:51
---

<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [TICKscript Lambda表达式](#tickscript-lambda表达式)
	- [概述](#概述)
		- [简介](#简介)
		- [内置函数](#内置函数)
	- [有状态函数](#有状态函数)
		- [`sigma`可用于定义强大的告警](#sigma可用于定义强大的告警)
			- [知识点-平均偏差](#知识点-平均偏差)
	- [无状态函数](#无状态函数)
		- [类型转换函数](#类型转换函数)
		- [判断存在的函数](#判断存在的函数)
		- [时间函数](#时间函数)
		- [数学函数](#数学函数)
		- [字符串函数](#字符串函数)
		- [人性化函数](#人性化函数)
		- [条件函数](#条件函数)

<!-- /TOC -->



# TICKscript Lambda表达式

[Lambda表达式官方帮助](https://docs.influxdata.com/kapacitor/v1.5/tick/expr/)

[函数帮助](https://help.aliyun.com/document_detail/113126.html?spm=a2c4g.11174283.6.740.529f77a2ijwJ6I#Transformations)

## 概述

TICKscript使用lambda表达式定义数据点的转换，并定义充当过滤器的布尔条件。

### 简介

Lambda表达式包含：
* 数学运算
* 布尔运算
* 内部函数调用
* 或三者的组合

TICKscript尝试类似于InfluxQL，因为您在InfluxQL WHERE子句中使用的大多数表达式将作为TICKscript中的表达式使用，但具有自己的语法：

* lambda表达式都以关键字`lambda:`开头。
* 所有`字段Field`或`标记Tag` 的标识符必须加双引号。
* 相等的比较运算符为`==`不是`=`。

### 内置函数

内置函数分为：

| 内置函数                                                     |                                                              |              | 数量 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------ | ---- |
| [Stateful functions](https://docs.influxdata.com/kapacitor/v1.5/tick/expr/#stateful-functions) | 有状态函数                                                   |              | `3`  |
| [Stateless functions](https://docs.influxdata.com/kapacitor/v1.5/tick/expr/#stateless-functions) | 无状态函数                                                   |              | `78` |
|                                                              | [Type conversion functions](https://docs.influxdata.com/kapacitor/v1.5/tick/expr/#type-conversion-functions) | 类型转换函数 | `5`  |
|                                                              | [Existence](https://docs.influxdata.com/kapacitor/v1.5/tick/expr/#existence) | 存在函数     | `1`  |
|                                                              | [Time functions](https://docs.influxdata.com/kapacitor/v1.5/tick/expr/#time-functions) | 时间函数     | `7`  |
|                                                              | [Math functions](https://docs.influxdata.com/kapacitor/v1.5/tick/expr/#math-functions) | 数学函数     | `42` |
|                                                              | [String functions](https://docs.influxdata.com/kapacitor/v1.5/tick/expr/#string-functions) | 字符串函数   | `21` |
|                                                              | [Human string functions](https://docs.influxdata.com/kapacitor/v1.5/tick/expr/#human-string-functions) | 人性化函数   | `1`  |
|                                                              | [Conditional functions](https://docs.influxdata.com/kapacitor/v1.5/tick/expr/#conditional-functions) | 条件函数     | `1`  |

## 有状态函数

|No.|函数|返回值|描述|
|:--|:--|:--|:--|
|1|`count()`|`int64`|返回表达式的计算次数|
|2|`sigma(value )`|`float64`|计算给定值远离运行平均值的标准偏差数。每次评估表达式时，都会更新运行平均值和标准差。|
|3|`spread(value )`|`float64`|计算传递给它的所有值的运行范围。范围是收到的最大值和最小值之间的差异。|

### `sigma`可用于定义强大的告警

每次计算表达式时，它都会更新正在运行的统计信息，然后返回偏差。

`sigma("value") > 3.0 `表示评估收到的数据点流的平均值的标准偏差：
1. 如果小于等于 3.0，则返回 `False`；
2. 如果超过3.0，则返回`True`。

这样的表达式可以在TICKscript内部使用来定义强大的警报。

```js
stream
    |from()
    ...
    |alert()
        // use an expression to define when an alert should go critical.
        .crit(lambda: sigma("value") > 3.0)
```

#### 知识点-平均偏差

[什么是平均偏差？](https://baike.baidu.com/item/%E5%B9%B3%E5%9D%87%E5%81%8F%E5%B7%AE/11042506)

> 平均偏差是数列中各项数值与其算术平均数的离差绝对值的算术平均数。平均偏差是用来测定数列中各项数值对其平均数离势程度的一种尺度。平均偏差可分为简单平均偏差和加权平均偏差。

**定义**

在统计中，如果要反映出所有原数据间的差异，就要在各原数据之间进行差异比较，当原数据较多时，进行两两比较就很麻烦，因此需要找到一个共同的比较标准，取每个原数据值与标准值进行比较。这个标准值就是算数平均数。

平均偏差就是每个原数据值与算数平均数之差的绝对值的均值，用符号A.D.(average deviation)表示。平均偏差是一种平均离差。离差是总体各单位的标志值与算术平均数之差。因离差和为零，离差的平均数不能将离差和除以离差的个数求得，而必须将离差取绝对数来消除正负号。

平均偏差是反映各标志值与算术平均数之间的平均差异。平均偏差越大，表明各标志值与算术平均数的差异程度越大，该算术平均数的代表性就越小；平均偏差越小，表明各标志值与算术平均数的差异程度越小，该算术平均数的代表性就越大。

平均偏差又有简单平均偏差和加权平均偏差之分。

**计算**

1. 简单平均偏差
如果原数据未分组，则计算平均偏差的公式为：
$$
A.D. =  
\frac{\sum\mid x-\overline x  \mid}{n}
$$
该式称为简单平均偏差。

举例：计算cpu每个点的平均偏差值 `10 11 9 8 12 11`

* 第一个点的值为10，平均偏差为`0`
* 第二个点的值为11，平均偏差为`0.5`
* 第三个点的值为9，平均偏差为`0.82`
* 第四个点的值为8，平均偏差为`1.12`
* 第五个点的值为12，平均偏差为`1.41`
* 第六个点的值为11，平均偏差为`1.34`

```
* 计算平均值
* 计算每个点与平均值的差值的绝对值
* 计算差值的和
* 差值和除以总次数
```

**python计算脚本**

```python
import numpy

def cal_mean_std(sum_list_in):
    # type: (list) -> tuple
    N = sum_list_in.__len__()
    narray = numpy.array(sum_list_in)
    sum = narray.sum()
    mean = sum / N

    narray_dev = narray - mean
    narray_dev = narray_dev * narray_dev
    sum_dev = narray_dev.sum()
    DEV = float(sum_dev) / float(N)
    STDEV = numpy.math.sqrt(DEV)
    return round(mean,2), round(DEV,2), round(STDEV,2)


sum_list_in = [10,11,9,8,12,11]
mean, DEV, STDEV=cal_mean_std(sum_list_in)
print(mean, DEV, STDEV)
```

* mean 平均值
* dev 方差
* stdev 标准差


2. 加权平均偏差
在分组情况下，平均偏差的计算公式为：该式称为加权平均偏差。
$$
A.D. =  
\frac{\sum\mid x-\overline x  \mid f}{n f}
$$

## 无状态函数

### 类型转换函数

|No.|函数|返回值|描述|
|:--|:--|:--|:--|
|1|`bool(value)`|`True/False`|将字符串和数字转换为布尔值|
|2|`int(value)`|`int64`|强制将字符串或float64转换为int64|
|3|`float(value)`|`float64`|制将字符串或int64转换为float64|
|4|`string(value)`|`string`|将bool，int64或float64转换为字符串|
|5|`duration(value int64\|float64, unit duration)`|`duration`|将int64或float64转换为持续时间|

### 判断存在的函数

|No.|函数|返回值|解释|
|:--|:--|:--|:--|
|1|`\|where(lambda: isPresent("myfield"))`|`True/False`|判断`myfield`是否存在|

该函数在`where`节点中使用，根据指定的字段或标记键是否存在返回布尔值。用于过滤数据这是缺少指定的字段或标记。

### 时间函数

| No.  | 函数                | 返回值  | 描述                                    |
| ---- | ------------------- | ------- | --------------------------------------- |
| 1    | `unixNano(t time) ` | `int64` | Unix时间                                |
| 2    | `minute(t time) `   | `int64` | 分钟                                    |
| 3    | `hour(t time) `     | `int64` | 小时                                    |
| 4    | `weekday(t time) `  | `int64` | 周 ` [0,6], 0 is Sunday`                |
| 5    | `day(t time) `      | `int64` | the day within the month: range [1,31]  |
| 6    | `month(t time) `    | `int64` | the month within the year: range [1,12] |
| 7    | `year(t time) `     | `int64` | 年                                      |

例如

```js
lambda: hour("time") >= 9 AND hour("time") < 19
```

### 数学函数

| No.  | 函数                                                         | 返回值 | 描述                                                         |
| ---- | ------------------------------------------------------------ | ------ | ------------------------------------------------------------ |
| 1 | [abs(x)](https://golang.org/pkg/math/#Abs)  | `float64` | Abs返回`x`的绝对值。                                           |
| 2 | [acos(x)](https://golang.org/pkg/math/#Acos) | `float64` | Acos以弧度为单位返回`x`的反余弦。                              |
| 3 | [acosh(x)](https://golang.org/pkg/math/#Acosh) | `float64` | Acosh返回x`的反双曲余弦值。                                   |
| 4 | [asin(x)](https://golang.org/pkg/math/#Asin) | `float64` | Asin以弧度为单位返回`x`的反正弦值。                            |
| 5 | [asinh(x)](https://golang.org/pkg/math/#Asinh) | `float64` | Asinh返回`x`的反双曲正弦值。                                   |
| 6 | [atan(x)](https://golang.org/pkg/math/#Atan) | `float64` | Atan以弧度为单位返回`x`的反正切值。                            |
| 7 | [atan2(y,x)](https://golang.org/pkg/math/#Atan2) | `float64` | Atan2返回`y / x`的反正切，使用二者的符号确定返回值的象限。     |
| 8 | [atanh(x)](https://golang.org/pkg/math/#Atanh) | `float64` | Atanh返回`x`的反双曲正切。                                     |
| 9 | [cbrt(x)](https://golang.org/pkg/math/#Cbrt) | `float64` | Cbrt返回`x`的立方根。                                          |
| 10 | [ceil(x)](https://golang.org/pkg/math/#Ceil) | `float64` | Ceil返回大于或等于`x`的最小整数值。                            |
| 11 | [cos(x)](https://golang.org/pkg/math/#Cos)  | `float64` | Cos返回弧度参数`x`的余弦值。                                   |
| 12 | [cosh(x)](https://golang.org/pkg/math/#Cosh) | `float64` | Cosh返回`x`的双曲余弦值。                                      |
| 13 | [erf(x)](https://golang.org/pkg/math/#Erf)  | `float64` | Erf返回`x`的错误函数。                                         |
| 14 | [erfc(x)](https://golang.org/pkg/math/#Erfc) | `float64` | Erfc返回`x`的互补误差函数。                                    |
| 15 | [exp(x)](https://golang.org/pkg/math/#Exp)  | `float64` | Exp返回`e ** x`，`x`的`base-e`指数。                               |
| 16 | [exp2(x)](https://golang.org/pkg/math/#Exp2) | `float64` | Exp2返回`2 ** x`，`x`的基数为`2`的指数。                           |
| 17 | [expm1(x)](https://golang.org/pkg/math/#Expm1) | `float64` | Expm1返回`e ** x - 1`，`x`的基数为`e`的指数减`1`.当`x`接近零时，它比`Exp(x）-1`更准确。 |
| 18 | [floor(x)](https://golang.org/pkg/math/#Floor) | `float64` | Floor返回小于或等于`x`的最大整数值。                           |
| 19 | [gamma(x)](https://golang.org/pkg/math/#Gamma) | `float64` | Gamma返回`x`的`Gamma`函数。                                      |
| 20 | [hypot(p,q)](https://golang.org/pkg/math/#Hypot) | `float64` | Hypot返回`Sqrt(p * p + q * q）`，注意避免不必要的溢出和下溢。 |
| 21 | [j0(x)](https://golang.org/pkg/math/#J0)    | `float64` | J0返回第一类的零阶贝塞尔函数。                               |
| 22 | [j1(x)](https://golang.org/pkg/math/#J1)    | `float64` | J1返回第一类的一阶贝塞尔函数。                         |
| 23 | [jn（n int64，x)](https://golang.org/pkg/math/#Jn) | `float64` | Jn返回第一种order-n Bessel函数。                             |
| 24 | [log(x)](https://golang.org/pkg/math/#Log)  | `float64` | Log返回`x`的自然对数。                                         |
| 25 | [log10(x)](https://golang.org/pkg/math/#Log10) | `float64` | Log10返回`x`的十进制对数。                                     |
| 26 | [log1p(x)](https://golang.org/pkg/math/#Log1p) | `float64` | Log1p返回`1`的自然对数加上其参数`x`。当`x`接近零时，它比`Log(1 + x）`更准确。 |
| 27 | [log2(x)](https://golang.org/pkg/math/#Log2) | `float64` | Log2返回`x`的二进制对数。                                      |
| 28 | [logb(x)](https://golang.org/pkg/math/#Logb) | `float64` | Logb返回`x`的二进制指数。                                      |
| 29 | [max(x,y)](https://golang.org/pkg/math/#Max) | `float64` | Max返回`x`或`y`中较大的一个。                                    |
| 30 | [min(x,y)](https://golang.org/pkg/math/#Min) | `float64` | Min返回`x`或`y`中较小的一个。                                    |
| 31 | [mod(x,y)](https://golang.org/pkg/math/#Mod) | `float64` | Mod返回`x / y`的浮点余数。结果的大小小于y，其符号与x的符号一致。 |
| 32 | [pow(x,y)](https://golang.org/pkg/math/#Pow) | `float64` | Pow返回`x ** y`，y的base-x指数。                               |
| 33 | [pow10(x int64](https://golang.org/pkg/math/#Pow10) | `float64` | Pow10返回`10 ** e`，`e`的基数为`10`的指数。                        |
| 34 | [sin(x)](https://golang.org/pkg/math/#Sin)  | `float64` | Sin返回弧度参数`x`的正弦值。                                   |
| 35 | [sinh(x)](https://golang.org/pkg/math/#Sinh) | `float64` | Sinh返回`x`的双曲正弦值。                                      |
| 36 | [sqrt(x)](https://golang.org/pkg/math/#Sqrt) | `float64` | Sqrt返回`x`的平方根。                                          |
| 37 | [tan(x)](https://golang.org/pkg/math/#Tan)  | `float64` | Tan返回弧度参数`x`的正切值。                                   |
| 38 | [tanh(x)](https://golang.org/pkg/math/#Tanh) | `float64` | Tanh返回`x`的双曲正切。                                        |
| 39 | [trunc(x)](https://golang.org/pkg/math/#Trunc) | `float64` | Trunc返回`x`的整数值。                                         |
| 40 | [y0(x)](https://golang.org/pkg/math/#Y0)    | `float64` | Y0返回第二种零阶贝塞尔函数。                                 |
| 41 | [y1(x)](https://golang.org/pkg/math/#Y1)    | `float64` | Y1返回第二种顺序一贝塞尔函数。                               |
| 42 | [yn(n int64,x)](https://golang.org/pkg/math/#Yn) | `float64` | Yn返回第二种order-n 贝塞尔函数。                             |

### 字符串函数

| No.  | 函数                                                         | 返回值 | 描述                                                         |
| ---- | ------------------------------------------------------------ | ------ | ------------------------------------------------------------ |
| 1    | [strContains(s, substr)](https://golang.org/pkg/strings/#Contains) | `bool`   | StrContains报告`substr`是否在`s`内。                         |
| 2    | [strContainsAny(s, chars)](https://golang.org/pkg/strings/#ContainsAny) | `bool`   | StrContainsAny报告字符中的任何`Unicode`代码点是否在`s`内。  |
| 3    | [strCount(s, sep)](https://golang.org/pkg/strings/#Count)    | `int64`  | StrCount计算`s`中非重叠`sep`实例的数量。如果`sep`是空字符串，则`Count`返回`1 + s`中的`Unicode`代码点数。 |
| 4    | [strHasPrefix(s, prefix)](https://golang.org/pkg/strings/#HasPrefix) | `bool`   | StrHasPrefix测试字符串`s`是否以`prefix`开头。                |
| 5    | [strHasSuffix(s, suffix)](https://golang.org/pkg/strings/#HasSuffix) | `bool`   | StrHasSuffix测试字符串`s`是否以后`suffix`缀结尾。           |
| 6    | [strIndex(s, sep)](https://golang.org/pkg/strings/#Index)    | `int64`  | StrIndex返回`s`中第一个`sep`实例的索引，如果`s`中不存在`sep`，则返回`-1`。 |
| 7    | [strIndexAny(s, chars)](https://golang.org/pkg/strings/#IndexAny) | `int64`  | StrIndexAny从`1`中的字符返回任何`Unicode`代码点的第一个实例的索引，如果`s`中没有来自`chars`的`Unicode`代码点，则返回`-1`。 |
| 8    | [strLastIndex(s, sep)](https://golang.org/pkg/strings/#LastIndex) | `int64`  | StrLastIndex返回`s`中最后一个`sep`实例的索引，如果`s`中不存在`sep`，则返回`-1`。 |
| 9    | [strLastIndexAny(s, chars)](https://golang.org/pkg/strings/#LastIndexAny) | `int64`  | StrLastIndexAny从`s`中的字符返回任何`Unicode`代码点的最后一个实例的索引，如果s中没有来自`chars`的`Unicode`代码点，则返回`-1`。 |
| 10   | [strLength(s)](https://golang.org/ref/spec#Length_and_capacity) | `int64`  | StrLength返回字符串的长度。                                  |
| 11   | [strReplace(s, old, new), n int64](https://golang.org/pkg/strings/#Replace) | `string` | StrReplace返回字符串`s`的副本，其中前n个非重叠实例由`new`替换。 |
| 12   | [strSubstring(s), start, stop int64](https://golang.org/ref/spec#Index_expressions) | `string` | StrSubstring根据给定的索引返回一个子字符串，`strSubstring(str,start,stop)`相当于Go中的`str [start：stop]`。 |
| 13   | [strToLower(s)](https://golang.org/pkg/strings/#ToLower)     | `string` | StrToLower返回字符串s的副本，其中所有`Unicode`字母都映射到它们的小写字母。 |
| 14   | [strToUpper(s)](https://golang.org/pkg/strings/#ToUpper)     | `string` | StrToUpper返回字符串s的副本，其中所有`Unicode`字母都映射到它们的大写字母。 |
| 15   | [strTrim(s, cutset)](https://golang.org/pkg/strings/#Trim)   | `string` | StrTrim返回字符串s的一个片段，其中包含`cutset`中包含的所有前导和尾随`Unicode`代码点。 |
| 16   | [strTrimLeft(s, cutset)](https://golang.org/pkg/strings/#TrimLeft) | `string` | StrTrimLeft返回字符串`s`的一个片段，其中包含`cutset`中包含的所有前导`Unicode`代码点。 |
| 17   | [strTrimPrefix(s, prefix)](https://golang.org/pkg/strings/#TrimPrefix) | `string` | StrTrimPrefix返回`s`而没有提供的前导前缀字符串。如果`s`不以`prefix`开头，则返回s不变。 |
| 18   | [strTrimRight(s, cutset)](https://golang.org/pkg/strings/#TrimRight) | `string` | StrTrimRight返回字符串`s`的一个切片，并删除了`cutset`中包含的所有尾随`Unicode`代码点。 |
| 19   | [strTrimSpace(s)](https://golang.org/pkg/strings/#TrimSpace) | `string` | StrTrimSpace返回字符串`s`的一部分，删除所有前导和尾随空格，如`Unicode`所定义。 |
| 20   | [strTrimSuffix(s, suffix)](https://golang.org/pkg/strings/#TrimSuffix) | `string` | StrTrimSuffix返回`s`而没有提供的尾随后缀字符串。如果`s`不以后缀结尾，则`s`保持不变。 |
| 21   | [regexReplace(r regex, s, pattern)](https://golang.org/pkg/regexp/#Regexp.ReplaceAllString) | `string` | RegexReplace将输入字符串中正则表达式的匹配替换为输出字符串。例如`regexReplace(/a(b*)c/, ‘abbbc’, ‘group is $1’) -> ‘group is bbb’`。如果未找到匹配项，则返回原始字符串。 |

### 人性化函数

|No.|函数|返回值|描述|
|:--|:--|:--|:--|
|1|`humanBytes(value)`|`string`|将具有单位字节的`int64`或`float64`转换为表示字节数的人类可读字符串。|


### 条件函数

|No.|函数|返回值|描述|
|:--|:--|:--|:--|
|1|`if(condition, true expression, false expression)`|`True/False`|根据第一个参数的值返回其操作数的结果。第二个和第三个参数必须返回相同的类型。|

例：

```js
|eval(lambda: if("field" > threshold AND "field" != 0, 'true', 'false'))
    .as('value')
```

`value`上例中字段的值将是字符串，`true`或者`false`取决于作为第一个参数传递的条件。

该`if`函数的返回类型相同类型作为其第二个和第三个参数。

```js
if(condition, true expression, false expression)
```
