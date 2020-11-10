---
title: MySQL 8.0的新增功能探索-安全性和帐户管理
date: 2020-05-26T17:56:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

- **安全性和帐户管理。** 添加了这些增强功能，以提高安全性并在帐户管理中提供更大的DBA灵活性：- `mysql`现在 ，系统数据库中的授权表是`InnoDB` （事务性）表。以前，这些是 `MyISAM`（非事务性）表。授予表存储引擎的更改是帐户管理对帐单行为的伴随更改的基础。以前，帐户管理对帐单（例如 [`CREATE USER`](https://dev.mysql.com/doc/refman/8.0/en/create-user.html)或 [`DROP USER`](https://dev.mysql.com/doc/refman/8.0/en/drop-user.html)），命名多个用户可以对某些用户成功，而对其他用户则失败。现在，每个语句都是事务性的，并且对于所有指定的用户都成功，或者回滚，如果发生任何错误，则无效。如果成功，则将语句写入二进制日志；如果失败，则不写入语句。在这种情况下，将发生回滚并且不进行任何更改。有关更多信息，请参见[第13.1.1节"原子数据定义语句支持"](https://dev.mysql.com/doc/refman/8.0/en/atomic-ddl.html)。

  - 一个新的`caching_sha2_password` 身份验证插件可用。像`sha256_password`插件一样 ， `caching_sha2_password`实现SHA-256密码哈希，但是使用缓存来解决连接时的延迟问题。它还支持更多的连接协议，并且不需要针对基于RSA密钥对的密码交换功能而针对OpenSSL进行链接。请参见 [第6.4.1.2节"缓存SHA-2可插拔身份验证"](https://dev.mysql.com/doc/refman/8.0/en/caching-sha2-pluggable-authentication.html)。

    该`caching_sha2_password`和 `sha256_password`认证插件提供比更安全的密码加密 `mysql_native_password`插件，并 `caching_sha2_password`提供了比更好的性能`sha256_password`。由于的这些优越的安全性和性能特征 `caching_sha2_password`，它现在是首选的身份验证插件，并且也是默认的身份验证插件，而不是 `mysql_native_password`。有关此默认插件更改对服务器操作以及服务器与客户端和连接器的兼容性的影响的信息，请参阅 [caching_sha2_password作为首选身份验证插件](https://dev.mysql.com/doc/refman/8.0/en/upgrading-from-previous-series.html#upgrade-caching-sha2-password)。

  - MySQL现在支持角色，这些角色被称为特权集合。可以创建和删除角色。角色可以具有授予和撤销的特权。可以向用户帐户授予角色或从用户帐户撤消角色。可以从授予该帐户的角色中选择该帐户的活动适用角色，并且可以在该帐户的会话期间进行更改。有关更多信息，请参见[第6.2.10节"使用角色"](https://dev.mysql.com/doc/refman/8.0/en/roles.html)。

  - MySQL现在合并了用户帐户类别的概念，根据系统用户和普通用户是否具有[`SYSTEM_USER`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_system-user)特权来区分它们 。请参见[第6.2.11节"帐户类别"](https://dev.mysql.com/doc/refman/8.0/en/account-categories.html)。

  - 以前，除了某些模式之外，不可能授予全局适用的特权。现在，如果[`partial_revokes`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_partial_revokes)启用了 系统变量，则可以这样做。请参见 [第6.2.12节"使用部分撤销的权限限制"](https://dev.mysql.com/doc/refman/8.0/en/partial-revokes.html)。

  - 该[`GRANT`](https://dev.mysql.com/doc/refman/8.0/en/grant.html)语句有一个子句，用于指定有关用于语句执行的特权上下文的其他信息。尽管此语法在SQL级别上可见，但其主要目的是通过使这些限制出现在二进制日志中，从而在由部分吊销施加的授予者特权限制的所有节点之间实现统一复制。请参见 [第13.7.1.6节" GRANT语句"](https://dev.mysql.com/doc/refman/8.0/en/grant.html)。 `AS *`user`* [WITH ROLE]`

  - MySQL现在维护有关密码历史记录的信息，从而可以限制重复使用以前的密码。DBA可能要求在一定数量的密码更改或一段时间内，不要从以前的密码中选择新密码。可以在全局范围内以及每个帐户基础上建立密码重用策略。

    现在可以要求通过指定要替换的当前密码来验证更改帐户密码的尝试。这使DBA可以防止用户在不证明他们知道当前密码的情况下更改密码。可以在全局范围内以及每个帐户基础上建立密码验证策略。

    现在允许帐户具有双重密码，这使得在复杂的多服务器系统中无缝执行阶段性密码更改而无需停机。

    MySQL现在使管理员能够配置用户帐户，以使由于密码错误而导致的连续登录失败过多，从而导致临时帐户锁定。每个帐户可以配置所需的失败次数和锁定时间。

    这些新功能使DBA可以更全面地控制密码管理。有关更多信息，请参见[第6.2.15节"密码管理"](https://dev.mysql.com/doc/refman/8.0/en/password-management.html)。

  - 如果使用OpenSSL进行编译，MySQL现在支持FIPS模式，并且在运行时可以使用OpenSSL库和FIPS对象模块。FIPS模式对加密操作施加了条件，例如对可接受的加密算法的限制或对更长密钥长度的要求。请参见[第6.6节" FIPS支持"](https://dev.mysql.com/doc/refman/8.0/en/fips-mode.html)。

  - 服务器用于新连接的TLS上下文现在可以在运行时重新配置。例如，此功能可能很有用，可避免重新启动一直运行已久的MySQL服务器（其SSL证书已过期）。请参阅 [服务器端运行时配置和监视加密连接](https://dev.mysql.com/doc/refman/8.0/en/using-encrypted-connections.html#using-encrypted-connections-server-side-runtime-configuration)。

  - 如果服务器和客户端均使用OpenSSL 1.1.1或更高版本进行编译，则OpenSSL 1.1.1支持TLS v1.3协议进行加密连接，而MySQL 8.0.16和更高版本也支持TLS v1.3。请参见 [第6.3.2节"加密的连接TLS协议和密码"](https://dev.mysql.com/doc/refman/8.0/en/encrypted-connection-protocols-ciphers.html)。

  - 现在，MySQL将授予命名管道上的客户端的访问控制设置为在Windows上成功进行通信所需的最低要求。较新的MySQL客户端软件无需任何其他配置即可打开命名管道连接。如果不能立即升级旧的客户端软件，[`named_pipe_full_access_group`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_named_pipe_full_access_group) 则可以使用新的 系统变量为Windows组授予打开命名管道连接所需的权限。完全访问组的成员资格应受到限制且是临时的。
