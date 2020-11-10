---
title: MySQL 8.0的新增功能探索-用户注释和用户属性
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

- **用户评论和用户属性。** MySQL 8.0.21引入了在创建或更新用户帐户时设置用户注释和用户属性的功能。用户注释包括作为参数传递`COMMENT`给与[`CREATE USER`](https://dev.mysql.com/doc/refman/8.0/en/create-user.html)or [`ALTER USER`](https://dev.mysql.com/doc/refman/8.0/en/alter-user.html)语句一起使用的子句的任意文本。用户属性由JSON对象形式的数据组成，该数据作为参数传递给与`ATTRIBUTE`这两个语句之一一起使用的 子句。该属性可以包含JSON对象表示法中的任何有效键值对。仅一个 `COMMENT`或`ATTRIBUTE` 可以在单一使用`CREATE USER`或 `ALTER USER` 声明。

  用户注释和用户属性在内部作为JSON对象存储在一起，注释文本作为元素的值`comment`作为其键。可以从 表的`ATTRIBUTE`列中 检索此信息 [`INFORMATION_SCHEMA.USER_ATTRIBUTES`](https://dev.mysql.com/doc/refman/8.0/en/user-attributes-table.html)。由于它是JSON格式，因此您可以使用MySQL的JSON函数和运算符来解析其内容（请参见 [第12.17节" JSON函数"](https://dev.mysql.com/doc/refman/8.0/en/json-functions.html)）。使用该[`JSON_MERGE_PATCH()`](https://dev.mysql.com/doc/refman/8.0/en/json-modification-functions.html#function_json-merge-patch) 功能时，对用户属性的连续更改将与其当前值合并。

  例：

  ```sql
  mysql> CREATE USER 'mary'@'localhost' COMMENT 'This is Mary Smith\'s account';
  Query OK, 0 rows affected (0.33 sec)

  mysql> ALTER USER 'mary'@'localhost'
      -≫     ATTRIBUTE '{"fname":"Mary", "lname":"Smith"}';
  Query OK, 0 rows affected (0.14 sec)

  mysql> ALTER USER 'mary'@'localhost'
      -≫     ATTRIBUTE '{"email":"mary.smith@example.com"}';
  Query OK, 0 rows affected (0.12 sec)

  mysql> SELECT
      ->    USER,
      ->    HOST,
      ->    ATTRIBUTE->>"$.fname" AS 'First Name',
      ->    ATTRIBUTE->>"$.lname" AS 'Last Name',
      ->    ATTRIBUTE->>"$.email" AS 'Email',
      ->    ATTRIBUTE->>"$.comment" AS 'Comment'
      -> FROM INFORMATION_SCHEMA.USER_ATTRIBUTES
      -> WHERE USER='mary' AND HOST='localhost'\G
  *************************** 1\. row ***************************
        USER: mary
        HOST: localhost
  First Name: Mary
   Last Name: Smith
       Email: mary.smith@example.com
     Comment: This is Mary Smith's account
  1 row in set (0.00 sec)
  ```

  有关更多信息和示例，请参见 [第13.7.1.3节" CREATE USER语句"](https://dev.mysql.com/doc/refman/8.0/en/create-user.html)，[第13.7.1.1节" ALTER USER语句"](https://dev.mysql.com/doc/refman/8.0/en/alter-user.html)和[第25.42节" INFORMATION_SCHEMA USER_ATTRIBUTES表"](https://dev.mysql.com/doc/refman/8.0/en/user-attributes-table.html)。
