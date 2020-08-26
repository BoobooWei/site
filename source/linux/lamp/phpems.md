---
title: 搭建LAMP架构的考试系统PHPEMS
---

# PHPEMS简介

开源免费的在线考试系统PHPEMS 6.0

http://www.phpems.net/

下载地址

https://www.a5xiazai.com/php/103885.html



# 应用环境

测试环境 10.200.6.53 centos 6

生产环境 10.0.0.121 centos 7



| LAMP架构  | 应用   | 软件                       | 主配置文件                 | 数据目录             |
| :-------- | :----- | :------------------------- | :------------------------- | :------------------- |
| 前端      | PHP    | php-5.4.16 | /etc/httpd/conf.d/php.conf | /var/lib/php/        |
|中间件| php-mysql|php-mysql-5.4.16|||
| Web服务器 | Apache | httpd-2.4.6                | /etc/httpd/conf/httpd.conf | /var/www/            |
| 数据库    | MySQL  | mysql-server 5.7.31        | /etc/my.cnf                | /alidata/mysql/data/ |

# 应用搭建

> 生产环境

## INSTALL

```bash
yum install -y httpd php php-mysql 
```

二进制安装mysql5.7版本，使用自动脚本部署 [install_mysql-5.7.31.sh](https://github.com/BoobooWei/DBA_Mysql/blob/master/scripts/auto_intall/install_mysql-5.7.31.sh)

## CONFIGURE

### APACHE

```bash
# 修改配置文件添加默认的动态网站首页
sed -i 's/index.html/index.html index.php/' /etc/httpd/conf/httpd.conf
# 下载在线考试系统并，解压到当前目录
wget http://61.155.169.167:81/uploads/userup2/1878/phpems20190924.zip
unzip Discuz_X3.0_SC_UTF8.zip
# 将upload中的文件第归复制到网站根目录下
cp phpems/* /var/www/html -r
chmod 777 /var/www/html -R
```



### MySQL

```bash
# 创建业务库pe6
create database pe6;
# 对前端php进行授权
grant all on pe6.* to "pe6_rw"@"10.0.0.121" identified by 'Pe6_rw@123';

flush privileges;
# 导入数据
use pe6;
source /var/www/html/pe6.sql;
```



## SERVICE

```bash
# 启动相应的服务，并设置为开机启动
setenforce 0
systemctl stop firewalld
systemctl disable firewalld
systemctl start httpd
systemctl enable httpd
```



## PHP

```bash
# 修改代码中的连接mysql 的配置文件
vim /var/www/html/lib/config.inc.php
define('DB','pe6');//MYSQL数据库名
define('DH','10.0.0.121');//MYSQL主机名，不用改
define('DU','pe6_rw');//MYSQL数据库用户名
define('DP','Pe6_rw@123');//MYSQL数据库用户密码
define('DTH','x2_');//系统表前缀，不用改
# web 页面访问系统 10.0.0.121
```





# 应用二次开发

修改页面前端打造DBA专用的考试平台

php代码打包：

数据打包：

## 使用方法

1. 将ks.com.tar 解压到 /var/www/html/目录，授予 777 权限；
2. 将 xxx.sql 覆盖 pe6 库
3. 修改php的配置中数据库的连接方式即可

# 数据库备份

```bash
mkdir /alidata/cloudcare_dba -p
cd /alidata/cloudcare_dba/
 
 
[root@node2 cloudcare_dba]# cat auto_backup_ks.sh
#!/bin/bash
# 自动备份ks考试系统
# booboowei 2020.07.07
DBUser=pe6_rw
DBPwd=Pe6_rw@123
DBName=pe6
DBHost=localhost
BackupPath="/alidata/cloudcare_dba/ks_backup"
BackupFile="$DBName-"$(date +%y%m%d_%H)".sql"
BackupLog="$DBName-"$(date +%y%m%d_%H)".log"
# Backup Dest directory, change this if you have someother location
if !(test -d $BackupPath)
then
mkdir $BackupPath -p
fi
cd $BackupPath
a=`/alidata/mysql/bin/mysqldump -u$DBUser -p$DBPwd -h$DBHost $DBName --opt --set-gtid-purged=OFF --default-character-set=utf8 --single-transaction --hex-blob --skip-triggers --max_allowed_packet=824288000 > "$BackupPath"/"$BackupFile" 2> /dev/null; echo $?`
if [ $a -ne 0 ]
then
 echo "$(date +%y%m%d_%H:%M:%S) 备份失败" >> $BackupLog
else
 echo "$(date +%y%m%d_%H:%M:%S) 备份成功" >> $BackupLog
fi
#Delete sql type file & log file
find "$BackupPath" -name "$DBname*[log,sql]" -type f -mtime +3 -exec rm -rf {} \;
 
 
[root@node2 cloudcare_dba]# crontab -l
59 0 * * * /bin/bash /alidata/cloudcare_dba/auto_backup_ks.sh
```


# 批量修改

```bash 
sed -n  '/Copyright.*Copyright.*dba.toberoot.com.*\</p' ./data/html/core/tpls/app/index.html

sed -i 's!Copyright.*Copyright.*dba.toberoot.com.*\<!Mail: rgweiyaping@hotmail.com   备案号：沪ICP备2020026043号\<!g' ./data/html/core/tpls/app/index.html

while read line; do sed -n '/Copyright.*Copyright.*dba.toberoot.com.*\</p' $line;echo $line; done < copy.txt

while read line; do sed -n '/Copyright.*Copyright.*dba.toberoot.com.*\</p' $line;echo $line; done < 2.txt	
while read line; do sed -i 's!Copyright.*Copyright.*dba.toberoot.com.*\<!Mail: rgweiyaping@hotmail.com   备案号：沪ICP备2020026043号\<!g'	$line;echo $line; done < 2.txt	
while read line; do sed -n '/Copyright.*dba.toberoot.com/p' $line;echo $line; done < copy.txt	
while read line; do sed -i 's!Copyright.*dba.toberoot.com!Mail: rgweiyaping@hotmail.com   备案号：沪ICP备2020026043号!g'	$line;echo $line; done < copy.txt
while read line; do sed -n '/沪ICP备2020026043号/p' $line;echo $line; done < copy.txt
		
./data/compile/exam/tpls/app/%%cpl%%footer.php
./data/compile/exam/tpls/master/%%cpl%%footer.php
./data/compile/content/tpls/master/%%cpl%%footer.php
./data/compile/bank/tpls/master/%%cpl%%footer.php
./data/compile/autoform/tpls/master/%%cpl%%footer.php
./data/compile/document/tpls/master/%%cpl%%footer.php
./data/compile/course/tpls/app/%%cpl%%footer.php
./data/compile/course/tpls/master/%%cpl%%footer.php
./data/compile/user/tpls/app/%%cpl%%footer.php
./data/compile/user/tpls/master/%%cpl%%footer.php
./data/compile/user/tpls/center/%%cpl%%footer.php
./data/compile/core/tpls/app/%%cpl%%footer.php
./data/compile/core/tpls/master/%%cpl%%footer.php
./data/compile/seminar/tpls/app/%%cpl%%footer.php
./data/compile/seminar/tpls/master/%%cpl%%footer.php
./data/compile/docs/tpls/master/%%cpl%%footer.php
./data/compile/certificate/tpls/master/%%cpl%%footer.php	
```

