---
title: Nginx
---

![img](https://confluence.jiagouyun.com/plugins/servlet/confluence/placeholder/macro?definition=e3RvY30&locale=en_GB&version=2)

# 概述

Nginx 是俄罗斯人编写的十分轻量级的HTTP 服务器,Nginx，它的发音为“engine X”， 是一个高性能的HTTP 和反向代理服务器，同时也是一个IMAP/POP3/SMTP 代理服务器。

# 部署方式

> 官方帮助文档：http://www.nginx.cn/install

本文分别介绍了在线和离线两种环境下的安装的方式，即：

1. 在线安装
2. 离线安装
   1. 离线安装包
   2. 二进制安装包
   3. 源码编译安装

安装方式的选择：

1. 如果可以在线安装则直接使用官方的源即可快速部署；如果为离线安装，则所有的安装包（源码包、二进制预编译包、rpm包、deb包）都需要提前下载并拷贝到待安装服务器上。
2. 由于源码编译安装需要非常熟悉软件的调优参数，除非定制，一般不建议选择源码编译安装；
3. 离线安装包（rpm包、deb包）的方式不适用于多环境规范化部署，因此生产中也不建议使用；
4. 生产环境中，为了多环境部署的规范性，建议使用离线安装之二进制安装包。

# 软件说明

## 软件版本说明

- 操作系统：RedHat 7.2 和 LTS Ubuntu 16.04
- Nginx：Nginx 1.12.2 和 Nginx 1.14.1

## 软件下载地址

本软件及依赖软件的下载地址，包括：

- 二进制包：http://nginx.org/download/nginx-1.14.1.tar.gz
- epel地址：https://mirrors.ustc.edu.cn/epel//7/x86_64/Packages/e/epel-release-7-11.noarch.rpm

# 前提条件

## 依赖关系包

安装必要的软件包：
**RedHat系统依赖包**



```bash
yum install -y openssl zlib pcre
```

Ubuntu系统依赖包

安装gcc g++的依赖库

```bash
sudo apt-get install build-essential sudo apt-get install libtool
```

安装pcre依赖库（http://www.pcre.org/）
```bash
sudo apt-get update sudo apt-get install libpcre3 libpcre3-dev

```

安装zlib依赖库（[http://www.zlib.net](http://www.zlib.net/)）

```bash
sudo apt-get install zlib1g-dev
```

# 安装步骤

## 在线安装_在RedHat上安装

使用本教程使用`.rpm` 软件包在Red Hat Enterprise Linux或CentOS Linux版本6和7上安装Nginx 1.12.2。

### 安装EPEL源

首先我们需要安装一个叫“epel-release”的软件包，这个软件包会自动配置yum的软件仓库。到下面的网址找你对应的CentOS版本和计算机架构：http://download.fedoraproject.org/pub/epel

64位的CentOS7，对应的地址是：https://mirrors.ustc.edu.cn/epel//7/x86_64/Packages/e/epel-release-7-11.noarch.rpm



```bash
wget https://mirrors.ustc.edu.cn/epel//7/x86_64/Packages/e/epel-release-7-11.noarch.rpm yum localinstall -y epel-release-7-11.noarch.rpm
```

### 安装Nginx

```bash
yum install -y nginx
```



## 在线安装_在Ubuntu上安装

使用本教程在LTS Ubuntu Linux系统上安装Nginx 1.12.2。

### 安装Nginx

```bash
sudo apt-get install nginx
```

## 离线安装_二进制手动安装

在下面的示例中，我们在/usr/local/nginx目录中安装Nginx 1.14.1

### 下载软件

```bash
 wget http://nginx.org/download/nginx-1.14.1.tar.gz
```

### 解压软件
```bash
 tar -zxvf nginx-1.14.1.tar.gz
```

### 进入解压目录

```bash
 cd nginx-1.14.1
```

### 配置
```bash
 ./configure --prefix=/usr/local/nginx
```

### 编译安装

```bash
 make && make install 
```

# 配置步骤

## 在线源YUM安装的启动方式

如果是通过在线源YUM安装，则无需额外配置直接启动nginx服务
**启动服务**

```bash
 systemctl start nginx
```

**停止服务**

```bash
 systemctl stop nginx
```

**重启服务**



```bash
systemctl restart nginx
```


在线源DEB安装的启动方式

nginx服务在安装后启动命令：

```bash
 sudo service nginx start
```



使用以下命令停止nginx服务器：

```bash
 sudo service nginx stop

```

要重新启动nginx服务器，请使用以下命令：

```bash
 sudo service nginx restart
```



## 二进制包安装的启动方式

```bash
 sudo /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
注意：-c 指定配置文件的路径，不加的话，nginx会自动加载默认路径的配置文件，可以通过-h查看帮助命令。 
```

# 验证

执行到这一步能够成功将服务启动。

查看后台进程是否启动 `ps -ef|grer nginx`

查看监听端口是否启动 `ss -luntp | grep nginx`

# Nginx配置文件详解

```bash
nginx进程数，建议设置为等于CPU总核心数.
worker_processes 8;

全局错误日志定义类型，[ debug | info | notice | warn | error | crit ]
error_log /var/log/nginx/error.log info;

进程文件
pid /var/run/nginx.pid;

一个nginx进程打开的最多文件描述符数目，理论值应该是最多打开文件数（系统的值ulimit -n）与nginx进程数相除，但是nginx分配请求并不均匀，所以建议与ulimit -n的值保持一致。
worker_rlimit_nofile 65535;

工作模式与连接数上限
events
{
　　#参考事件模型，use [ kqueue | rtsig | epoll | /dev/poll | select | poll ]; epoll模型是Linux 2.6以上版本内核中的高性能网络I/O模型，如果跑在FreeBSD上面，就用kqueue模型。
　　use epoll;
　　#单个进程最大连接数（最大连接数=连接数*进程数）
　　worker_connections 65535;
}

设定http服务器
http
{

include mime.types; #文件扩展名与文件类型映射表
default_type application/octet-stream; #默认文件类型
#charset utf-8; #默认编码
server_names_hash_bucket_size 128; #服务器名字的hash表大小
client_header_buffer_size 32k; #上传文件大小限制
large_client_header_buffers 4 64k; #设定请求缓
client_max_body_size 8m; #设定请求缓
sendfile on; #开启高效文件传输模式，sendfile指令指定nginx是否调用sendfile函数来输出文件，对于普通应用设为 on，如果用来进行下载等应用磁盘IO重负载应用，可设置为off，以平衡磁盘与网络I/O处理速度，降低系统的负载。注意：如果图片显示不正常把这个改成off。
autoindex on; #开启目录列表访问，合适下载服务器，默认关闭。
tcp_nopush on; #防止网络阻塞
tcp_nodelay on; #防止网络阻塞
keepalive_timeout 120; #长连接超时时间，单位是秒

#FastCGI相关参数是为了改善网站的性能：减少资源占用，提高访问速度。下面参数看字面意思都能理解。
fastcgi_connect_timeout 300;
fastcgi_send_timeout 300;
fastcgi_read_timeout 300;
fastcgi_buffer_size 64k;
fastcgi_buffers 4 64k;
fastcgi_busy_buffers_size 128k;
fastcgi_temp_file_write_size 128k;

#gzip模块设置
gzip on; #开启gzip压缩输出
gzip_min_length 1k; #最小压缩文件大小
gzip_buffers 4 16k; #压缩缓冲区
gzip_http_version 1.0; #压缩版本（默认1.1，前端如果是squid2.5请使用1.0）
gzip_comp_level 2; #压缩等级
gzip_types text/plain application/x-javascript text/css application/xml;
#压缩类型，默认就已经包含text/html，所以下面就不用再写了，写上去也不会有问题，但是会有一个warn。
gzip_vary on;
#limit_zone crawler $binary_remote_addr 10m; #开启限制IP连接数的时候需要使用

upstream blog.ha97.com {
#upstream的负载均衡，weight是权重，可以根据机器配置定义权重。weigth参数表示权值，权值越高被分配到的几率越大。
server 192.168.80.121:80 weight=3;
server 192.168.80.122:80 weight=2;
server 192.168.80.123:80 weight=3;

}

虚拟主机的配置
server
{

listen 80;　　　　#监听端口

　　　　server_name aa.cn www.aa.cn ; #server_name end  #域名可以有多个，用空格隔开

index index.html index.htm index.php;  # 设置访问主页

　　　　set $subdomain '';  # 绑定目录为二级域名 bbb.aa.com  根目录 /bbb  文件夹
　　　　if ( $host ~* "(?:(\w+\.){0,})(\b(?!www\b)\w+)\.\b(?!(com|org|gov|net|cn)\b)\w+\.[a-zA-Z]+" ) { set $subdomain "/$2"; }

root /home/wwwroot/aa.cn/web$subdomain;# 访问域名跟目录  

include rewrite/dedecms.conf; #rewrite end   #载入其他配置文件


location ~ .*.(php|php5)?$
{
　　fastcgi_pass 127.0.0.1:9000;
　　fastcgi_index index.php;
　　include fastcgi.conf;
}
#图片缓存时间设置
location ~ .*.(gif|jpg|jpeg|png|bmp|swf)$
{
　　expires 10d;
}
#JS和CSS缓存时间设置
location ~ .*.(js|css)?$
{
　　expires 1h;
}

}

日志格式设定

log_format access '$remote_addr - $remote_user [$time_local] "$request" '
'$status $body_bytes_sent "$http_referer" '
'"$http_user_agent" $http_x_forwarded_for';
#定义本虚拟主机的访问日志
access_log /var/log/nginx/ha97access.log access;

#对 "/" 启用反向代理
location / {

proxy_pass http://127.0.0.1:88;
proxy_redirect off;
proxy_set_header X-Real-IP $remote_addr;
#后端的Web服务器可以通过X-Forwarded-For获取用户真实IP
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
#以下是一些反向代理的配置，可选。
proxy_set_header Host $host;
client_max_body_size 10m; #允许客户端请求的最大单文件字节数
client_body_buffer_size 128k; #缓冲区代理缓冲用户端请求的最大字节数，
proxy_connect_timeout 90; #nginx跟后端服务器连接超时时间(代理连接超时)
proxy_send_timeout 90; #后端服务器数据回传时间(代理发送超时)
proxy_read_timeout 90; #连接成功后，后端服务器响应时间(代理接收超时)
proxy_buffer_size 4k; #设置代理服务器（nginx）保存用户头信息的缓冲区大小
proxy_buffers 4 32k; #proxy_buffers缓冲区，网页平均在32k以下的设置
proxy_busy_buffers_size 64k; #高负荷下缓冲大小（proxy_buffers*2）
proxy_temp_file_write_size 64k;
#设定缓存文件夹大小，大于这个值，将从upstream服务器传

}
设定查看Nginx状态的地址

location /NginxStatus {

stub_status on;
access_log on;
auth_basic "NginxStatus";
auth_basic_user_file conf/htpasswd;
#htpasswd文件的内容可以用apache提供的htpasswd工具来产生。

}

#本地动静分离反向代理配置
#所有jsp的页面均交由tomcat或resin处理
location ~ .(jsp|jspx|do)?$ {

proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_pass http://127.0.0.1:8080;

}
#所有静态文件由nginx直接读取不经过tomcat或resin
location ~ .*.(htm|html|gif|jpg|jpeg|png|bmp|swf|ioc|rar|zip|txt|flv|mid|doc|ppt|pdf|xls|mp3|wma)$
{　　 expires 15d;　　 }
location ~ .*.(js|css)?$
{ 　　expires 1h;　　 }

}
```
