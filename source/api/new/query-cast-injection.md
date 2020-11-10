---
title: MySQL 8.0的新增功能探索-强制转换操作
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

- **查询演员表注入。** 在8.0.18及更高版本中，MySQL将强制转换操作注入表达式和条件内的查询项树中，在该表达式和条件中，参数的数据类型与预期的数据类型不匹配。这对查询结果或执行速度没有影响，但是使查询的执行等同于符合SQL标准的查询，同时保持了与MySQL早期版本的向后兼容性。

现在这样的隐式转换的时间类型（之间执行[`DATE`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html)， [`DATETIME`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html)， [`TIMESTAMP`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html)， [`TIME`](https://dev.mysql.com/doc/refman/8.0/en/time.html)）和数字类型（[`SMALLINT`](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html)， [`TINYINT`](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html)， [`MEDIUMINT`](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html)， [`INT`](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html)/ [`INTEGER`](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html)， [`BIGINT`](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html); [`DECIMAL`](https://dev.mysql.com/doc/refman/8.0/en/fixed-point-types.html)/ [`NUMERIC`](https://dev.mysql.com/doc/refman/8.0/en/fixed-point-types.html); [`FLOAT`](https://dev.mysql.com/doc/refman/8.0/en/floating-point-types.html)， [`DOUBLE`](https://dev.mysql.com/doc/refman/8.0/en/floating-point-types.html)， [`REAL`](https://dev.mysql.com/doc/refman/8.0/en/floating-point-types.html)， [`BIT`](https://dev.mysql.com/doc/refman/8.0/en/bit-type.html)），只要他们正在使用任何标准的数字比较运算符（相较[`=`](https://dev.mysql.com/doc/refman/8.0/en/assignment-operators.html#operator_assign-equal)， [`>=`](https://dev.mysql.com/doc/refman/8.0/en/comparison-operators.html#operator_greater-than-or-equal)， [`>`](https://dev.mysql.com/doc/refman/8.0/en/comparison-operators.html#operator_greater-than)， [`<`](https://dev.mysql.com/doc/refman/8.0/en/comparison-operators.html#operator_less-than)， [`<=`](https://dev.mysql.com/doc/refman/8.0/en/comparison-operators.html#operator_less-than-or-equal)， [`<>`](https://dev.mysql.com/doc/refman/8.0/en/comparison-operators.html#operator_not-equal)/ [`!=`](https://dev.mysql.com/doc/refman/8.0/en/comparison-operators.html#operator_not-equal)，要么 [`<=>`](https://dev.mysql.com/doc/refman/8.0/en/comparison-operators.html#operator_equal-to)）。在这种情况下，任何尚未为a的值 `DOUBLE`都将强制转换为1。现在还执行强制转换注入，以比较[`DATE`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html)或 [`TIME`](https://dev.mysql.com/doc/refman/8.0/en/time.html)值与 [`DATETIME`](https://dev.mysql.com/doc/refman/8.0/en/datetime.html)值，其中，必要时将参数强制转换 为 `DATETIME`。

从MySQL 8.0.21开始，在将字符串类型与其他类型进行比较时，也会执行此类转换。被投字符串类型包括[`CHAR`](https://dev.mysql.com/doc/refman/8.0/en/char.html)， [`VARCHAR`](https://dev.mysql.com/doc/refman/8.0/en/char.html)， [`BINARY`](https://dev.mysql.com/doc/refman/8.0/en/binary-varbinary.html)， [`VARBINARY`](https://dev.mysql.com/doc/refman/8.0/en/binary-varbinary.html)， [`BLOB`](https://dev.mysql.com/doc/refman/8.0/en/blob.html)， [`TEXT`](https://dev.mysql.com/doc/refman/8.0/en/blob.html)， [`ENUM`](https://dev.mysql.com/doc/refman/8.0/en/enum.html)，和 [`SET`](https://dev.mysql.com/doc/refman/8.0/en/set.html)。将字符串类型的值与数字类型或进行比较时 `YEAR`，字符串强制转换为 `DOUBLE`; 如果其他参数的类型不是`FLOAT`，`DOUBLE`或者`REAL`，它也投来 `DOUBLE`。在将字符串类型与a `DATETIME`或`TIMESTAMP` value 进行比较时 ，字符串将强制转换为`DATETIME`; 将字符串类型与比较时`DATE`，字符串被强制转换为`DATE`。

因此能够看到通过查看的输出时石膏注入到一个给定的查询[`EXPLAIN ANALYZE`](https://dev.mysql.com/doc/refman/8.0/en/explain.html#explain-analyze)，`EXPLAIN FORMAT=JSON`或者，如下所示，`EXPLAIN FORMAT=TREE`：

```sql
  mysql> CREATE TABLE d (dt DATETIME, d DATE, t TIME);
  Query OK, 0 rows affected (0.62 sec)

  mysql> CREATE TABLE n (i INT, d DECIMAL, f FLOAT, dc DECIMAL);
  Query OK, 0 rows affected (0.51 sec)

  mysql> CREATE TABLE s (c CHAR(25), vc VARCHAR(25),
      ->     bn BINARY(50), vb VARBINARY(50), b BLOB, t TEXT,
      ->     e ENUM('a', 'b', 'c'), se SET('x' ,'y', 'z'));
  Query OK, 0 rows affected (0.50 sec)

  mysql> EXPLAIN FORMAT=TREE SELECT * from d JOIN n ON d.dt = n.i\G
  *************************** 1\. row ***************************
  EXPLAIN: -> Inner hash join (cast(d.dt as double) = cast(n.i as double))
  (cost=0.70 rows=1)
      -> Table scan on n  (cost=0.35 rows=1)
      -> Hash
          -> Table scan on d  (cost=0.35 rows=1)

  mysql> EXPLAIN FORMAT=TREE SELECT * from s JOIN d ON d.dt = s.c\G
  *************************** 1\. row ***************************
  EXPLAIN: -> Inner hash join (d.dt = cast(s.c as datetime(6)))  (cost=0.72 rows=1)
      -> Table scan on d  (cost=0.37 rows=1)
      -> Hash
          -> Table scan on s  (cost=0.35 rows=1)

  1 row in set (0.01 sec)

  mysql> EXPLAIN FORMAT=TREE SELECT * from n JOIN s ON n.d = s.c\G
  *************************** 1\. row ***************************
  EXPLAIN: -> Inner hash join (cast(n.d as double) = cast(s.c as double))  (cost=0.70 rows=1)
      -> Table scan on s  (cost=0.35 rows=1)
      -> Hash
          -> Table scan on n  (cost=0.35 rows=1)

  1 row in set (0.00 sec)
```

也可以通过执行看到这种强制类型转换`EXPLAIN [FORMAT=TRADITIONAL]`，在这种情况下，也有必要[`SHOW WARNINGS`](https://dev.mysql.com/doc/refman/8.0/en/show-warnings.html)在执行`EXPLAIN`语句之后 发出。
