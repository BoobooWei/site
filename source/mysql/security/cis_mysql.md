---
title: MySQL 5.7 安全基线
---

# 概述

本文根据以下文章进行整理学习：

* [CIS Oracle MySQL Community Server 5.7 Benchmark v1.0.0 - 12-29-2015](http://benchmarks.cisecurity.org)

* 文档《CIS Oracle MySQL Community Server 5.7 Benchmark》为建立MySQL Community Server的安全配置姿态提供了规范性指导。

* CIS（互联网安全中心）是美国一家非盈利技术组织，主要是制定一些系统安全基线。 

* 指南针对运行在ubuntulinux14.04上的MySQL社区服务器5.7进行了测试，但也适用于其他Linux发行版。

## 目标受众

本文档面向计划开发、部署、评估或保护包含Oracle MySQL Community Server 5.7的解决方案的系统和应用程序管理员、安全专家、审核员、帮助台和平台部署人员。

## 共识指导

该基准是使用由主题专家组成的共识审查过程创建的。共识参与者提供不同背景的观点，包括咨询、软件开发、审计和合规、安全研究、运营、政府和法律。

每个独联体基准都要经过两个阶段的共识审查。

* 第一阶段发生在初始基准开发期间。在这一阶段，主题专家召集起来讨论、创建和测试基准的工作草案。在就基准建议达成共识之前，将进行这一讨论。
* 第二阶段在基准发布之后开始。在这一阶段，所有由互联网社区提供的反馈都将由共识小组审查，以纳入基准。

如果您有兴趣参与协商一致进程，请访问https://community.cisecurity.org。


## 评分信息

评分状态表示对给定建议的遵从性是否会影响被评估对象的基准分数。此基准使用以下评分状态：Scored/Not Scored 代表记分/不记分

### Scored 得分

不遵守“评分”建议将降低最终基准分数。遵守“评分”建议将提高最终基准分数。

### Not Scored 未得分

不遵守“未评分”建议将不会降低最终基准分数。遵守“未评分”建议不会提高最终基准分数。

## Acknowledgements

这个基准测试举例说明了用户、供应商和主题专家的社区可以通过一致的协作完成的伟大的事情。独联体社区感谢整个共识小组，并特别感谢为本指南的制定做出了巨大贡献的以下个人：

Contributor
Adam Montville Robert Thomas
Editor
Binod Bista Daniël van Eeden

## 检查列表

基线检查项目共9个维度:
* 64小项；
* 记分项48个
* 不记分项12个

基准定义：

•1级-基础安全措施，对技术的实用性或性能不产生负面影响。
•2级-纵深防御措施，可能对技术的实用性或性能产生负面影响。"					
									

### 中文版

| 编号 | 子编号 | 基线列表                                                     | 是否积分 |
| ---- | ------ | ------------------------------------------------------------ | -------- |
| 1    |        | 操作系统级配置                                               |          |
|      | 1.1    | 将数据库放在非系统分区上（记分）                             | 1        |
|      | 1.2    | 使用MySQL守护程序/服务的最低特权专用帐户（计分）             | 1        |
|      | 1.3    | 禁用MySQL命令历史记录（记分）                                | 1        |
|      | 1.4    | 验证MYSQL_PWD环境变量是否未使用（已记分）                    | 1        |
|      | 1.5    | 禁用交互式登录（记分）                                       | 1        |
|      | 1.6    | 验证“MYSQL_PWD”未在用户配置文件中设置（记分）                | 1        |
| 2    |        | 安装和规划                                                   |          |
|      | 2.1    | 备份和灾难恢复                                               |          |
|      | 2.1.1  | 备份策略到位（不记分）                                       | 0        |
|      | 2.1.2  | 验证备份是否良好（不记分）                                   | 0        |
|      | 2.1.3  | 安全备份凭据（不记分）                                       | 0        |
|      | 2.1.4  | 备份应妥善保护（不计分）                                     | 0        |
|      | 2.1.5  | 时间点恢复（不记分）                                         | 0        |
|      | 2.1.6  | 灾难恢复计划（不记分）                                       | 0        |
|      | 2.1.7  | 配置和相关文件的备份（不记分）                               | 0        |
|      | 2.2    | 专用机器运行MySQL（不记分）                                  | 0        |
|      | 2.3    | 不要在命令行中指定密码（不记分）                             | 0        |
|      | 2.4    | 不重复使用用户名（不记分）                                   | 0        |
|      | 2.5    | 不要使用默认或非MySQL特定的加密密钥（未计分）                | 0        |
|      | 2.6    | 为特定用户设置密码过期策略（不记分）                         | 0        |
| 3    |        | 文件系统权限                                                 |          |
|      | 3.1    | 确保“datadir”具有适当的权限（已记分）                        | 1        |
|      | 3.2    | 确保“log_bin_basename”文件具有适当的权限（记分）             | 1        |
|      | 3.3    | 确保“日志错误”具有适当的权限（已记分）                       | 1        |
|      | 3.4    | 确保“慢查询日志”具有适当的权限（已记分）                     | 1        |
|      | 3.5    | 确保“中继日志”“basename”文件具有适当的权限（已记分）         | 1        |
|      | 3.6    | 确保“常规日志文件”具有适当的权限（记分）                     | 1        |
|      | 3.7    | 确保SSL密钥文件具有适当的权限（记分）                        | 1        |
|      | 3.8    | 确保插件目录具有适当的权限（记分）                           | 1        |
| 4    |        | 总则                                                         |          |
|      | 4.1    | 确保应用了最新的安全补丁（不记分）                           | 0        |
|      | 4.2    | 确保“测试”数据库未安装（记分）                               | 1        |
|      | 4.3    | 确保“允许可疑自定义项”设置为“假”（记分）                     | 1        |
|      | 4.4    | 确保“本地填充”已禁用（记分）                                 | 1        |
|      | 4.5    | 确保“mysqld”不是以“--skip-grant-tables”（记分）开头          | 1        |
|      | 4.6    | 确保启用“--skip-symbolic-links”（跳过符号链接）（记分）      | 1        |
|      | 4.7    | 确保“daemon_memcached”插件已禁用（记分）                     | 1        |
|      | 4.8    | 确保“secure_file_priv”不为空（记分）                         | 1        |
|      | 4.9    | 确保“sql模式”包含“STRICT”所有表（记分）                      | 1        |
| 5    |        | MySQL权限                                                    |          |
|      | 5.1    | 确保只有管理用户具有完整的数据库访问权限（记分）             | 1        |
|      | 5.2    | 确保非管理用户的“file_priv”未设置为“Y”（记分）               | 1        |
|      | 5.3    | 确保非管理用户的“process_priv”未设置为“Y”（记分）            | 1        |
|      | 5.4    | 确保非管理用户的“super_priv”未设置为“Y”（记分）              | 1        |
|      | 5.5    | 确保非管理用户的“shutdown_priv”未设置为“Y”（记分）           | 1        |
|      | 5.6    | 确保非管理用户的“create_user_priv”未设置为“Y”（记分）        | 1        |
|      | 5.7    | 确保非管理用户的“grant_priv”未设置为“Y”（记分）              | 1        |
|      | 5.8    | 确保非从属用户的“repl_slave_priv”未设置为“Y”（记分）         | 1        |
|      | 5.9    | 确保DML/DDL授权仅限于特定数据库和用户（计分）                | 1        |
| 6    |        | 审核和记录                                                   |          |
|      | 6.1    | 确保“log_error”记录不为空                                    | 1        |
|      | 6.2    | 确保日志文件存储在非系统分区上（计分）                       | 1        |
|      | 6.3    | 确保“log_error_verbosity”未设置为“1”（记分）                 | 1        |
|      | 6.4    | 确保审核日志记录已启用（不记分）                             | 0        |
|      | 6.5    | 确保“log raw”设置为“OFF”（记分）                             | 1        |
| 7    |        | 身份验证                                                     |          |
|      | 7.1    | 确保密码不存储在全局配置中（计分）                           | 1        |
|      | 7.2    | 确保“sql_mode”包含“NO_AUTO_CREATE_USER”（记分）              | 1        |
|      | 7.3    | 确保为所有MySQL帐户设置密码（计分）                          | 1        |
|      | 7.4    | 确保“default_password_lifetime”小于或等于“90”（记分）        | 1        |
|      | 7.5    | 确保密码复杂性到位（记分）                                   | 1        |
|      | 7.6    | 确保没有用户具有通配符主机名（记分）                         | 1        |
|      | 7.7    | 确保不存在匿名帐户（计分）                                   | 1        |
| 8    |        | 网络                                                         |          |
|      | 8.1    | 确保“have_ssl”设置为“YES”（记分）                            | 1        |
|      | 8.2    | 确保所有远程用户的“ssl_type”设置为“ANY”、“X509”或“SPECIFIED”（已记分） | 1        |
| 9    |        | 复制                                                         |          |
|      | 9.1    | 确保复制流量安全（不记分）                                   | 0        |
|      | 9.2    | 确保“MASTER_SSL_VERIFY_SERVER_CERT”设置为“YES”或“1”（记分）  | 1        |
|      | 9.3    | 确保“master_info_repository”设置为“TABLE”（记分）            | 1        |
|      | 9.4    | 确保复制用户的“super_priv”未设置为“Y”（记分）                | 1        |
|      | 9.5    | 确保没有复制用户具有通配符主机名（记分）                     | 1        |


### 英文版

| No.  | Subnumber | List                                                         | Scored |
| ---- | --------- | ------------------------------------------------------------ | ------ |
| 1    |           | Operating System Level Configuration                         |        |
|      | 1.1       | Place Databases on Non-System  Partitions (Scored)           | 1      |
|      | 1.2       | Use Dedicated Least Privileged  Account for MySQL Daemon/Service (Scored) | 1      |
|      | 1.3       | Disable MySQL Command History  (Scored)                      | 1      |
|      | 1.4       | Verify That the MYSQL_PWD  Environment Variables Is Not In Use (Scored) | 1      |
|      | 1.5       | Disable Interactive Login  (Scored)                          | 1      |
|      | 1.6       | Verify That 'MYSQL_PWD' Is Not  Set In Users' Profiles (Scored) | 1      |
| 2    |           | Installation  and Planning                                   |        |
|      | 2.1       | Backup  and Disaster Recovery                                |        |
|      | 2.1.1     | Backup policy in place (Not  Scored)                         | 0      |
|      | 2.1.2     | Verify backups are good (Not  Scored)                        | 0      |
|      | 2.1.3     | Secure backup credentials (Not  Scored)                      | 0      |
|      | 2.1.4     | The backups should be properly  secured (Not Scored)         | 0      |
|      | 2.1.5     | Point in time recovery (Not  Scored)                         | 0      |
|      | 2.1.6     | Disaster recovery plan (Not  Scored)                         | 0      |
|      | 2.1.7     | Backup of configuration and  related files (Not Scored)      | 0      |
|      | 2.2       | Dedicate Machine Running MySQL  (Not Scored)                 | 0      |
|      | 2.3       | Do Not Specify Passwords in  Command Line (Not Scored)       | 0      |
|      | 2.4       | Do Not Reuse Usernames (Not  Scored)                         | 0      |
|      | 2.5       | Do Not Use Default or  Non-MySQL-specific Cryptographic Keys (Not Scored) | 0      |
|      | 2.6       | Set a Password Expiry Policy  for Specific Users (Not Scored) | 0      |
| 3    |           | File  System Permissions                                     |        |
|      | 3.1       | Ensure 'datadir' Has  Appropriate Permissions (Scored)       | 1      |
|      | 3.2       | Ensure 'log_bin_basename'  Files Have Appropriate Permissions (Scored) | 1      |
|      | 3.3       | Ensure 'log_error' Has  Appropriate Permissions (Scored)     | 1      |
|      | 3.4       | Ensure 'slow_query_log' Has  Appropriate Permissions (Scored) | 1      |
|      | 3.5       | Ensure 'relay_log_basename'  Files Have Appropriate Permissions (Scored) | 1      |
|      | 3.6       | Ensure 'general_log_file' Has  Appropriate Permissions (Scored) | 1      |
|      | 3.7       | Ensure SSL Key Files Have  Appropriate Permissions (Scored)  | 1      |
|      | 3.8       | Ensure Plugin Directory Has  Appropriate Permissions (Scored) | 1      |
| 4    |           | General                                                      |        |
|      | 4.1       | Ensure Latest Security Patches  Are Applied (Not Scored)     | 0      |
|      | 4.2       | Ensure the 'test' Database Is  Not Installed (Scored)        | 1      |
|      | 4.3       | Ensure 'allow-suspicious-udfs'  Is Set to 'FALSE' (Scored)   | 1      |
|      | 4.4       | Ensure 'local_infile' Is  Disabled (Scored)                  | 1      |
|      | 4.5       | Ensure 'mysqld' Is Not Started  with '--skip-grant-tables' (Scored) | 1      |
|      | 4.6       | Ensure '--skip-symbolic-links'  Is Enabled (Scored)          | 1      |
|      | 4.7       | Ensure the 'daemon_memcached'  Plugin Is Disabled (Scored)   | 1      |
|      | 4.8       | Ensure 'secure_file_priv' Is  Not Empty (Scored)             | 1      |
|      | 4.9       | Ensure 'sql_mode' Contains  'STRICT_ALL_TABLES' (Scored)     | 1      |
| 5    |           | MySQL  Permissions                                           |        |
|      | 5.1       | Ensure Only Administrative  Users Have Full Database Access (Scored) | 1      |
|      | 5.2       | Ensure 'file_priv' Is Not Set  to 'Y' for Non-Administrative Users (Scored) | 1      |
|      | 5.3       | Ensure 'process_priv' Is Not  Set to 'Y' for Non-Administrative Users (Scored) | 1      |
|      | 5.4       | Ensure 'super_priv' Is Not Set  to 'Y' for Non-Administrative Users (Scored) | 1      |
|      | 5.5       | Ensure 'shutdown_priv' Is Not  Set to 'Y' for Non-Administrative Users (Scored) | 1      |
|      | 5.6       | Ensure 'create_user_priv' Is  Not Set to 'Y' for Non-Administrative Users (Scored) | 1      |
|      | 5.7       | Ensure 'grant_priv' Is Not Set  to 'Y' for Non-Administrative Users (Scored) | 1      |
|      | 5.8       | Ensure 'repl_slave_priv' Is  Not Set to 'Y' for Non-Slave Users (Scored) | 1      |
|      | 5.9       | Ensure DML/DDL Grants Are  Limited to Specific Databases and Users (Scored) | 1      |
| 6    |           | Auditing  and Logging                                        |        |
|      | 6.1       | Ensure 'log_error' Is Not  Empty (Scored)                    | 1      |
|      | 6.2       | Ensure Log Files Are Stored on  a Non-System Partition (Scored) | 1      |
|      | 6.3       | Ensure 'log_error_verbosity'  Is Not Set to '1' (Scored)     | 1      |
|      | 6.4       | Ensure Audit Logging Is  Enabled (Not Scored)                | 0      |
|      | 6.5       | Ensure 'log-raw' Is Set to  'OFF' (Scored)                   | 1      |
| 7    |           | Authentication                                               |        |
|      | 7.1       | Ensure Passwords Are Not  Stored in the Global Configuration (Scored) | 1      |
|      | 7.2       | Ensure 'sql_mode' Contains  'NO_AUTO_CREATE_USER' (Scored)   | 1      |
|      | 7.3       | Ensure Passwords Are Set for  All MySQL Accounts (Scored)    | 1      |
|      | 7.4       | Ensure  'default_password_lifetime' Is Less Than Or Equal To '90' (Scored) | 1      |
|      | 7.5       | Ensure Password Complexity Is  in Place (Scored)             | 1      |
|      | 7.6       | Ensure No Users Have Wildcard  Hostnames (Scored)            | 1      |
|      | 7.7       | Ensure No Anonymous Accounts  Exist (Scored)                 | 1      |
| 8    |           | Network                                                      |        |
|      | 8.1       | Ensure 'have_ssl' Is Set to  'YES' (Scored)                  | 1      |
|      | 8.2       | Ensure 'ssl_type' Is Set to  'ANY', 'X509', or 'SPECIFIED' for All Remote Users (Scored) | 1      |
| 9    |           | Replication                                                  |        |
|      | 9.1       | Ensure Replication Traffic Is  Secured (Not Scored)          | 0      |
|      | 9.2       | Ensure  'MASTER_SSL_VERIFY_SERVER_CERT' Is Set to 'YES' or '1' (Scored) | 1      |
|      | 9.3       | Ensure  'master_info_repository' Is Set to 'TABLE' (Scored)  | 1      |
|      | 9.4       | Ensure 'super_priv' Is Not Set  to 'Y' for Replication Users (Scored) | 1      |
|      | 9.5       | Ensure No Replication Users  Have Wildcard Hostnames (Scored) | 1      |



```python
cis_mysql_57_benchmark = [
{
    "name": "操作系统级配置",
    "v_name": "OperatingSystemLevelConfiguration",
    "value": [
                                    {
                                        "check_name": "",       #   基线名
                                        "is_scored":"1",        #   是否计分 1 积分 0 不计分
                                        "applicability":"",     #   基础安全措施、纵深防御措施
                                        "Description":"",       #   描述
                                        "rationale":"",         #   理论基础
                                        "audit":"",             #   检查算法
                                        "remediation":"",       #   修复方法
                                        "impact": None,         #   影响
                                        "default_value": None,  #   默认值
                                        "references": """""",   #   链接
                                        "check_func": "",       #   检查函数名
                                    }
            ]
},
{"name": "安装和规划","v_name":"InstallationandPlanning", "value": []},
{"name": "文件系统权限","v_name":"FileSystemPermissions","value": []},
{"name": "总则","v_name":"General","value": []},
{"name": "MySQL权限","v_name":"MySQLPermissions","value": []},
{"name": "审核和记录","v_name":"AuditingAndLogging","value": []},
{"name": "身份验证","v_name":"Authentication","value": []},
{"name": "网络","v_name":"Network","value": []},
{"name": "复制","v_name":"Replication","value": []},
]
```

# 操作系统级配置


```python
# 操作系统级配置
OperatingSystemLevelConfiguration = {
"name":"操作系统级配置",
"value":[
{
    "check_name": "将数据库放在非系统分区上",
    "is_scored":"1",
    "applicability":"基础安全措施",
    "Description":"主机操作系统应该为不同的目的包含不同的文件系统分区。一组文件系统通常称为“系统分区”，通常为主机系统/应用程序操作保留。另一组文件系统通常称为“非系统分区”，这些位置通常是为存储数据而保留的。",
    "rationale":"主机操作系统应该为不同的目的包含不同的文件系统分区。一组文件系统通常称为“系统分区”，通常为主机系统/应用程序操作保留。另一组文件系统通常称为“非系统分区”，这些位置通常是为存储数据而保留的。",
    "audit":"""执行以下命令：
show variables where variable_name = 'datadir';
df -h <datadir Value>
df命令的结果不应该包括 ('/'), "/var", or "/usr" 的目录。
""",
    "remediation":"""请执行以下步骤修正此设置：
1.为MySQL数据选择一个非系统分区的新位置
2.使用命令停止mysqld，比如：service mysql Stop
3.使用命令复制数据：cp-rp<datadir Value><new location>
4.将datadir位置设置为MySQL配置文件中的新位置
5.使用命令启动mysqld，比如：service mysql start

注意：在某些Linux发行版上，您可能需要另外修改apparmor设置。例如，在Ubuntu14.04.1系统上，编辑文件/etc/apparmor.d/usr.sbin.mysqld公司因此datadir访问是适当的。""",
    "impact": "将数据库移动到非系统分区可能很困难，这取决于设置操作系统时是否只有一个分区，以及是否有额外的可用存储空间。",
    "default_value": None,
    "references": None,
    "check_func": check_mysql_datadir,
},
{
    "check_name": "使用MySQL守护程序/服务的最低特权专用帐户",
    "is_scored":"1",
    "applicability":"基础安全措施",
    "Description":"与安装在主机上的任何服务一样，它可以提供自己的用户上下文。为服务提供一个专用的用户可以在更大的主机上下文中精确地约束服务。",
    "rationale":"利用MySQL的最低权限帐户执行as可能会减少MySQL固有漏洞的影响。受限帐户将无法访问与MySQL无关的资源，例如操作系统配置。",
    "audit":"""执行以下命令：
ps -ef | egrep "^mysql.*$"
如果未返回任何行，则不得分
""",
    "remediation":"创建一个只用于运行MySQL和直接相关进程的用户。此用户不得具有系统的管理权限。",
    "impact": None,
    "default_value": None,
    "references": """1. http://dev.mysql.com/doc/refman/5.7/en/changing-mysql-user.html 
2. http://dev.mysql.com/doc/refman/5.7/en/server-options.html#option_mysqld_user""",
    "check_func": check_mysql_user,
},
{
    "check_name": "禁用MySQL命令历史记录",
    "is_scored":"1",
    "applicability":"纵深防御措施",
    "Description":"在Linux/UNIX上，MySQL客户机将交互执行的语句记录到历史记录中文件。默认情况下，此文件在用户的主目录中命名为.mysql_history。在MySQL客户机应用程序中运行的大多数交互式命令都保存在历史文件中。应禁用MySQL命令历史记录。",
    "rationale":"禁用MySQL命令历史记录可以降低暴露敏感信息（如密码和加密密钥）的概率。",
    "audit":"""执行以下命令：
find /home -name ".mysql_history"
如果没有文件返回则得分，否则对于返回的每个文件，确定该文件是否符号链接到/dev/null，如果是则得分，否则不得分
""",
    "remediation":"""请执行以下步骤修正此设置：
1. 删除 .mysql_history（如果存在）。
2. 使用以下任一技术防止再次创建：
2.1 将 MYSQL_HISTFILE 环境变量设置为 /dev/null。这需要放在shell的启动脚本中。
2.2 创建 $HOME/.mysql_history 作为 /dev/null的软连接 。
> ln -s /dev/null $HOME/.mysql_history
""",
    "impact": None,
    "default_value": "默认情况下，MySQL命令历史文件位于$HOME/.MySQL_history中。",
    "references": """1. http://dev.mysql.com/doc/refman/5.7/en/mysql-logging.html 
2. http://bugs.mysql.com/bug.php?id=72158
""",
    "check_func":"disable_mysql_command_history",
},
{
    "check_name": "验证MYSQL_PWD环境变量是否未使用",
    "is_scored":"1",
    "applicability":"基础安全措施",
    "Description":"MySQL可以从名为MySQL_PWD的环境变量中读取默认数据库密码。",
    "rationale":"MYSQL_PWD环境变量的使用意味着MYSQL凭证的明文存储。避免这种情况可能会增加对MySQL凭证保密性的保证。",
    "audit":"""执行以下命令：
grep MYSQL_PWD/proc/*/environ
如果未返回任何行，则得分；否则代表设置了参数 MYSQL_PWD 则不得分。
""",
    "remediation":"检查哪些用户和/或脚本正在设置MYSQL_PWD，并将其更改为使用更安全的方法。",
    "impact": None,
    "default_value": None,
    "references": """1. http://dev.mysql.com/doc/refman/5.7/en/environment-variables.html
2. https://blogs.oracle.com/myoraclediary/entry/how_to_check_environment_variables""",
    "check_func": check_mysql_pwd_from_proc,
},
{
    "check_name": "禁用交互式登录",
    "is_scored":"1",
    "applicability":"纵深防御措施",
    "Description":"创建后，MySQL用户可以交互访问操作系统，这意味着MySQL用户可以像其他用户一样登录主机。",
    "rationale":"阻止MySQL用户以交互方式登录可能会减少MySQL帐户受损的影响。此外，访问MySQL服务器所在的操作系统需要用户自己的帐户，这也需要更多的责任。MySQL用户的交互访问是不必要的，应该禁用。",
    "audit":"""执行以下命令：
getent passwd mysql | egrep "^.*[\/bin\/false|\/sbin\/nologin]$"
如果未返回任何行，则不得分
""",
    "remediation":"""请执行以下步骤进行修复：（在终端中执行以下命令之一）
usermod -s /bin/false 
usermod -s /sbin/nologin
""",
    "impact": """此设置将阻止MySQL管理员使用MySQL用户以交互方式登录到操作系统。""",
    "default_value": None,
    "references": None,
    "check_func": check_mysql_login,
},
{
    "check_name": "验证“MYSQL_PWD”未在用户配置文件中设置",
    "is_scored":"1",
    "applicability":"基础安全措施",
    "Description":"MySQL可以从名为MySQL_PWD的环境变量中读取默认数据库密码。",
    "rationale":"MYSQL_PWD环境变量的使用意味着MYSQL凭证的明文存储。避免这种情况可能会增加对MySQL凭证保密性的保证。",
    "audit":"""执行以下命令：
grep MYSQL_PWD /home/*/.{bashrc,profile,bash_profile}
如果未返回任何行，则不得分
""",
    "remediation":"检查哪些用户和/或脚本正在设置MYSQL_PWD，并将其更改为使用更安全的方法。",
    "impact": None,
    "default_value": None,
    "references": """1. http://dev.mysql.com/doc/refman/5.7/en/environment-variables.html
2. https://blogs.oracle.com/myoraclediary/entry/how_to_check_environment_variables""",
    "check_func": check_mysql_pwd_from_file,
},
],

}
```
# 安装和规划
## 备份和灾难恢复
### 备份策略到位（不记分）
### 验证备份是否良好（不记分）
### 安全备份凭据（不记分）
### 备份应妥善保护（不计分）
### 时间点恢复（不记分）
### 灾难恢复计划（不记分）
### 配置和相关文件的备份（不记分）
## 专用机器运行MySQL（不记分）
## 不要在命令行中指定密码（不记分）
## 不重复使用用户名（不记分）
## 不要使用默认或非MySQL特定的加密密钥（未计分）
## 为特定用户设置密码过期策略（不记分）
# 文件系统权限
## 确保“datadir”具有适当的权限（已记分）
## 确保“log_bin_basename”文件具有适当的权限（记分）
## 确保“日志错误”具有适当的权限（已记分）
## 确保“慢查询日志”具有适当的权限（已记分）
## 确保“中继日志”“basename”文件具有适当的权限（已记分）
## 确保“常规日志文件”具有适当的权限（记分）
## 确保SSL密钥文件具有适当的权限（记分）
## 确保插件目录具有适当的权限（记分）
# 总则
## 确保应用了最新的安全补丁（不记分）
## 确保“测试”数据库未安装（记分）
## 确保“允许可疑自定义项”设置为“假”（记分）
## 确保“本地填充”已禁用（记分）
## 确保“mysqld”不是以“--skip-grant-tables”（记分）开头
## 确保启用“--skip-symbolic-links”（跳过符号链接）（记分）
## 确保“daemon_memcached”插件已禁用（记分）
## 确保“secure_file_priv”不为空（记分）
## 确保“sql模式”包含“STRICT”所有表（记分）
# MySQL权限
## 确保只有管理用户具有完整的数据库访问权限（记分）
## 确保非管理用户的“file_priv”未设置为“Y”（记分）
## 确保非管理用户的“process_priv”未设置为“Y”（记分）
## 确保非管理用户的“super_priv”未设置为“Y”（记分）
## 确保非管理用户的“shutdown_priv”未设置为“Y”（记分）
## 确保非管理用户的“create_user_priv”未设置为“Y”（记分）
## 确保非管理用户的“grant_priv”未设置为“Y”（记分）
## 确保非从属用户的“repl_slave_priv”未设置为“Y”（记分）
## 确保DML/DDL授权仅限于特定数据库和用户（计分）
# 审核和记录
## 确保“log_error”记录不为空
## 确保日志文件存储在非系统分区上（计分）
## 确保“log_error_verbosity”未设置为“1”（记分）
## 确保审核日志记录已启用（不记分）
## 确保“log raw”设置为“OFF”（记分）
# 身份验证
## 确保密码不存储在全局配置中（计分）
## 确保“sql_mode”包含“NO_AUTO_CREATE_USER”（记分）
## 确保为所有MySQL帐户设置密码（计分）
## 确保“default_password_lifetime”小于或等于“90”（记分）
## 确保密码复杂性到位（记分）
## 确保没有用户具有通配符主机名（记分）
## 确保不存在匿名帐户（计分）
# 网络
## 确保“have_ssl”设置为“YES”（记分）
## 确保所有远程用户的“ssl_type”设置为“ANY”、“X509”或“SPECIFIED”（已记分）
# 复制
## 确保复制流量安全（不记分）
## 确保“MASTER_SSL_VERIFY_SERVER_CERT”设置为“YES”或“1”（记分）
## 确保“master_info_repository”设置为“TABLE”（记分）
## 确保复制用户的“super_priv”未设置为“Y”（记分）
## 确保没有复制用户具有通配符主机名（记分）
