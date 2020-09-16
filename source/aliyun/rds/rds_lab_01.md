title: RDS高负载和查询效率低的混合场景性能优化

# 实验概述

RDS MySQL 使用过程中，会遇到CPU使用率过高甚至达到100%的情况。该实验模拟生产中QPS负载高和查询效率低的混合场景，需要学生对CPU问题进行排查并优化。

该场景中RDS For MySQL CPU使用率上升原因有以下两点：

1. 应用负载（QPS）变高 
2. 慢查询

为何慢SQL会导致CPU高呢？

应用提交查询操作或数据修改操作时，系统需要执行大量的逻辑读操作，其中逻辑IO包含执行查询所需访问表的数据行数。所以系统需要消耗大量的CPU资源以维护从存储系统读取到内存中的数据一致性。

> 提示：大量行锁冲突、行锁等待或后台任务也有可能会导致实例的CPU使用率过高，本实验中已排除这些因素。

属于 QPS 高和查询效率低的混合模式导致的 CPU 使用率高，建议从优化查询入手。

## 实验目的

完成此实验后，可以掌握的能力有：

1）RDS 实例CPU问题的诊断方法。

2）RDS 实例QPS负载高和查询效率低的混合场景导致的CPU问题的优化方法。

# 实验准备

本实验需要开通的资源：开通专有网络VPC、创建 RDS MySQL 5.7 1台、开通DMS企业版、开通DAS、云监控CMS。通过以下步骤完成实验准备。

## 开通实例

1 登录阿里云管理控制台，进入云数据库RDS实例的管理页面。

1. 点击 **控制台url** ，进入阿里云管理控制台登录界面；
2. 在阿里云登录页面，输入本实验提供的 **子用户名称** 和 **子用户密码** ，并点击 **登录** ；

![屏幕快照 2020-09-09 上午10](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/01.png)![屏幕快照 2020-09-09 上午10](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/02.png)

3. 在阿里云管理控制台，点击顶部 **产品与服务** ，在弹出的下拉菜单中，依次点击 **数据库 ---> 云数据库RDS** ，进入RDS数据库的管理页面；

![屏幕快照 2020-09-09 上午10](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/03.png)

4. 购买按量付费RDS实例
5. 在 **实例列表** 页面中，选择 **实验资源** 中提供的 **地域**，然后在 **搜索框** 中，输入本 **实验资源** 提供的 **RDS实例ID** 并点击 **搜索**，找到本 **实验资源** 提供的RDS实例ID，点击 **管理** ，进入RDS实例的管理页面。

![屏幕快照 2020-09-09 上午11](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/04.png)

## 创建数据库账号

创建RDS数据库账号和密码：demo_user/Demo@User

1）点击左侧栏 **账号管理** ，右侧中心页面进入 账号管理 页面；

2）点击右侧中心页面 **创建账号** ，创建数据库账号；

![屏幕快照 2020-09-09 上午11](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/05.png)

3）根据命名提示，输入 **数据库账号** 和 **密码**：

- 数据库账号：demo_user
- 密码：Demo@User
- 账号类型：普通账号

![屏幕快照 2020-09-09 上午11](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/06.png)

4）完成如上操作后，点击 确定 ；

5）返回 账号管理 页面，新建账号 demo_user 的状态为 **激活**。

![屏幕快照 2020-09-09 上午11](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/07.png)

## 创建数据库

创建RDS数据库：demo_db

1）点击左侧栏的 **数据库管理** ，进入数据库管理页面；

2）在右侧的 **数据库管理** 页面，点击 **创建数据库** ，进入创建页面；

![屏幕快照 2020-09-09 上午11](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/08.png)

3）在创建页面中，添加如下信息：

- 数据库（DB）名称：根据输入框下端的命名规则 ，输入数据库名称：**demo_db** 。
- 支持字符集：**utf8**
- 授权帐号：选择之前新建的数据库账号 **demo_user**。
- 账号类型：设置为 **读写** 

![屏幕快照 2020-09-09 上午11](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/09.png)



4）完成如上配置信息后，点击底部的 **创建** ，完成数据库的创建。

