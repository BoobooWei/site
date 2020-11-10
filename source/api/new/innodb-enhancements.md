---
title: MySQL 8.0的新增功能探索-InnoDB增强功能
date: 2020-05-26T17:58:00.000Z
categories:
- [MySQL8.0]
tags:
- MySQL新特性
---

**InnoDB增强功能。** 这些`InnoDB`增强功能已添加：- 每次值更改时，当前最大自动增量计数器值都会写入重做日志，并保存到每个检查点的引擎专用系统表中。这些更改使当前的最大自动增量计数器值在服务器重新启动期间保持不变。另外：

- 重新启动服务器不再取消 `AUTO_INCREMENT = N`table选项的作用。如果将自动递增计数器初始化为特定值，或者将自动递增计数器值更改为较大的值，则新值将在服务器重新启动后保留。
- 在[`ROLLBACK`](https://dev.mysql.com/doc/refman/8.0/en/commit.html) 操作之后立即重新启动服务器 不再导致重用分配给回滚事务的自动增量值。
- 如果将`AUTO_INCREMENT` 列值修改为大于当前最大自动增量值的值（[`UPDATE`](https://dev.mysql.com/doc/refman/8.0/en/update.html)例如，在一个 操作中），则将保留新值，并且后续 [`INSERT`](https://dev.mysql.com/doc/refman/8.0/en/insert.html)操作将从较大的新值开始分配自动增量值。

  有关更多信息，请参见 [第15.6.1.6节" InnoDB中的AUTO_INCREMENT处理"](https://dev.mysql.com/doc/refman/8.0/en/innodb-auto-increment-handling.html)和 [InnoDB AUTO_INCREMENT计数器初始化](https://dev.mysql.com/doc/refman/8.0/en/innodb-auto-increment-handling.html#innodb-auto-increment-initialization)。

- 遇到索引树损坏时， `InnoDB`将损坏标志写入重做日志，这会使损坏标志崩溃。`InnoDB`还将内存损坏标志数据写入每个检查点上的引擎专用系统表。在恢复期间， `InnoDB`在将内存表和索引对象标记为已损坏之前，从两个位置读取损坏标志并合并结果。

- 的`InnoDB` **memcached的**插件支持多个 `get`操作（读取在一个单一的多键-值对**分布式缓存** 查询）和范围查询。请参见 [第15.20.4节" InnoDB memcached多重获取和范围查询支持"](https://dev.mysql.com/doc/refman/8.0/en/innodb-memcached-multiple-get-range-query.html)。

- 新的动态变量 [`innodb_deadlock_detect`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_deadlock_detect)可以用于禁用死锁检测。在高并发系统上，当多个线程等待相同的锁时，死锁检测会导致速度变慢。有时，禁用死锁检测并在[`innodb_lock_wait_timeout`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_lock_wait_timeout) 发生死锁时依靠设置进行事务回滚可能会更有效 。- 新 [`INFORMATION_SCHEMA.INNODB_CACHED_INDEXES`](https://dev.mysql.com/doc/refman/8.0/en/innodb-cached-indexes-table.html) 表报告`InnoDB`每个索引在缓冲池中缓存的索引页数 。

- `InnoDB`现在在共享临时表空间中创建临时表 `ibtmp1`。

- 该`InnoDB` [表空间加密功能](https://dev.mysql.com/doc/refman/8.0/en/innodb-data-encryption.html)重做日志和撤销日志数据的支持加密。请参阅 [重做日志加密](https://dev.mysql.com/doc/refman/8.0/en/innodb-data-encryption.html#innodb-data-encryption-redo-log)和 [撤消日志加密](https://dev.mysql.com/doc/refman/8.0/en/innodb-data-encryption.html#innodb-data-encryption-undo-log)。

- `InnoDB`支持 `NOWAIT`和`SKIP LOCKED`选项`SELECT ... FOR SHARE`以及`SELECT ... FOR UPDATE`锁定读取语句。 `NOWAIT`如果请求的行被另一个事务锁定，则导致该语句立即返回。`SKIP LOCKED`从结果集中删除锁定的行。请参阅 [使用NOWAIT和SKIP LOCKED锁定读取并发](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking-reads.html#innodb-locking-reads-nowait-skip-locked)。

  `SELECT ... FOR SHARE`replaces `SELECT ... LOCK IN SHARE MODE`，但 `LOCK IN SHARE MODE`仍可用于向后兼容。这些语句是等效的。然而，`FOR UPDATE`和 `FOR SHARE`支持 `NOWAIT`，`SKIP LOCKED`和选项。请参见[第13.2.10节" SELECT语句"](https://dev.mysql.com/doc/refman/8.0/en/select.html)。 `OF *`tbl_name`*`

  `OF *`tbl_name`*` 将锁定查询应用于命名表。

- `ADD PARTITION`，`DROP PARTITION`，`COALESCE PARTITION`，`REORGANIZE PARTITION`，和`REBUILD PARTITION` [`ALTER TABLE`](https://dev.mysql.com/doc/refman/8.0/en/alter-table.html)选项由本地分区就地API的支持，可能与使用 `ALGORITHM={COPY|INPLACE}`和 `LOCK`条款。

  `DROP PARTITION`与一起 `ALGORITHM=INPLACE`删除分区中存储的数据并删除分区。但是， `DROP PARTITION`使用 `ALGORITHM=COPY`或 [`old_alter_table=ON`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_old_alter_table) 重建分区表，并尝试将数据从删除的分区移动到具有兼容`PARTITION ... VALUES` 定义的另一个分区。无法删除的数据将被删除。

- `InnoDB`现在 ，存储引擎使用MySQL数据字典，而不是其自己的特定于存储引擎的数据字典。有关数据字典的信息，请参见 [第14章，_MySQL数据字典_](https://dev.mysql.com/doc/refman/8.0/en/data-dictionary.html)。

- `mysql`现在，在MySQL数据目录中`InnoDB`命名的单个表空间文件中 创建系统表和数据字典表 `mysql.ibd`。以前，这些表是`InnoDB`在`mysql`数据库目录中的单个表空间文件中创建的。

- MySQL 8.0中引入了以下撤消表空间更改：

  - 默认情况下，撤消日志现在位于初始化MySQL实例时创建的两个撤消表空间中。撤消日志不再在系统表空间中创建。

  - 从MySQL 8.0.14开始，可以在运行时使用[`CREATE UNDO TABLESPACE`](https://dev.mysql.com/doc/refman/8.0/en/create-tablespace.html)语法在选定位置创建其他撤消表空间 。

    ```sql
    CREATE UNDO TABLESPACE tablespace_name ADD DATAFILE 'file_name.ibu';
    ```

    使用[`CREATE UNDO TABLESPACE`](https://dev.mysql.com/doc/refman/8.0/en/create-tablespace.html)语法创建的撤消表空间 可以在运行时使用[`DROP UNDO TABLESPACE`](https://dev.mysql.com/doc/refman/8.0/en/drop-tablespace.html)语法删除 。

    ```sql
    DROP UNDO TABLESPACE tablespace_name;
    ```

    [`ALTER UNDO TABLESPACE`](https://dev.mysql.com/doc/refman/8.0/en/alter-tablespace.html) 语法可用于将撤消表空间标记为活动或不活动。

    ```sql
    ALTER UNDO TABLESPACE tablespace_name SET {ACTIVE|INACTIVE};
    ```

    甲`STATE`，显示了表空间的状态列被添加到 [`INFORMATION_SCHEMA.INNODB_TABLESPACES`](https://dev.mysql.com/doc/refman/8.0/en/innodb-tablespaces-table.html) 表中。撤消表空间必须处于 `empty`状态才能被删除。

  - [`innodb_undo_log_truncate`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_undo_log_truncate) 默认情况下启用 该 变量。

  - 该 [`innodb_rollback_segments`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_rollback_segments) 变量定义每个撤消表空间的回滚段数。以前， [`innodb_rollback_segments`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_rollback_segments) 指定了MySQL实例的回滚段总数。此更改增加了可用于并发事务的回滚段的数量。更多的回滚段会增加并发事务将单独的回滚段用于撤消日志的可能性，从而减少资源争用。

- 修改了影响缓冲池预刷新和刷新行为的变量的默认值：

  - 现在 [`innodb_max_dirty_pages_pct_lwm`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_max_dirty_pages_pct_lwm) 默认值为10。先前的默认值0禁用缓冲池预刷新。当缓冲池中的脏页百分比超过10％时，值为10启用预刷新。启用预冲洗可提高性能一致性。
  - 将 [`innodb_max_dirty_pages_pct`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_max_dirty_pages_pct) 默认值从75到90。增加 `InnoDB`尝试刷新的数据从缓冲池，使脏页的百分比不超过这个值。增加的默认值允许缓冲池中脏页的百分比更高。

- 现在默认 [`innodb_autoinc_lock_mode`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_autoinc_lock_mode) 设置为2（交错）。交错锁定模式允许并行执行多行插入，从而提高了并发性和可伸缩性。新的 [`innodb_autoinc_lock_mode`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_autoinc_lock_mode) 默认设置反映了从基于语句的复制到基于行的复制的更改，这是MySQL 5.7中的默认复制类型。基于语句的复制需要连续的自动增量锁定模式（以前的默认设置），以确保按照给定的SQL语句序列以可预测和可重复的顺序分配自动增量值，而基于行的复制对SQL语句不敏感。 SQL语句的执行顺序。有关更多信息，请参见 [InnoDB AUTO_INCREMENT锁定模式](https://dev.mysql.com/doc/refman/8.0/en/innodb-auto-increment-handling.html#innodb-auto-increment-lock-modes)。

  对于使用基于语句的复制的系统，新的 [`innodb_autoinc_lock_mode`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_autoinc_lock_mode) 默认设置可能会破坏依赖于顺序自动增量值的应用程序。要恢复以前的默认设置，请设置 [`innodb_autoinc_lock_mode`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_autoinc_lock_mode) 为1。

- [`ALTER TABLESPACE ... RENAME TO`](https://dev.mysql.com/doc/refman/8.0/en/alter-tablespace.html)语法 支持重命名常规表空间 。

- [`innodb_dedicated_server`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_dedicated_server) 默认情况下禁用 的新 变量可用于`InnoDB`根据服务器上检测到的内存量自动配置以下选项：

  - [`innodb_buffer_pool_size`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_buffer_pool_size)
  - [`innodb_log_file_size`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_log_file_size)
  - [`innodb_flush_method`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_flush_method)

  此选项适用于在专用服务器上运行的MySQL服务器实例。有关更多信息，请参见 [第15.8.12节"为专用的MySQL服务器启用自动配置"](https://dev.mysql.com/doc/refman/8.0/en/innodb-dedicated-server.html)。

- 新 [`INFORMATION_SCHEMA.INNODB_TABLESPACES_BRIEF`](https://dev.mysql.com/doc/refman/8.0/en/innodb-tablespaces-brief-table.html) 视图为表空间提供空间，名称，路径，标志和空间类型数据`InnoDB`。

- 与MySQL捆绑在一起 的[zlib库](http://www.zlib.net/)版本从1.2.3版本提高到1.2.11版本。MySQL在zlib库的帮助下实现了压缩。

  如果使用`InnoDB`压缩表，请参见[第2.11.4节" MySQL 8.0中的更改"](https://dev.mysql.com/doc/refman/8.0/en/upgrading-from-previous-series.html)以获取相关的升级含义。

- `InnoDB`除全局临时表空间和撤消表空间文件外 ，所有表空间文件中都存在序列化字典信息（SDI）。SDI是表和表空间对象的序列化元数据。SDI数据的存在提供了元数据冗余。例如，如果数据字典变得不可用，则可以从表空间文件中提取字典对象元数据。使用[**ibd2sdi**](https://dev.mysql.com/doc/refman/8.0/en/ibd2sdi.html)工具执行SDI提取。SDI数据以`JSON`格式存储。

  在表空间文件中包含SDI数据会增加表空间文件的大小。SDI记录需要一个索引页，默认情况下大小为16KB。但是，在存储SDI数据时会对其进行压缩，以减少存储空间。

- `InnoDB`现在 ，存储引擎支持原子DDL，即使服务器在操作期间停止运行，它也可以确保DDL操作完全提交或回退。有关更多信息，请参见[第13.1.1节"原子数据定义语句支持"](https://dev.mysql.com/doc/refman/8.0/en/atomic-ddl.html)。

- 使用该[`innodb_directories`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_directories) 选项，在服务器脱机时，可以将表空间文件移动或还原到新位置 。有关更多信息，请参见 [第15.6.3.6节"在服务器脱机时移动表空间文件"](https://dev.mysql.com/doc/refman/8.0/en/innodb-moving-data-files-offline.html)。

- 实现了以下重做日志优化：

  - 用户线程现在可以并发写入日志缓冲区，而无需同步写入。
  - 用户线程现在可以按轻松的顺序将脏页添加到刷新列表中。
  - 现在，专用的日志线程负责将日志缓冲区写入系统缓冲区，将系统缓冲区刷新到磁盘，通知用户线程有关已写入和已刷新的重做，保持松弛的刷新列表顺序所需的滞后时间，以及写入检查点。
  - 添加了系统变量，用于配置等待刷新重做的用户线程使用旋转延迟的方法：

    - [`innodb_log_wait_for_flush_spin_hwm`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_log_wait_for_flush_spin_hwm)：定义最大平均日志刷新时间，超过该时间后，用户线程将在等待刷新重做时不再旋转。
    - [`innodb_log_spin_cpu_abs_lwm`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_log_spin_cpu_abs_lwm)：定义最小CPU使用量，在该最小使用量之下，用户线程在等待刷新重做时不再旋转。
    - [`innodb_log_spin_cpu_pct_hwm`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_log_spin_cpu_pct_hwm)：定义最大CPU使用量，在该最大CPU使用量之上，用户线程在等待刷新重做时不再旋转。

  - 的 [`innodb_log_buffer_size`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_log_buffer_size) 变量是现在动态的，服务器运行时其允许调整大小日志缓冲区的。

  有关更多信息，请参见 [第8.5.4节"优化InnoDB重做日志"](https://dev.mysql.com/doc/refman/8.0/en/optimizing-innodb-logging.html)。

- 从MySQL 8.0.12开始，对大对象（LOB）数据的小更新支持撤消日志记录，从而提高了大小为100字节或更小的LOB更新的性能。以前，LOB更新的大小至少为一个LOB页，对于可能只修改几个字节的更新而言，这不是最佳的。此增强功能基于MySQL 8.0.4中添加的对LOB数据的部分更新的支持。

- 从MySQL 8.0.12开始，`ALGORITHM=INSTANT` 以下[`ALTER TABLE`](https://dev.mysql.com/doc/refman/8.0/en/alter-table.html)操作受支持 ：

  - 添加一列。此功能也称为 " 即时`ADD COLUMN` "。有限制条件。请参见 [第15.12.1节"在线DDL操作"](https://dev.mysql.com/doc/refman/8.0/en/innodb-online-ddl-operations.html)。
  - 添加或删除虚拟列。
  - 添加或删除列默认值。
  - 修改[`ENUM`](https://dev.mysql.com/doc/refman/8.0/en/enum.html)或 [`SET`](https://dev.mysql.com/doc/refman/8.0/en/set.html)列的定义 。
  - 更改索引类型。
  - 重命名表。

  `ALGORITHM=INSTANT`仅 支持的操作会 修改数据字典中的元数据。在表上没有采取任何元数据锁，并且表数据不受影响，从而使操作立即进行。如果未明确指定，`ALGORITHM=INSTANT`则默认情况下由支持它的操作使用。如果 `ALGORITHM=INSTANT`指定但不支持，则操作立即失败并显示错误。

  有关支持的操作的更多信息 `ALGORITHM=INSTANT`，请参见 [第15.12.1节"在线DDL操作"](https://dev.mysql.com/doc/refman/8.0/en/innodb-online-ddl-operations.html)。

- 从MySQL 8.0.13开始，`TempTable` 存储引擎支持二进制大对象（BLOB）类型列的存储。此增强功能提高了使用包含BLOB数据的临时表的查询的性能。以前，包含BLOB数据的临时表存储在定义的磁盘存储引擎中 [`internal_tmp_disk_storage_engine`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_internal_tmp_disk_storage_engine)。有关更多信息，请参见 [第8.4.4节" MySQL中的内部临时表使用"](https://dev.mysql.com/doc/refman/8.0/en/internal-temporary-tables.html)。

- 从MySQL 8.0.13开始，静态`InnoDB` 数据加密功能支持常规表空间。以前，只能加密每表文件表空间。一般的表空间支持加密，[`CREATE TABLESPACE`](https://dev.mysql.com/doc/refman/8.0/en/create-tablespace.html)和[`ALTER TABLESPACE`](https://dev.mysql.com/doc/refman/8.0/en/alter-tablespace.html)语法扩展到包括 `ENCRYPTION`条款。

  该 [`INFORMATION_SCHEMA.INNODB_TABLESPACES`](https://dev.mysql.com/doc/refman/8.0/en/innodb-tablespaces-table.html) 表现在包括一`ENCRYPTION` 列，该列指示表空间是否已加密。

  在`stage/innodb/alter tablespace (encryption)`加入绩效模式阶段仪器允许监测一般表的加密操作。

- 禁用该 `innodb_buffer_pool_in_core_file` 变量可通过排除`InnoDB`缓冲池页面来减少核心文件的大小 。要使用此变量，[`core_file`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_core_file) 必须启用该变量，并且操作系统必须支持Linux 3.4及更高版本支持的的`MADV_DONTDUMP`非POSIX扩展`madvise()`。有关更多信息，请参见[第15.8.3.7节"从核心文件中排除缓冲池页面"](https://dev.mysql.com/doc/refman/8.0/en/innodb-buffer-pool-in-core-file.html)。

- 从MySQL 8.0.13开始，由用户创建的临时表和由优化程序创建的内部临时表存储在会话临时表空间中，该会话临时表空间是从临时表空间池中分配给会话的。当会话断开连接时，其临时表空间将被截断并释放回池中。在以前的版本中，临时表是在全局临时表空间（`ibtmp1`）中创建的，在删除临时表后，该临时表不会将磁盘空间返回给操作系统。

  该 [`innodb_temp_tablespaces_dir`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_temp_tablespaces_dir) 变量定义创建会话临时表空间的位置。默认位置是 `#innodb_temp`数据目录中的目录。

  该 [`INNODB_SESSION_TEMP_TABLESPACES`](https://dev.mysql.com/doc/refman/8.0/en/innodb-session-temp-tablespaces-table.html) 表提供有关会话临时表空间的元数据。

  现在，全局临时表空间（`ibtmp1`）存储了回滚段，用于对用户创建的临时表所做的更改。

- 从MySQL 8.0.14开始，`InnoDB`支持并行聚集索引读取，这可以提高 [`CHECK TABLE`](https://dev.mysql.com/doc/refman/8.0/en/check-table.html)性能。此功能不适用于二级索引扫描。的 [`innodb_parallel_read_threads`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_parallel_read_threads) 并行聚簇索引读取发生会话变量必须被设置为一个大于1的值。默认值为4。用于执行并行聚集索引读取的实际线程数取决于 [`innodb_parallel_read_threads`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_parallel_read_threads) 设置或要扫描的索引子树的数量，以较小者为准。

- 从8.0.14开始，[`innodb_dedicated_server`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_dedicated_server) 启用该 变量后，将根据自动配置的缓冲池大小来配置日志文件的大小和数量。以前，日志文件的大小是根据在服务器上检测到的内存量来配置的，而日志文件的数量不是自动配置的。

- 从8.0.14开始，该 语句的`ADD DATAFILE`子句[`CREATE TABLESPACE`](https://dev.mysql.com/doc/refman/8.0/en/create-tablespace.html)是可选的，它允许没有[`FILE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_file)特权的用户 创建表空间。一个[`CREATE TABLESPACE`](https://dev.mysql.com/doc/refman/8.0/en/create-tablespace.html)没有执行的语句 `ADD DATAFILE`子句隐式地创建一个独特的文件名的表空间的数据文件。

- 默认情况下，当TempTable存储引擎占用的内存量超过该[`temptable_max_ram`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_temptable_max_ram) 变量定义的内存限制时 ，TempTable存储引擎将开始从磁盘分配内存映射的临时文件。从MySQL 8.0.16开始，此行为由[`temptable_use_mmap`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_temptable_use_mmap) 变量控制 。禁用 [`temptable_use_mmap`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_temptable_use_mmap) 会导致TempTable存储引擎将 `InnoDB`磁盘内部临时表而不是内存映射文件用作其溢出机制。有关更多信息，请参阅 [内部临时表存储引擎](https://dev.mysql.com/doc/refman/8.0/en/internal-temporary-tables.html#internal-temporary-tables-engines)。

- 从MySQL 8.0.16开始，静态`InnoDB` 数据加密功能支持对`mysql`系统表空间的加密。该 `mysql`系统表空间包含 `mysql`系统数据库和MySQL数据字典表。有关更多信息，请参见 [第15.13节" InnoDB静态数据加密"](https://dev.mysql.com/doc/refman/8.0/en/innodb-data-encryption.html)。

- [`innodb_spin_wait_pause_multiplier`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_spin_wait_pause_multiplier) MySQL 8.0.16中引入 的 变量提供了对自旋锁轮询延迟持续时间的更好控制，自旋锁轮询延迟是在线程等待获取互斥量或rw锁时发生的。可以对延迟进行更精细的调整，以解决不同处理器体系结构上PAUSE指令持续时间的差异。有关更多信息，请参见 [第15.8.8节"配置自旋锁定轮询"](https://dev.mysql.com/doc/refman/8.0/en/innodb-performance-spin_lock_polling.html)。

- `InnoDB` 在MySQL 8.0.17中，通过更好地利用读取线程，减少了并行扫描期间发生的预取活动的读取线程I / O来改善了大型数据集的并行读取线程性能，并支持了分区的并行扫描。

  并行读取线程功能由[`innodb_parallel_read_threads`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_parallel_read_threads) 变量控制 。现在，最大设置为256，这是所有客户端连接的线程总数。如果达到线程限制，连接将退回到使用单个线程。

- [`innodb_idle_flush_pct`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_idle_flush_pct) MySQL 8.0.18中引入 的 变量允许限制空闲期间的页面刷新，这可以帮助延长固态存储设备的寿命。请参见 [在空闲期间限制缓冲区刷新](https://dev.mysql.com/doc/refman/8.0/en/innodb-buffer-pool-flushing.html#innodb-limit-flushing-rate)。

- `InnoDB`从MySQL 8.0.19开始，为了生成直方图统计数据，已经 对数据进行了有效采样。请参见 [直方图统计分析](https://dev.mysql.com/doc/refman/8.0/en/analyze-table.html#analyze-table-histogram-statistics-analysis)。

- 从MySQL 8.0.20开始，doublewrite缓冲区存储区位于doublewrite文件中。在以前的版本中，存储区位于系统表空间中。将存储区域移出系统表空间可减少写延迟，增加吞吐量并提供关于双写缓冲区页放置的灵活性。为高级双写缓冲区配置引入了以下系统变量：

  - [`innodb_doublewrite_dir`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_doublewrite_dir)

    定义双写缓冲区文件目录。

  - [`innodb_doublewrite_files`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_doublewrite_files)

    定义双写文件的数量。

  - [`innodb_doublewrite_pages`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_doublewrite_pages)

    定义批量写入时每个线程的最大双写页数。

  - [`innodb_doublewrite_batch_size`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_doublewrite_batch_size)

    定义要批量写入的双写页面数。

  有关更多信息，请参见 [第15.6.4节" Doublewrite缓冲区"](https://dev.mysql.com/doc/refman/8.0/en/innodb-doublewrite-buffer.html)。

- MySQL 8.0.20中改进了竞争感知事务调度（CATS）算法，该算法优先考虑等待锁的事务。现在，事务调度权重计算完全在单独的线程中执行，从而提高了计算性能和准确性。

  删除了也用于事务调度的先进先出（FIFO）算法。CATS算法的增强使FIFO算法变得多余。以前由FIFO算法执行的事务调度现在由CATS算法执行。

  `TRX_SCHEDULE_WEIGHT`在`INFORMATION_SCHEMA.INNODB_TRX`表中添加了 一个列 ，该列允许查询由CATS算法分配的事务调度权重。

  `INNODB_METRICS`添加了 以下计数器来监视代码级事务调度事件：

  - `lock_rec_release_attempts`

    尝试释放记录锁定的次数。

  - `lock_rec_grant_attempts`

    授予记录锁定的尝试次数。

  - `lock_schedule_refreshes`

    分析等待图表以更新交易计划权重的次数。

  有关更多信息，请参见 [第15.7.6节"事务调度"](https://dev.mysql.com/doc/refman/8.0/en/innodb-transaction-scheduling.html)。
