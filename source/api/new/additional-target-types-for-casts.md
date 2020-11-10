---
title: CAST() 和 CONVERT()新增目标类型
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

## 功能描述

{% note info 数据类型转换函数%}
[`CAST()`](https://dev.mysql.com/doc/refman/8.0/en/cast-functions.html#function_cast)和 [`CONVERT()`](https://dev.mysql.com/doc/refman/8.0/en/cast-functions.html#function_convert)现在支持转换到类型 [`DOUBLE`](https://dev.mysql.com/doc/refman/8.0/en/floating-point-types.html)， [`FLOAT`](https://dev.mysql.com/doc/refman/8.0/en/floating-point-types.html)和 [`REAL`](https://dev.mysql.com/doc/refman/8.0/en/floating-point-types.html)。在MySQL 8.0.17中添加。请参见[第12.10节"Cast Functions and Operators"](https://dev.mysql.com/doc/refman/8.0/en/cast-functions.html)。
{% endnote %}

## 功能学习

名称|	描述
:--|:--
CAST()|	将值强制转换为特定类型
CONVERT()|	将值强制转换为特定类型


```sql
select convert('abc', float);
select cast('abc' as float);

select convert(1, double);
select cast(1 as double);
```

FLOAT和DOUBLE类型代表近似数字数据值。MySQL对单精度值使用四个字节，对双精度值使用八个字节。

- `DOUBLE`

  产生[`DOUBLE`](https://dev.mysql.com/doc/refman/8.0/en/floating-point-types.html)结果。在MySQL 8.0.17中添加。

- `FLOAT[(*`p`*)]`

  如果*`p`*未指定精度，则产生type的结果 [`FLOAT`](https://dev.mysql.com/doc/refman/8.0/en/floating-point-types.html)。如果 *`p`*提供并且0 <= << *`p`*= 24，则结果为类型`FLOAT`。如果25 <= *`p`*<= 53，则结果为类型[`DOUBLE`](https://dev.mysql.com/doc/refman/8.0/en/floating-point-types.html)。如果 *`p`*<0或 *`p`*> 53，则返回错误。在MySQL 8.0.17中添加。

### CONVERT的用法

1. `CONVERT(e)` 一个参数，使用一个`USING`子句，实现不同字符集之间数据转换，语法：

  ```SQL
  CONVERT(expr USING transcoding_name)
  ```

  在MySQL中，转码名称与相应的字符集名称相同，例如：

  ```SQL
  SELECT CONVERT('test' USING utf8mb4);
  SELECT CONVERT(_latin1'Müller' USING utf8mb4);
  INSERT INTO utf8mb4_table (utf8mb4_column)
      SELECT CONVERT(latin1_column USING utf8mb4) FROM latin1_table;
  ```

2. `CONVERT(s, c)` 两个参数，语法：

    ```SQL
    CONVERT(string, CHAR[(N)] CHARACTER SET charset_name)
    ```

    例如：

    ```SQL
    SELECT CONVERT('test', CHAR CHARACTER SET utf8mb4);
    ```

    注意，如果要额外指定转换非默认的字符排序规则时，语法为： `SELECT CONVERT() COLLATE utf8mb4_bin;`


{% note info 习题1%}
**比较以不同字符集表示的字符串,`_latin1 'abc'` 和` _latin2 'abc'`。**

```SQL
SET @s1 = _latin1 'abc', @s2 = _latin2 'abc';
select @s1=@s2;
select @s1=convert(@s2 using latin1);
select @s1=convert(@s2, char character set latin1);
```
{% endnote %}

结果：

```sql
mysql> SET @s1 = _latin1 'abc', @s2 = _latin2 'abc';
Query OK, 0 rows affected (0.00 sec)

mysql> select @s1=@s2;
ERROR 1267 (HY000): Illegal mix of collations (latin1_swedish_ci,IMPLICIT) and (latin2_general_ci,IMPLICIT) for operation '='
mysql> select @s1=convert(@s2 using latin1);
+-------------------------------+
| @s1=convert(@s2 using latin1) |
+-------------------------------+
|                             1 |
+-------------------------------+
1 row in set (0.00 sec)

mysql> select @s1=convert(@s2, char character set latin1);
+---------------------------------------------+
| @s1=convert(@s2, char character set latin1) |
+---------------------------------------------+
|                                           1 |
+---------------------------------------------+
1 row in set (0.00 sec)
```


{% note info 习题2%}
**转换为日期类型**
强制转换功能对于在`CREATE TABLE ... SELECT`语句中创建具有特定类型的列很有用

```sql
mysql> SELECT CAST('2000/01/01' AS DATE);
+----------------------------+
| CAST('2000/01/01' AS DATE) |
+----------------------------+
| 2000-01-01                 |
+----------------------------+
1 row in set (0.00 sec)
```
{% endnote %}

{% note info 习题3%}
**对ENUM的列进行排序**

强制转换功能对于按ENUM词汇顺序对列进行排序很有用。通常，ENUM使用内部数值对列进行排序。将值强制转换 CHAR为词法排序：

```SQL
SELECT enum_col FROM tbl_name ORDER BY CAST(enum_col AS CHAR);
```
{% endnote %}


### CAST用法

CAST() 是标准的SQL语法。

```SQL
CAST(expr AS type [ARRAY])
```

例如：

```SQL
CAST(string AS CHAR[(N)] CHARACTER SET charset_name)
SELECT CAST('test' AS CHAR CHARACTER SET utf8mb4) COLLATE utf8mb4_bin;
```

### Type的类型

[`CONVERT()`](https://dev.mysql.com/doc/refman/8.0/en/cast-functions.html#function_convert)不带， `USING`并 [`CAST()`](https://dev.mysql.com/doc/refman/8.0/en/cast-functions.html#function_cast)使用一个表达式和一个 *`type`*值指定结果类型。这些*`type`*值是允许的：

- `BINARY[(*`N`*)]`

  产生具有[`BINARY`](https://dev.mysql.com/doc/refman/8.0/en/binary-varbinary.html)数据类型的字符串 。有关如何影响比较的描述，请参见 [第11.3.3节“ BINARY和VARBINARY类型”](https://dev.mysql.com/doc/refman/8.0/en/binary-varbinary.html)。如果*`N`*给出了可选的长度 ，则 导致强制类型转换使用不超过 参数字节的字节数。小于字节的值用字节填充，长度为 。 `BINARY(*`N`*)`*`N`**`N`*`0x00`*`N`*

- `CHAR[(*`N`*)] [*`charset_info`*]`

  产生具有[`CHAR`](https://dev.mysql.com/doc/refman/8.0/en/char.html)数据类型的字符串 。如果*`N`*给出了可选的长度，则 导致强制类型转换最多使用 参数的字符。小于字符的值不会出现填充 。 `CHAR(*`N`*)`*`N`**`N`*

  如果没有*`charset_info`*子句，则 `CHAR`生成具有默认字符集的字符串。要明确指定字符集，可以使用以下*`charset_info`*值：

  - `CHARACTER SET *`charset_name`*`：产生具有给定字符集的字符串。
  - `ASCII`：的简写 `CHARACTER SET latin1`。
  - `UNICODE`：的简写 `CHARACTER SET ucs2`。

  在所有情况下，字符串都具有字符集默认排序规则。

- `DATE`

  产生一个[`DATE`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html)值。

- `DATETIME`

  产生一个[`DATETIME`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html)值。

- `DECIMAL[(*`M`*[,*`D`*])]`

  产生一个[`DECIMAL`](https://dev.mysql.com/doc/refman/8.0/en/fixed-point-types.html)值。如果给出了可选*`M`*和 *`D`*值，则它们指定最大位数（精度）和小数点后的位数（小数位数）。

- `DOUBLE`

  产生[`DOUBLE`](https://dev.mysql.com/doc/refman/8.0/en/floating-point-types.html)结果。在MySQL 8.0.17中添加。

- `FLOAT[(*`p`*)]`

  如果*`p`*未指定精度，则产生type的结果 [`FLOAT`](https://dev.mysql.com/doc/refman/8.0/en/floating-point-types.html)。如果 *`p`*提供并且0 <= << *`p`*= 24，则结果为类型`FLOAT`。如果25 <= *`p`*<= 53，则结果为类型[`DOUBLE`](https://dev.mysql.com/doc/refman/8.0/en/floating-point-types.html)。如果 *`p`*<0或 *`p`*> 53，则返回错误。在MySQL 8.0.17中添加。

- `JSON`

  产生一个[`JSON`](https://dev.mysql.com/doc/refman/8.0/en/json.html)值。有关在[`JSON`](https://dev.mysql.com/doc/refman/8.0/en/json.html)和其他类型之间进行值转换的规则的详细信息 ，请参见 [JSON值的比较和排序](https://dev.mysql.com/doc/refman/8.0/en/json.html#json-comparison)。

- `NCHAR[(*`N`*)]`

  与相似`CHAR`，但会产生带有国家字符集的字符串。请参见 [第10.3.7节“国家字符集”](https://dev.mysql.com/doc/refman/8.0/en/charset-national.html)。

  与不同`CHAR`，`NCHAR` 不允许指定结尾字符集信息。

- `REAL`

  产生type的结果 [`REAL`](https://dev.mysql.com/doc/refman/8.0/en/floating-point-types.html)。这实际上是 `FLOAT`，如果 `REAL_AS_FLOAT`启用了SQL模式; 否则结果为类型 `DOUBLE`。

- `SIGNED [INTEGER]`

  产生一个有符号的整数值。

- `TIME`

  产生一个[`TIME`](https://dev.mysql.com/doc/refman/8.0/en/time.html)值。

- `UNSIGNED [INTEGER]`

  产生一个无符号整数值。