5）返回数据库管理页面可以看到新建的 **数据库名**、**数据库状态**、**绑定账号** 等，此时数据库状态为创建中，等待1分钟左右，点击右上角的 **刷新** ，可以查看到数据库状态为 **运行中** 且 **绑定账号** 为新建数据库账号 **demo_user** 。

![屏幕快照 2020-09-09 上午11](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/10.png)

## 登陆DMS

1）在 数据库管理 页面，点击 **登陆数据库**，登陆DMS控制台；

![屏幕快照 2020-09-09 上午11](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/11.png)

2）在弹出的 **登陆实例** 对话框，检查 **数据库类型、实例地区、实例ID** 是否是 **实验资源** 中的信息，并输入之前创建的 **数据库账号和密码：demo_user/Demo@User** ，点击 **测试连接** ，通过后点击 **登录** ；

![屏幕快照 2020-09-09 上午11](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/12.png)

注意：若弹出 **白名单问题** ，点击 **设置白名单** ，或者直接点击 **关闭对话框** 。



![屏幕快照 2020-09-09 上午11](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/13.png)



## 执行SQL

执行SQL，创建表table_demo，并导入数据。

```
# 创建测试表 table_demo
# 存储过程test1会自动导入1千万行数据到table_demo表中
# 存储过程test2和test3用于模拟高负载和慢查询的场景
CREATE TABLE table_demo(   
id int primary key auto_increment,    
uname  varchar(20) ,   
ucreatetime  datetime  ,   
age  int(11)) DEFAULT CHARACTER SET=utf8 COLLATE=utf8_general_ci;

delimiter $
create  procedure test1()  
begin
declare v_cnt decimal (10)  default 0 ;
start transaction;
dd:loop            

        insert  into table_demo values       
        (null,'用户1',sysdate(),20),         
        (null,'用户2',sysdate(),20),         
        (null,'用户3',sysdate(),20),         
        (null,'用户4',sysdate(),20),         
        (null,'用户5',sysdate(),20),         
        (null,'用户6',sysdate(),20),         
        (null,'用户7',sysdate(),20),         
        (null,'用户8',sysdate(),20),         
        (null,'用户9',sysdate(),20),         
        (null,'用户0',sysdate(),20)             
                ;                                        
        set v_cnt = v_cnt+10 ;                            
            if  v_cnt = 10000000 then leave dd;                           
            end if;          
        end loop dd ; 
commit;
end $

delimiter $
CREATE DEFINER=`demo_user`@`%` PROCEDURE `test2`()
begin
declare v_cnt decimal (10)  default 0 ;
start transaction;
dd:loop            


        insert  into table_demo values       
        (null,'用户-test',sysdate(),20);
        select count(*) from  table_demo where uname='用户-test';                                        
        set v_cnt = v_cnt+1 ;                            
            if  v_cnt = 10 then leave dd;                           
            end if;       
        end loop dd ; 
commit;
end $


delimiter $
CREATE DEFINER=`demo_user`@`%` PROCEDURE `test3`()
begin
declare v_cnt decimal (10)  default 0 ;
-- start transaction;
dd:loop            
        select count(*) from  table_demo where uname='用户-test';                                        
        set v_cnt = v_cnt+1 ;                            
            if  v_cnt = 1000 then leave dd;                           
            end if;       
        end loop dd ; 
-- commit;
end $
delimiter ;
call test1();
```

1）在DMS控制台的 **搜索框**，输入 **实验资源** 提供的 **RDS实例ID** ，并点击 **搜索** ，在 **已登录实例** ，**双击 demo_db**，在右侧弹出的 **SQL Console** 中，输入以上 **SQL** 并点击 **执行** ，可以看到有5条执行结果；

![屏幕快照 2020-09-09 上午11](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/14.png)

2）在中间 **表** 部分，点击 **刷新** ，可以看到新创建的 **table_demo表** ，执行如下SQL语句，可以看到已导入 **10000000** 条数据；

```
SELECT COUNT(*) FROM `table_demo`
```

![屏幕快照 2020-09-09 上午11](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/15.png)

3）在中间 **可编程对象** 部分，可以看到 **存储过程 ：test1、test2、test3** ，至此实验准备已完成。

![屏幕快照 2020-09-09 上午11](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/16.png)

## 实验资源

![image-20200916093650525](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/17.png)

# 实验步骤

## 配置告警规则

