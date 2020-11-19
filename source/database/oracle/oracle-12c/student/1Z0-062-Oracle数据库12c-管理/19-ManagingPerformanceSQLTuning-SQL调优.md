---
title: Oracle数据库12c-管理 实践19:管理性能之SQL调优
---

> **Practices for Lesson 19: Managing Performance: SQL Tuning**
>
> 2020.01.29 BoobooWei

<!-- MDTOC maxdepth:2 firsth1:1 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

- [实践19:管理性能之SQL调优](#实践19管理性能之sql调优)   
   - [实践19:概览](#实践19概览)   
   - [实践19-1:使用自动SQL调优](#实践19-1使用自动sql调优)   
   - [实践19-2:收集扩展的统计信息](#实践19-2收集扩展的统计信息)   
   - [实践19-3:为特定工作负载检测有用的列组](#实践19-3为特定工作负载检测有用的列组)   
   - [实践19-4:创建在工作负载监视期间检测到的列组](#实践19-4创建在工作负载监视期间检测到的列组)   

<!-- /MDTOC -->

## 实践19:概览

Practices for Lesson 19: Overview

By default, Automatic SQL Tuning executes automatically during each nightly maintenance window. For this practice, you simulate the execution of Automatic SQL Tuning, and explore its results.

默认情况下，在每个夜间维护窗口期间自动执行SQL调优。对于这个实践，您将模拟自动SQL调优的执行，并研究其结果。

## 实践19-1:使用自动SQL调优

Practice 19-1: Using Automatic SQL Tuning

### Overview

In this practice, you manually launch Automatic SQL Tuning to automatically tune a small application workload. You then investigate the outcome and configuration possibilities.

**Assumptions**

ADMIN Super Administrator user has been created in Enterprise Manager Cloud Control.

DBA1 user with SYSDBA privileges has been created in orcl database.

### Task

1. In Cloud Control, Configure the Automatic SQL Tuning Task to Implement SQL Profiles Automatically.

   | **Step** | **Window/Page Description** | **Choices or Values**                                        |
   | -------- | --------------------------- | ------------------------------------------------------------ |
   | a.       | EMCC Cloud Control          | Login as **ADMIN** user.                                     |
   | b.       | Summary page                | Navigate to the **orcl** database home page.                 |
   | c.       | orcl database home          | Click **Administration > Oracle Scheduler > Automated Maintenance Tasks**. |
   | d.       | Database Login              | Login with **S****YSDBA** credentials to database.  Use DBA1/oracle_4U AS SYSDBA |
   | e.       | Automated Maintenance Tasks | Verify Status is **Enabled.**  Click **Configure.**          |

   | **Step** | **Window/Page Description**                | **Choices or Values**                              |
   | -------- | ------------------------------------------ | -------------------------------------------------- |
   | f.       | Automated Maintenance Tasks  Configuration | Click **Configure**  next to Automatic SQL Tuning. |

   | **Step** | **Window/Page Description**    | **Choices or Values**                                        |
   | -------- | ------------------------------ | ------------------------------------------------------------ |
   | g.       | Automatic SQL Tuning  Settings | Click **Yes** for Automatic Implementation of  SQL Profiles. Click **Show SQL.** |

   | **Step** | **Window/Page Description** | **Choices or Values**                      |
   | -------- | --------------------------- | ------------------------------------------ |
   | h.       | Show SQL                    | View the SQL statement.  Click **Return.** |

   | **Step** | **Window/Page Description**    | **Choices or Values** |
   | -------- | ------------------------------ | --------------------- |
   | i.       | Automatic SQL Tuning  Settings | Click **Apply.**      |

2. Review and execute the **$LABS/P19/ast_setup.sh** script .This script creates the AST user, turns off automatic maintenance tasks, and drops any existing profiles on queries executed by the AST user.

3. Execute the **ast_workload_stream.sh** script. This script executes a query that is not correctly optimized multiple times. The query in question uses hints that force the optimizer to pick a suboptimal execution plan. The script executes for approximately 60 seconds.

4. Automatic SQL Tuning is implemented by using an automated task that runs during maintenance windows. However, you are not going to wait for the next maintenance window to open. This might take too long. Instead, you will force the opening of your next maintenance window now. This will automatically trigger the Automatic SQL Tuning task. Review and execute the **ast_run.sh** script to do that. It takes about 10 minutes for the script to execute.

5. Execute the **ast_workload_stream.sh** script again. What do you observe?

   You should see that the execution time for ast_workload_stream.sh is much faster than the original execution. This is probably due to the fact that Automatic SQL Tuning implemented a profile for your statement automatically.

6. Log in as the AST user and force the creation of an AWR snapshot.

7. How can you confirm that a SQL Profile was automatically implemented?

   a.  In Enterprise Manager Cloud Control, navigate to **Administration > Oracle Scheduler>Automated Maintenance Tasks.**

   b.  Click **Automatic SQL Tuning**.

   c.  On the Automatic SQL Tuning Result Summary page, view the tuning results.

   d.  Look at the graphs on the Automatic SQL Tuning Result Summary page. (If you do not see any graphs, return to step 5, execute the work load twice, and then continue with step 6 and 7.)

   e.  Focus on understanding the pie chart and the bar graph next to it. You should be able to get a sense of the general findings, as well as the number of SQL profiles implemented by the task.

   f.   In the Summary Time Period section, Click **View Report** to see a detailed SQL-level report.                                                                                                                                                                                                                                     g.  Find and select the SQL statement that ran in the AST schema.

   **Note:** The Thumbs Up icon means that the profile was implemented.

   h.  Click **View Recommendations**.

   i.   Click the **Compare Explain Plans** eyeglass icon for the SQL Profile entry.

    j.   Scroll down the page.

   k.  Look at the old and new explain plans for the query.

   l.   Then click the “**Recommendations for SQL ID**” locator link (the last of the breadcrumbs on top of the page) to return to the previous screen.

   m.  Investigate a SQL profile. While still on the “Recommendations for SQL_ID” page, click the **SQL Text** link to go to the SQL Details page for this SQL.

   n.  On the SQL Details – Tuning History page note the link to SYS_AUTO_SQL_TUNING_TASK that is there to show that the SQL was tuned by this tuning task.

   o.  Click the **Plan Control** tab.

   p.  Note that a profile was created automatically for this SQL. The type of AUTO means it was automatically created.

   q.  Click the **Statistics** tab to take a look at the execution history for this SQL.

   r.   Select one of the plan hash values from the pull down Plan Hash Values. What is the time of the execution, and Elapsed Time per Execution?

   s.  Select the other plan hash value from the pull down Plan Hash Values. What is the time of the execution, and Elapsed Time per Execution?

   t.   Which of the two executed first? Which one executed more quickly?

   *The hash value 4005616876 in the example executed first, and the second hash value executed more quickly.*

   u.  Select All in the Plan Hash Values. This shows the improved plan and the original in the same graph. The bar graph for the second run with the SQL Profile applied may be so small as to be almost invisible.

8. Generate a text report for more in-depth information. From the command line, execute the **ast_task_report.sh** script. What do you observe?

   a.  Notice the first queries that fetch the execution name and object number from the advisor schema, followed by the final query that gets the text report. In the text report, look for the section about the SQL profile finding and peruse the Validation Results section. This shows you the execution statistics observed during test-execute and allows you to get a better sense of the profile’s quality. You can also use the report_auto_tuning_task API to get reports that span multiple executions of the task.

9. Investigate configuring Automatic SQL Tuning with Cloud Control.

   a.  While you are logged in to the **orcl** database target as the **D****BA1** user, navigate to **Administration > Oracle Scheduler > Automated Maintenance Tasks**.

   b.  The chart shows times in the past when each client was executed, and times in the future when they are scheduled to run again.

   c.  Select **7 days** in the Interval menu and click **Go** to see an entire week’s worth of data.

   d.  Click the **Configure** button. On the Automated Maintenance Tasks Configuration page, you can disable individual clients and change which windows they run in.

   e.  Disable the Automatic SQL Tuning client entirely and click **Show SQL**.

   f.   Review the command and then click **Return**.

   g.  On the Automated Maintenance Tasks Configuration page, click **Apply**. You should receive a success message.

   h.  Click the **Automated Maintenance Tasks** locator link at the top of the page i.   Notice the forbidden sign right next to the task name.

   j.   Click **Configure**.

   k.  **Enable** the Automatic SQL Tuning task.

   l.   Optionally, click Show SQL, review the commands and then click **Return**.

   m.  Click **Apply** to enable Automatic SQL Tuning. You should receive a success message. n.   Navigate to the Automatic SQL Tuning Settings page. If you are on the Automated Maintenance Tasks Configuration page, click the **Configure** button for Automatic SQL Tuning.

   o.  On the Automatic SQL Tuning Settings page, select **No** beside the “Automatic

   Implementation of SQL Profiles” field, and click **Show SQL**.

   p.  Review the command, click **Return**, and then click **Apply**. You should receive a success message.

10. OPTIONAL: Review the ast_manual_config.sh script to understand how you can configure Automatic SQL Tuning by using PL/SQL.   



### Practice

### KnowledgePoint

## 实践19-2:收集扩展的统计信息

### Overview

列组统计信息如何使优化器给出更准确的基数估计

### Task

### Practice

该`DBA_TAB_COL_STATISTICS`表的以下查询显示有关在列上ORDER_DATE和ORDER_ID从`OE.ORDERS`表中收集的统计信息的信息：

```sql
COL COLUMN_NAME FORMAT a20
COL NDV FORMAT 999
SELECT COLUMN_NAME, NUM_DISTINCT AS "NDV", HISTOGRAM
FROM   DBA_TAB_COL_STATISTICS
WHERE  OWNER = 'OE'
AND    TABLE_NAME = 'ORDERS'
AND    COLUMN_NAME IN ('ORDER_MODE', 'ORDER_ID');
```

示例输出如下：

```sql
COLUMN_NAME	      NDV HISTOGRAM
-------------------- ---- ---------------
ORDER_ID	      105 NONE
ORDER_MODE		2 FREQUENCY
```

考虑一个有针对性查询的解释计划：

```sql
EXPLAIN PLAN FOR
  SELECT *
  FROM   sh.customers
  WHERE  cust_state_province = 'CA'
  AND    country_id=52790;
```

查看执行结果

```sql
SQL> SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY);

PLAN_TABLE_OUTPUT
--------------------------------------------------------------------------------
Plan hash value: 4043159647

--------------------------------------------------------------------------------
--------

| Id  | Operation		    | Name     | Rows  | Bytes | Cost (%CPU)| Ti
me     |

--------------------------------------------------------------------------------
--------


PLAN_TABLE_OUTPUT
--------------------------------------------------------------------------------
|   0 | SELECT STATEMENT	    |	       |     1 |    37 |     1	 (0)| 00
:00:01 |

|*  1 |  TABLE ACCESS BY INDEX ROWID| ORDERS   |     1 |    37 |     1	 (0)| 00
:00:01 |

|*  2 |   INDEX UNIQUE SCAN	    | ORDER_PK |     1 |       |     0	 (0)| 00
:00:01 |

--------------------------------------------------------------------------------
--------

PLAN_TABLE_OUTPUT
--------------------------------------------------------------------------------


Predicate Information (identified by operation id):
---------------------------------------------------

   1 - filter("ORDER_MODE"='direct')
   2 - access("ORDER_ID"=2458)

15 rows selected.

```



### KnowledgePoint

管理扩展统计

`DBMS_STATS`使您可以收集**扩展的统计信息**，当表的不同列上存在多个谓词或谓词使用表达式时，这些统计信息可以改善基数估计。

的**扩展**可以是一个列组或表达。当来自同一表的多个列一起出现在SQL语句中时，列组统计信息可以改善基数估计。当谓词使用表达式（例如内置或用户定义的函数）时，表达式统计信息可改进优化程序的估计。

注意：您不能在虚拟列上创建扩展统计信息。

管理列组统计信息

**列组**是一组将被视为一个单元的列。

本质上，列组是虚拟列。通过收集列组的统计信息，当查询将这些列组合在一起时，优化器可以更准确地确定基数估计。

以下各节概述了[列组统计信息](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/tgsql/glossary.html#GUID-FB763B43-B5C7-47D0-8B0D-47CED4922501)，并说明了如何手动管理它们：

- [关于列组](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/tgsql/managing-extended-statistics.html#GUID-858E0BDA-06C6-471F-B9E8-888C3AD29673)
  统计信息单个列统计信息对于确定`WHERE`子句中单个谓词的选择性很有用。
- [检测特定工作负载的有用列组](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/tgsql/managing-extended-statistics.html#GUID-E1F39134-24F1-4EF7-B614-82F9428CA762)
  您可以使用`DBMS_STATS.SEED_COL_USAGE`并`REPORT_COL_USAGE`基于指定的工作负载来确定表需要哪些列组。
- [创建在工作负载监视期间检测到的列组](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/tgsql/managing-extended-statistics.html#GUID-783A8687-EDB3-437E-AE99-3F12369BA10A)
  您可以使用该`DBMS_STATS.CREATE_EXTENDED_STATS`功能创建以前通过执行检测到的列组`DBMS_STATS.SEED_COL_USAGE`。
- [手动创建和收集列组的统计信息](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/tgsql/managing-extended-statistics.html#GUID-D070C3B3-C356-4630-8EB3-766A790866F3)
  在某些情况下，您可能知道要创建的列组。
- [显示列组信息](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/tgsql/managing-extended-statistics.html#GUID-532D06C9-C6A2-4018-B496-AD87003B682E)
  要获取[列组](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/tgsql/managing-extended-statistics.html#GUID-532D06C9-C6A2-4018-B496-AD87003B682E)的名称，请使用`DBMS_STATS.SHOW_EXTENDED_STATS_NAME`函数或数据库视图。
- [删除列组](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/tgsql/managing-extended-statistics.html#GUID-DBBB6C91-4450-457D-A612-71E835D6159C)
  使用此`DBMS_STATS.DROP_EXTENDED_STATS`功能可以从表中删除列组。

也可以看看：

[Oracle Database PL / SQL软件包和类型参考](https://www.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/12.2/tgsql&id=ARPLS059)以了解该`DBMS_STATS`软件包

| 计划单位或偏好               | 描述                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| `SEED_COL_USAGE` 程序        | 遍历指定工作负载中的SQL语句，进行编译，然后为出现在这些语句中的列播种列使用情况信息。为了确定适当的列组，数据库必须遵守代表性的工作负载。在监视期间，您不需要自己运行查询。相反，您可以`EXPLAIN PLAN`在工作负载中运行一些运行时间更长的查询，以确保数据库正在记录这些查询的列组信息。 |
| `REPORT_COL_USAGE` 功能      | 生成一个报告，该报告列出`GROUP BY`在工作负载中的过滤谓词，连接谓词和子句中看到的列。您可以使用此功能来查看为特定表记录的列使用情况信息。 |
| `CREATE_EXTENDED_STATS` 功能 | 创建扩展名，扩展名可以是列组或表达式。当用户生成或自动统计信息收集作业收集表的统计信息时，数据库将收集扩展的统计信息。 |
| `AUTO_STAT_EXTENSIONS` 偏爱  | 收集优化程序统计信息时，控制扩展的自动创建，包括列组。使用此设置偏好`SET_TABLE_PREFS`，`SET_SCHEMA_PREFS`或`SET_GLOBAL_PREFS`。当`AUTO_STAT_EXTENSIONS`设置为`OFF`（默认）时，数据库不会自动创建列组统计信息。要创建扩展，您必须执行`CREATE_EXTENDED_STATS`函数或`METHOD_OPT`在`DBMS_STATS`API 的参数中显式指定扩展统计信息。设置为时`ON`，SQL计划伪指令可以根据工作负载中谓词中列的使用情况自动触发创建列组统计信息。 |

也可以看看：

- “ [为表设置人工优化器统计信息](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/tgsql/controlling-the-use-of-optimizer-statistics.html#GUID-D73DB2F4-46C1-4C5B-94DD-DE1F1DD5BBCC) ”
- [Oracle Database PL / SQL软件包和类型参考](https://www.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/12.2/tgsql&id=ARPLS059)以了解该`DBMS_STATS`软件包
- [Oracle数据库SQL语言参考](https://www.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/12.2/tgsql&id=SQLRF54467)中有关虚拟列限制的列表

## 实践19-3:为特定工作负载检测有用的列组

### Overview

本教程假定以下内容：

- 对于使用引用列和的谓词`oe.customers_test`的`customers`表查询（从表创建），基数估计不正确。 `country_id``cust_state_province`
- 您希望数据库在5分钟（300秒）内监视您的工作负载。
- 您希望数据库确定自动需要哪些列组。

### Task

### Practice

您可以使用`DBMS_STATS.SEED_COL_USAGE`和`REPORT_COL_USAGE`根据指定的工作负荷来确定表需要哪些列组。

当您不知道要创建哪个扩展统计信息时，此技术很有用。此技术不适用于表达式统计。

假设条件

本教程假定以下内容：

- 对于使用引用列和的谓词`sh.customers_test`的`customers`表查询（从表创建），基数估计不正确。
- 您希望数据库在5分钟（300秒）内监视您的工作负载。
- 您希望数据库确定自动需要哪些列组。

要检测列组：

1. 启动SQL * Plus或SQL Developer，然后以user身份登录数据库`sh`。

2. 创建`customers_test`表并为其收集统计信息：

   ```sql
   conn oe/oe@emrep;
   DROP TABLE customers_test;
   CREATE TABLE customers_test AS SELECT * FROM customers;
   EXEC DBMS_STATS.GATHER_TABLE_STATS(user, 'customers_test');  
   ```

3. 启用工作负载监视。

   在另一个SQL * Plus会话中，连接为`SYS`并运行以下PL / SQL程序以启用监视300秒：

   ```sql
   conn sys/WLS3Gg5_2@emrep as sysdba
   BEGIN
     DBMS_STATS.SEED_COL_USAGE(null,null,300);
   END;
   /
   ```

4. 以用户`oe`身份，运行工作负载中两个查询的解释计划。

   以下示例显示了`customers_test`表中两个查询的解释计划：

   ```sql
   conn oe/oe@emrep;
   EXPLAIN PLAN FOR
     SELECT *
     FROM   customers_test
     WHERE  CUST_EMAIL = 'Charlotte.Buckley@PINTAIL.EXAMPLE.COM'
     AND    GENDER = 'F'
     AND    ACCOUNT_MGR_ID = 149;

   SELECT PLAN_TABLE_OUTPUT
   FROM   TABLE(DBMS_XPLAN.DISPLAY('plan_table', null,'basic rows'));

   EXPLAIN PLAN FOR
     SELECT   ACCOUNT_MGR_ID, GENDER, count(*)
     FROM     customers_test
     GROUP BY ACCOUNT_MGR_ID, GENDER;

   SELECT PLAN_TABLE_OUTPUT
   ```

   输出示例如下：

   ```sql
   PLAN_TABLE_OUTPUT
   --------------------------------------------------------------------------------
   Plan hash value: 2112738156

   ----------------------------------------------------
   | Id  | Operation	  | Name	   | Rows  |
   ----------------------------------------------------
   |   0 | SELECT STATEMENT  |		   |	 1 |
   |   1 |  TABLE ACCESS FULL| CUSTOMERS_TEST |	 1 |
   ----------------------------------------------------

   8 rows selected.

   PLAN_TABLE_OUTPUT
   --------------------------------------------------------------------------------
   Plan hash value: 1820398555

   -----------------------------------------------------
   | Id  | Operation	   | Name	    | Rows  |
   -----------------------------------------------------
   |   0 | SELECT STATEMENT   |		    |	  6 |
   |   1 |  HASH GROUP BY	   |		    |	  6 |
   |   2 |   TABLE ACCESS FULL| CUSTOMERS_TEST |	319 |
   -----------------------------------------------------

   9 rows selected.
   ```



5. 查看为表记录的列使用情况信息。

   `oe`用户调用该`DBMS_STATS.REPORT_COL_USAGE`函数以生成报告：

   ```
   SET LONG 100000
   SET LINES 120
   SET PAGES 0
   SELECT DBMS_STATS.REPORT_COL_USAGE(user, 'customers_test')
   FROM   DUAL;
   ```

   该报告显示在下面：

   ```sql
   LEGEND:
   .......

   EQ	   : Used in single table EQuality predicate
   RANGE	   : Used in single table RANGE predicate
   LIKE	   : Used in single table LIKE predicate
   NULL	   : Used in single table is (not) NULL predicate
   EQ_JOIN    : Used in EQuality JOIN predicate
   NONEQ_JOIN : Used in NON EQuality JOIN predicate
   FILTER	   : Used in single table FILTER predicate
   JOIN	   : Used in JOIN predicate
   GROUP_BY   : Used in GROUP BY expression
   ...............................................................................

   ###############################################################################

   COLUMN USAGE REPORT FOR OE.CUSTOMERS_TEST
   .........................................

   1. ACCOUNT_MGR_ID		       : EQ
   2. CUST_EMAIL			       : EQ
   3. GENDER			       : EQ
   4. (CUST_EMAIL, ACCOUNT_MGR_ID,
       GENDER)			       : FILTER
   ###############################################################################

   ```

   在上一个报告中，前三个列在第一个受监视的查询中的相等谓词中使用：

   ```
   ...
     WHERE  CUST_EMAIL = 'Charlotte.Buckley@PINTAIL.EXAMPLE.COM'
     AND    GENDER = 'F'
     AND    ACCOUNT_MGR_ID = 149;
   ```

   所有这三列都出现在同一`WHERE`子句中，因此报告将它们显示为组过滤器。`FILTER`报表中的列集是列组的候选项。

### KnowledgePoint

**父主题：** [关于列组的统计信息](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/tgsql/managing-extended-statistics.html#GUID-858E0BDA-06C6-471F-B9E8-888C3AD29673)

为特定工作负载检测有用的列组

您可以使用`DBMS_STATS.SEED_COL_USAGE`和`REPORT_COL_USAGE`根据指定的工作负荷来确定表需要哪些列组。

当您不知道要创建哪个扩展统计信息时，此技术很有用。此技术不适用于表达式统计。

## 实践19-4:创建在工作负载监视期间检测到的列组

### Overview

本教程假定您已经进行中上一步的步骤。

### Task

### Practice

1. `customers_test`根据在监视窗口期间捕获的使用情况信息为表创建列组。

   例如，`oe`用户运行以下查询：

   ```
   SELECT DBMS_STATS.CREATE_EXTENDED_STATS(user, 'customers_test') FROM DUAL;
   ```

   输出示例如下：

   ```
   ###############################################################################

   EXTENSIONS FOR OE.CUSTOMERS_TEST
   ................................

   1. (CUST_EMAIL, ACCOUNT_MGR_ID,
       GENDER)			       : SYS_STU6#U3UU_Q98DVUTCYY6RGBXW created
   ###############################################################################
   ```

   数据库创建了一个列组`customers_test`：一个列组用于过滤谓词。

2. 收集表统计信息。

   以用户`oe`身份，运行`GATHER_TABLE_STATS`以收集以下统计信息`customers_test`：

   ```
   EXEC DBMS_STATS.GATHER_TABLE_STATS(user,'customers_test');
   ```

3. 以用户`oe`身份，运行工作负载中两个查询的解释计划。

   检查`USER_TAB_COL_STATISTICS`视图以确定数据库创建了哪些其他统计信息：

   ```
   SELECT COLUMN_NAME, NUM_DISTINCT, HISTOGRAM
   FROM   USER_TAB_COL_STATISTICS
   WHERE  TABLE_NAME = 'CUSTOMERS_TEST'
   ORDER BY 1;
   ```

   部分样本输出如下所示：

   ```sql
   ...
   CUST_EMAIL		      319 NONE
   GENDER				2 FREQUENCY
   SYS_STU6#U3UU_Q98DVUTCYY6RGBXW	      319 NONE
   ...
   ```

   本示例显示了从`DBMS_STATS.CREATE_EXTENDED_STATS`函数返回的1个列组名称 `SYS_STU6#U3UU_Q98DVUTCYY6RGBXW`

4. 再次说明计划。

   以下示例显示了`customers_test`表中两个查询的解释计划：

   ```
   conn oe/oe@emrep;
   EXPLAIN PLAN FOR
     SELECT *
     FROM   customers_test
     WHERE  CUST_EMAIL = 'Charlotte.Buckley@PINTAIL.EXAMPLE.COM'
     AND    GENDER = 'F'
     AND    ACCOUNT_MGR_ID = 149;

   SELECT PLAN_TABLE_OUTPUT
   FROM   TABLE(DBMS_XPLAN.DISPLAY('plan_table', null,'basic rows'));

   EXPLAIN PLAN FOR
     SELECT   ACCOUNT_MGR_ID, GENDER, count(*)
     FROM     customers_test
     GROUP BY ACCOUNT_MGR_ID, GENDER;

   SELECT PLAN_TABLE_OUTPUT
   FROM   TABLE(DBMS_XPLAN.DISPLAY('plan_table', null,'basic rows'));
   ```

   新计划显示了更准确的基数估计。

### KnowledgePoint

您可以使用该`DBMS_STATS.CREATE_EXTENDED_STATS`函数通过执行创建先前检测到的列组`DBMS_STATS.SEED_COL_USAGE`。

也可以看看：

[Oracle Database PL / SQL软件包和类型参考](https://www.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/12.2/tgsql&id=ARPLS059)以了解该`DBMS_STATS`软件包
