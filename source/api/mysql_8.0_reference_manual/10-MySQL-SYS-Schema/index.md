---
title: MySQL 8.0 SYS Schema
---

**Table of Contents**

- [27.1 Prerequisites for Using the sys Schema](https://dev.mysql.com/doc/refman/8.0/en/sys-schema-prerequisites.html)

- [27.2 Using the sys Schema](https://dev.mysql.com/doc/refman/8.0/en/sys-schema-usage.html)

- [27.3 sys Schema Progress Reporting](https://dev.mysql.com/doc/refman/8.0/en/sys-schema-progress-reporting.html)

- [27.4 sys Schema Object Reference](https://dev.mysql.com/doc/refman/8.0/en/sys-schema-reference.html)



MySQL 8.0 includes the [`sys`](https://dev.mysql.com/doc/refman/8.0/en/sys-schema.html) schema, a set of objects that helps DBAs and developers interpret data collected by the Performance Schema. [`sys`](https://dev.mysql.com/doc/refman/8.0/en/sys-schema.html) schema objects can be used for typical tuning and diagnosis use cases. Objects in this schema include:

- Views that summarize Performance Schema data into more easily understandable form.
- Stored procedures that perform operations such as Performance Schema configuration and generating diagnostic reports.
- Stored functions that query Performance Schema configuration and provide formatting services.

For new installations, the [`sys`](https://dev.mysql.com/doc/refman/8.0/en/sys-schema.html) schema is installed by default during data directory initialization if you use [**mysqld**](https://dev.mysql.com/doc/refman/8.0/en/mysqld.html) with the [`--initialize`](https://dev.mysql.com/doc/refman/8.0/en/server-options.html#option_mysqld_initialize) or [`--initialize-insecure`](https://dev.mysql.com/doc/refman/8.0/en/server-options.html#option_mysqld_initialize-insecure) option. If this is not desired, you can drop the [`sys`](https://dev.mysql.com/doc/refman/8.0/en/sys-schema.html) schema manually after initialization if it is unneeded.

The MySQL upgrade procedure produces an error if a [`sys`](https://dev.mysql.com/doc/refman/8.0/en/sys-schema.html) schema exists but has no [`version`](https://dev.mysql.com/doc/refman/8.0/en/sys-version.html) view, on the assumption that absence of this view indicates a user-created `sys` schema. To upgrade in this case, remove or rename the existing [`sys`](https://dev.mysql.com/doc/refman/8.0/en/sys-schema.html) schema first.

[`sys`](https://dev.mysql.com/doc/refman/8.0/en/sys-schema.html) schema objects have a `DEFINER` of `'mysql.sys'@'localhost'`. Use of the dedicated `mysql.sys` account avoids problems that occur if a DBA renames or removes the `root` account.