>  通过云监控CMS为RDS配置CPU报警规则

第一步： 在RDS管理控制台，进入 **实例管理** ，在上方选择实例所在 **地域**，点击左侧菜单栏的 **监控与报警** ，在右侧点击 **报警** ，可以看到目前还未设置任何报警，点击 **报警规则设置** ，进入云监控控制台；

![屏幕快照 2020-09-09 上午11](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/18.png)

第二步：在报警规则页点击 **创建报警规则** ；

![屏幕快照 2020-09-09 上午11](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/19.png)

若弹出如下对话框，点击 **升配监控频率** 。

![屏幕快照 2020-09-09 上午11](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/20.png)

在 **设置报警规则** 中，填写 **告警规则** ：

**规则名称：CPU使用率超过80%**

**规则描述：CPU使用率、1分钟周期、持续3个周期、平均值>=80%**

![img](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/21.png)

在 **通知方式** ，选择 **通知对象 、报警级别** ，然后点击 **确定** 。

![img](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/22.png)



注意：若 **无联系人通知组** ，可在 **通知方式** 的 **通知对象** 下，点击 **快速创建联系人组** 进行创建；输入 **组名、选择联系人** ，若无联系人，可点击 **新建联系人** 进行创建；

![img](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/23.png)

![img](https://edu.aliyun.com/labadmin/providers/my-courses/sections/65555570b4424115a5e1471a2b2bba21/editing)

输入报警联系人信息：**姓名、手机号码、邮箱、钉钉机器人**（可参考文档 **如何获得钉钉机器人地址**），点击 **保存** ；创建完成报警联系人后，返回 **创建报警规则**，并选择创建的 **报警联系人** 和 **联系人通知组** 。

![img](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/24.png)

第三步：查看配置完成的报警规则 **CPU使用率** ，另外还可以配置 **内存使用率、连接数使用率** 等。

![course-5f30f52a18234433a05002715c01db02-section-65555570b4424115a5e1471a2b2bba21-content-image-1599534340201-khl8Zc](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/25.png)

## 模拟CPU问题

>  模拟QPS负载高和查询效率低的混合场景导致的CPU问题

第一步：登陆DMS控制台，输入用户名和密码demo_user/Demo@User，访问demo_db库

![img](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/26.png)

第二步：选中 **demo_db** ，打开9个SQLConsole窗口，分别执行 **call test2();** 打开第10个SQLConsole窗口，执行 **call test3();**

![img](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/27.png)

![img](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/28.png)

第三步：等待执行完成，并查看 **执行历史**

![img](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/29.png)

第四步：收到 **钉钉、邮件、短信报警通知**

（图片仅供参考，时间和名称会实验中操作时不同。）

1.钉钉通知

![IMG_8671](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/30.png)

2.邮件通知

![屏幕快照 2020-09-10 下午3](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/31.png)

3.短信通知

![IMG_8670](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/32.png)

第五步：返回RDS控制台，进入 **RDS实例管理**，点击左侧 **监控与报警** ，在右侧页面点击 **监控** ，即可看到RDS **资源监控**：**CPU使用率达到100%，超过80%** ，模拟成功。

![img](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/33.png)



如下图所示，点击 **引擎监控** ，查看TPS/QPS监控图表，可以看到CPU飚高阶段 QPS变化趋势一致。如果想要有更明显的趋势，可以增加会话，例如开启30个会话执行 **call test2();**

![img](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/34.png)



第六步：为了避免实验结束后收到报警提醒，删除实验中创建的报警联系人和报警联系组。

1. 返回云监控控制台，点击左侧菜单 **报警服务** 下的 **报警联系人** ，点击右侧页面的 **删除** ；

![屏幕快照 2020-09-11 下午2](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/45.png)

点击 **确定** ，删除实验中创建的报警联系人。

![屏幕快照 2020-09-11 下午2](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/35.png)

2. 点击 **报警联系组** ，点击 **删除按钮** ；

![屏幕快照 2020-09-11 下午2](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/36.png)

点击 **确定** ，删除实验中创建的报警联系组。

![屏幕快照 2020-09-11 下午2](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/37.png)

## 诊断CPU问题

{% note info 步骤概览%}

第一步：查看告警明细

第二步：信息收集

1. 实例基础信息（规格、数据库版本等基础信息）
2. 诊断报告（资源使用率、慢查询、锁等）
3. 慢SQL详细信息（CPU飙高时段前后的慢SQL趋势，和慢SQL明细）

第三步：SQL诊断优化

第四步：总结CPU问题原因并提出解决建议

{% endnote %}



### 第一步：查看告警明细

在云监控控制台，点击 **报警服务** ，**报警历史** ，在右侧找到对应的报警 **实例ID** ，点击右侧 **图表** ，进一步查看报警对象的情况。

![img](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/38.png)

### 第二步：信息收集

返回RDS管理控制台，点击左侧菜单栏的 **基本信息** ，搜集报警的RDS的基础信息，包括：**实例信息、监控信息（资源监控、引擎监控）、日志**

1. 实例信息
   ![img](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/39.png)



快速了解实例的基础配置，例如demo中使用的是1核1G 5G存储的RDS实例。

2. 创建诊断报告

点击RDS管理控制台左侧菜单 **自治服务** 下的 **诊断报告** ，在右侧页面点击 **发起诊断** 。

![course-5f30f52a18234433a05002715c01db02-section-55d40cc875894ae4b448c337c8fac19f-content-image-1599535359080-297NXJ](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/40.png)

根据告警可以将时间范围缩短在 2020年8月31日 13:05:35 -2020年8月31日 14:05:35 （参考 **实验资源** 提供的时间范围）。

![img](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/41.png)

点击确定，等待一分钟所有刷新，可以看到报告。

![img](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/42.png)

3. 查看报告

![img](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/43.png)

![屏幕快照 2020-09-10 下午6](https://edu.aliyun.com/lab/files/courses/5f30f52a18234433a05002715c01db02/sections/55d40cc875894ae4b448c337c8fac19f/content/images/course-5f30f52a18234433a05002715c01db02-section-55d40cc875894ae4b448c337c8fac19f-content-image-1599733365965-Lknlhk)

![img](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/44.png)

![img](https://edu.aliyun.com/lab/files/courses/5f30f52a18234433a05002715c01db02/sections/55d40cc875894ae4b448c337c8fac19f/content/images/course-5f30f52a18234433a05002715c01db02-section-55d40cc875894ae4b448c337c8fac19f-content-image-1599535856694-lSe9O8)

从报告中，可以确定CPU飚高时段中没有死锁，但存在慢SQL，从资源监控中看到 CPU 使用率与QPS变化曲线一致。

4. 自治服务：慢SQL

点击RDS管理控制台左侧菜单 **自治服务** 下的 **慢SQL** ，进一步排查慢SQL的情况。

选择CPU飚高时间段，如2020年8月31日 13:05:35 -2020年8月31日 14:05:35 （参考 **实验资源** 提供的时间范围），点击 **查看** 。

![img](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/46.png)

可以看到该时间段的慢SQL与报告中显示一致，均为 **CALL test2()**。

5. 自治服务：一键诊断

点击RDS管理控制台左侧菜单 **自治服务** 下的 **一键诊断** ，点击 **性能洞察** ，获取到真正导致慢的问题SQL为 test2存储过程中的select语句。

![course-5f30f52a18234433a05002715c01db02-section-55d40cc875894ae4b448c337c8fac19f-content-image-1599535921509-GofZ1h](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/47.png)

![img](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/48.png)

**备注：若无 平均活跃会话 数据，可在 DMS控制台，**选中 **demo_db** ，打开9个SQLConsole窗口，同时执行 **call test3();** 然后刷新页面即可看到。

### 第三步：自治服务-SQL诊断优化

点击 **性能洞察 ---> 平均活跃会话数量 ---> 优化** 按钮，进入“SQL诊断优化”窗口，等待1-3分钟，可查看 **诊断结果 。**![img](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/49.png)

诊断结果提供了索引诊断建议并列出了优化后的收益，见如下内容详情：

**索引诊断建议**

```
ALTER TABLE `demo_db`.`table_demo` ADD INDEX `idx_uname` (`uname`);
```

优化收益：预期性能提升502959.86倍(提升倍数=旧代价/新代价-1)

**第四步：总结CPU问题原因并提出解决建议**

**CPU问题的原因**

本次RDS For MySQL CPU使用率上升原因有以下两点： 

1. 应用负载（QPS）变高
2. 慢查询

属于 QPS 高和查询效率低的混合模式导致的 CPU 使用率高，建议从优化查询入手彻底解决问题。比如我们分析到本次实验中的demo_db.table_demo表，缺失索引，建议在业务低峰期添加以下索引：

```
ALTER TABLE `demo_db`.`table_demo` ADD INDEX `idx_uname` (`uname`);
```



**解决建议**



1. CPU100%的情况下，若需要快速将CPU使用率降下来，可以在应用侧允许kill会话的前提下，**结束全部会话**。
2. 在业务低峰期对慢SQL进行优化，添加索引。





具体步骤见下一章节。



# 处理CPU问题

上一章节中我们已经排查出CPU问题是由于 QPS 高和查询效率低的混合模式导致，并给出了解决建议：

1. CPU100%的情况下，若需要快速将CPU使用率降下来，可以在应用侧允许kill会话的前提下，结束全部会话。
2. 在业务低峰期对慢SQL进行优化，添加索引。

### 第一步：结束全部会话

> 为了快速将CPU使用率降低下来，（实际生产中应在应用侧允许kill会话的前提下），使用 **DAS** 的 **会话管理** 功能来**结束全部会话**。

1. 点击RDS管理控制台左侧菜单 **自治服务** 下的 **一键诊断** ，点击 **会话管理** ，点击 **结束全部会话** 。

![img](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/50.png)

2. 点击弹框中的 **确定** 按钮。

![img](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/51.png)

3. 看到弹框中的 **执行结果** 列全部显示为 **kill成功** ，则可以点击 **关闭**。



### 第二步：添加索引

> 阻塞会话全部结束后，我们选择在业务低峰期对慢SQL进行优化，按照上一章节中慢SQL优化分析的建议**添加索引**。

索引诊断建议

```
ALTER TABLE `demo_db`.`table_demo` ADD INDEX `idx_uname` (`uname`);
```

打开DMS的SQLConsole窗口，复制上面的SQL命令，并点击 **执行**。（*注意一定要先完成第一步的删除所有会话再执行添加索引的操作，否则会引起锁冲突，添加索引的SQL语句执行时间大概在25秒）*

![img](https://edu.aliyun.com/lab/files/courses/5f30f52a18234433a05002715c01db02/sections/5247107ef78d480fa13967f4d49e4a80/content/images/course-5f30f52a18234433a05002715c01db02-section-5247107ef78d480fa13967f4d49e4a80-content-image-1600219416754-FaUrLY)



1）在 DMS控制台，选中 demo_db ，执行如上SQL；（注意：若还有 9个SQLConsole窗口在执行 call test3(); 中断任务后再执行如上SQL。）

2）以上SQL执行完毕后，再次打开9个SQLConsole窗口，执行 call test3(); 可以看到执行速度明显加快。 

### 第三步：验证结果

> 索引添加完毕后，验证优化结果。

| 对比          | 优化前   | 优化后 |
| ------------- | -------- | ------ |
| call test2(); | 100137ms | 24ms   |

下图为添加索引前 test2() 的执行结果，耗时100137ms。

![img](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/52.png)

下图为添加索引后 test2() 的执行结果，耗时24ms。

![img](/Users/booboowei/Documents/GitHub/site/source/aliyun/rds/pic/53.png)





### 第四步：其他建议

{% note warn 避免出现 CPU 使用率达到100%影响业务的一般原则 %}

1）云监控CMS设置 CPU 使用率报警，实例 CPU 使用率保证一定的冗余度。

2）应用设计和开发过程中，要考虑查询的优化，遵守 MySQL 优化的一般优化原则，降低查询的逻辑 IO，提高应用可扩展性。

3）新功能、新模块上线前，要使用生产环境数据进行压力测试（可以考虑使用阿里云 PTS 压力测试工具）。

4）建议经常关注慢查询日志和使用DAS中的诊断报告。

{% endnote %}



到此CPU问题处理结束。



# 思考与讨论

1. 通过本次实验，思考并探索云数据RDS CPU问题的诊断与解决方法。

2. 进一步了解并应用避免出现 CPU 使用率达到 100% 影响业务的一般原则。