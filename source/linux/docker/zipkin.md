---
title: Docker部署zipkin
---

# Zipkin简介

## Zipkin是什么

 Zipkin的官方介绍：https://zipkin.io/

 Zipkin是一款开源的分布式实时数据追踪系统（Distributed Tracking System），基于 Google Dapper的论文设计而来，由 Twitter 公司开发贡献。其主要功能是聚集来自各个异构系统的实时监控数据。分布式跟踪系统还有其他比较成熟的实现，例如：Naver的Pinpoint、Apache的HTrace、阿里的鹰眼Tracing、京东的Hydra、新浪的Watchman，美团点评的CAT，skywalking等。

## 为什么用Zipkin

 随着业务越来越复杂，系统也随之进行各种拆分，特别是随着微服务架构和容器技术的兴起，看似简单的一个应用，后台可能有几十个甚至几百个服务在支撑；一个前端的请求可能需要多次的服务调用最后才能完成；当请求变慢或者不可用时，我们无法得知是哪个后台服务引起的，这时就需要解决如何快速定位服务故障点，Zipkin分布式跟踪系统就能很好的解决这样的问题。

Zipkin的一些基本概念？

### Brave

Brave 是用来装备 Java 程序的类库，提供了面向 Standard Servlet、Spring MVC、Http Client、JAX RS、Jersey、Resteasy 和 MySQL 等接口的装备能力，可以通过编写简单的配置和代码，让基于这些框架构建的应用可以向 Zipkin报告数据。同时 Brave 也提供了非常简单且标准化的接口，在以上封装无法满足要求的时候可以方便扩展与定制。

如下图是 Brave 的结构图。Brave 利用 reporter 向 Zipkin的 Collector 发送 trace 信息。

![](https://img2018.cnblogs.com/blog/1153954/201905/1153954-20190528211846969-1126002065.png)

Brave 主要是利用拦截器在请求前和请求后分别埋点。例如 Spingmvc 监控使用 Interceptors，Mysql 监控使用 statementInterceptors。同理 Dubbo 的监控是利用 com.alibaba.dubbo.rpc.Filter 来过滤生产者和消费者的请求。

### traceId
一次请求全局只有一个traceId。用来在海量的请求中找到同一链路的几次请求。比如servlet服务器接收到用户请求，调用dubbo服务，然后将结果返回给用户，整条链路只有一个traceId。开始于用户请求，结束于用户收到结果。

### spanId
一个链路中每次请求都会有一个spanId。例如一次rpc，一次sql都会有一个单独的spanId从属于traceId。

### cs
Clent Sent 客户端发起请求的时间，比如 dubbo 调用端开始执行远程调用之前。

### cr
Client Receive 客户端收到处理完请求的时间。

### ss
Server Receive 服务端处理完逻辑的时间。

### sr
Server Receive 服务端收到调用端请求的时间。

```
sr - cs = 请求在网络上的耗时
ss - sr = 服务端处理请求的耗时
cr - ss = 回应在网络上的耗时
cr - cs = 一次调用的整体耗时
```

## Zipkin的工作过程

当用户发起一次调用时，Zipkin 的客户端会在入口处为整条调用链路生成一个全局唯一的 trace id，并为这条链路中的每一次分布式调用生成一个 span id。span 与 span 之间可以有父子嵌套关系，代表分布式调用中的上下游关系。span 和 span 之间可以是兄弟关系，代表当前调用下的两次子调用。一个 trace 由一组 span 组成，可以看成是由 trace 为根节点，span 为若干个子节点的一棵树。

Zipkin 会将 trace 相关的信息在调用链路上传递，并在每个调用边界结束时异步的把当前调用的耗时信息上报给 Zipkin Server。Zipkin Server 在收到 trace 信息后，将其存储起来。随后 Zipkin 的 Web UI 会通过 API 访问的方式从存储中将 trace 信息提取出来分析并展示。

![](https://img2018.cnblogs.com/blog/1153954/201905/1153954-20190529124054604-116989993.png)

# 安装Zipkin

Zipkin的 github 地址：https://github.com/apache/incubator-zipkin

## 安装Docker

CentOS 7 (使用yum进行安装)

> step 1: 安装必要的一些系统工具

```bash
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

> Step 2: 添加软件源信息

```bash
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
cat /etc/yum.repos.d/centos.repo
[centos7]
name='centos'
baseurl='http://mirror.centos.org/centos/7/os/x86_64/'
enabled=1
gpgcheck=0

[centos7-extra]
name='centos extra'
baseurl='http://mirror.centos.org/centos/7/extras/x86_64/'
enabled=1
gpgcheck=0
```

> Step 3: 更新并安装Docker-CE

```bash
sudo yum makecache fast
sudo yum list docker-ce --showduplicates | sort -r
sudo yum -y install pigz container-selinux
sudo yum -y install docker-ce
```

> Step 4: 开启Docker服务

```bash
sudo systemctl  start docker
docker run hello-world
```

## docker-compose

Compose 是用于定义和运行多容器 Docker 应用程序的工具。通过 Compose，您可以使用 YML 文件来配置应用程序需要的所有服务。然后，使用一个命令，就可以从 YML 文件配置中创建并启动所有服务。

```bash
curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
docker-compose --version
```

## docker部署zipkin

```bash
docker run -d -p 9411:9411 openzipkin/zipkin 
```

运行结果

```bash
[root@foundation0 ~]# docker pull openzipkin/zipkin
Using default tag: latest
latest: Pulling from openzipkin/zipkin
Digest: sha256:096200b68b0c56ab3446622a94c8d1a64c849b36d068c3bcf0799c73435d2e48
Status: Image is up to date for openzipkin/zipkin:latest
docker.io/openzipkin/zipkin:latest
[root@foundation0 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
openzipkin/zipkin   latest              40f2f21f6707        9 days ago          158MB
hello-world         latest              bf756fb1ae65        6 months ago        13.3kB

[root@foundation0 ~]# docker run -d --restart always -p 9411:9411 --name zipkin openzipkin/zipkin
f0887b77042cdd151f9366f9830111e69446e860ff8c276802bea3b50f6c5efc
[root@foundation0 ~]# ps -ef|grep 9411
root     25957  6391  0 17:01 ?        00:00:00 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 9411 -container-ip 172.17.0.2 -container-port 9411
root     26054  5611  0 17:01 pts/1    00:00:00 grep --color=auto 9411
[root@foundation0 ~]# docker ps -a
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS                         PORTS                              NAMES
f0887b77042c        openzipkin/zipkin   "/busybox/sh run.sh"   16 seconds ago      Up 14 seconds                  9410/tcp, 0.0.0.0:9411->9411/tcp   zipkin
9856b62b0130        hello-world         "/hello"               About an hour ago   Exited (0) About an hour ago                                      clever_engelbart
```


```bash
# 10.200.6.53
grant all on ziplin.* to 'ziplin'@'%' identified by 'Zyadmin@123';
#服务器任意目录创建zipkinDocker文件夹
#新建以下文件
vi docker-compose.yml
#编写内容
#注意mysql数据库版本最好是5.6-5.7的，我用mysql8.0连接不成功
version: '2'

services:
  # The zipkin process services the UI, and also exposes a POST endpoint that
  # instrumentation can send trace data to. Scribe is disabled by default.
  zipkin:
    image: openzipkin/zipkin
    container_name: zipkin
    environment:
      - STORAGE_TYPE=mysql
      # Point the zipkin at the storage backend
      - MYSQL_DB=zipkin
      - MYSQL_USER=zipkin
      - MYSQL_PASS=Zyadmin#123
      - MYSQL_HOST=你的数据库IP地址
      - MYSQL_TCP_PORT=3306
      # Uncomment to enable scribe
      # - SCRIBE_ENABLED=true
      # Uncomment to enable self-tracing
      # - SELF_TRACING_ENABLED=true
      # Uncomment to enable debug logging
      # - JAVA_OPTS=-Dlogging.level.zipkin=DEBUG -Dlogging.level.zipkin2=DEBUG
    network_mode: host
    ports:
      # Port used for the Zipkin UI and HTTP Api
      - 9411:9411
      # Uncomment if you set SCRIBE_ENABLED=true
      # - 9410:9410
    #networks: 
    #  - default 
    #  - my_net #创建网路 docker network create my_net 删除网络 docker network rm my_net
#networks: 
  #my_net: 
    #external: true

create database zipkin;

CREATE TABLE IF NOT EXISTS zipkin_spans
(
    trace_id_high BIGINT       NOT NULL DEFAULT 0 COMMENT 'If non zero, this means the trace uses 128 bit traceIds instead of 64 bit',
    `trace_id`    BIGINT       NOT NULL,
    `id`          BIGINT       NOT NULL,
    `name`        VARCHAR(255) NOT NULL,
    `parent_id`   BIGINT,
    `debug`       BIT(1),
    `start_ts`    BIGINT COMMENT 'Span.timestamp(): epoch micros used for endTs query and to implement TTL',
    `duration`    BIGINT COMMENT 'Span.duration(): micros used for minDuration and maxDuration query'
) ENGINE = InnoDB
  ROW_FORMAT = COMPRESSED
  CHARACTER SET = utf8
  COLLATE utf8_general_ci;
ALTER TABLE zipkin_spans
    ADD UNIQUE KEY (`trace_id_high`, `trace_id`, `id`) COMMENT 'ignore insert on duplicate';
ALTER TABLE zipkin_spans
    ADD INDEX (`trace_id_high`, `trace_id`, `id`) COMMENT 'for joining with zipkin_annotations';
ALTER TABLE zipkin_spans
    ADD INDEX (`trace_id_high`, `trace_id`) COMMENT 'for getTracesByIds';
ALTER TABLE zipkin_spans
    ADD INDEX (`name`) COMMENT 'for getTraces and getSpanNames';
ALTER TABLE zipkin_spans
    ADD INDEX (`start_ts`) COMMENT 'for getTraces ordering and range';
CREATE TABLE IF NOT EXISTS zipkin_annotations
(
    `trace_id_high`         BIGINT       NOT NULL DEFAULT 0 COMMENT 'If non zero, this means the trace uses 128 bit traceIds instead of 64 bit',
    `trace_id`              BIGINT       NOT NULL COMMENT 'coincides with zipkin_spans.trace_id',
    `span_id`               BIGINT       NOT NULL COMMENT 'coincides with zipkin_spans.id',
    `a_key`                 VARCHAR(255) NOT NULL COMMENT 'BinaryAnnotation.key or Annotation.value if type == -1',
    `a_value`               BLOB COMMENT 'BinaryAnnotation.value(), which must be smaller than 64KB',
    `a_type`                INT          NOT NULL COMMENT 'BinaryAnnotation.type() or -1 if Annotation',
    `a_timestamp`           BIGINT COMMENT 'Used to implement TTL; Annotation.timestamp or zipkin_spans.timestamp',
    `endpoint_ipv4`         INT COMMENT 'Null when Binary/Annotation.endpoint is null',
    `endpoint_ipv6`         BINARY(16) COMMENT 'Null when Binary/Annotation.endpoint is null, or no IPv6 address',
    `endpoint_port`         SMALLINT COMMENT 'Null when Binary/Annotation.endpoint is null',
    `endpoint_service_name` VARCHAR(255) COMMENT 'Null when Binary/Annotation.endpoint is null'
) ENGINE = InnoDB
  ROW_FORMAT = COMPRESSED
  CHARACTER SET = utf8
  COLLATE utf8_general_ci;
ALTER TABLE zipkin_annotations
    ADD UNIQUE KEY (`trace_id_high`, `trace_id`, `span_id`, `a_key`, `a_timestamp`) COMMENT 'Ignore insert on duplicate';
ALTER TABLE zipkin_annotations
    ADD INDEX (`trace_id_high`, `trace_id`, `span_id`) COMMENT 'for joining with zipkin_spans';
ALTER TABLE zipkin_annotations
    ADD INDEX (`trace_id_high`, `trace_id`) COMMENT 'for getTraces/ByIds';
ALTER TABLE zipkin_annotations
    ADD INDEX (`endpoint_service_name`) COMMENT 'for getTraces and getServiceNames';
ALTER TABLE zipkin_annotations
    ADD INDEX (`a_type`) COMMENT 'for getTraces and autocomplete values';
ALTER TABLE zipkin_annotations
    ADD INDEX (`a_key`) COMMENT 'for getTraces and autocomplete values';
ALTER TABLE zipkin_annotations
    ADD INDEX (`trace_id`, `span_id`, `a_key`) COMMENT 'for dependencies job';
CREATE TABLE IF NOT EXISTS zipkin_dependencies
(
    `day`         DATE         NOT NULL,
    `parent`      VARCHAR(255) NOT NULL,
    `child`       VARCHAR(255) NOT NULL,
    `call_count`  BIGINT,
    `error_count` BIGINT
) ENGINE = InnoDB
  ROW_FORMAT = COMPRESSED
  CHARACTER SET = utf8
  COLLATE utf8_general_ci;
ALTER TABLE zipkin_dependencies
    ADD UNIQUE KEY (`day`, `parent`, `child`);



root@MySQL-01 17:39:  [zipkin]> show tables;
+---------------------+
| Tables_in_zipkin    |
+---------------------+
| zipkin_annotations  |
| zipkin_dependencies |
| zipkin_spans        |
+---------------------+
3 rows in set (0.00 sec)


#启动服务编排，在docker-compose.yml所在文件目录执行
docker-compose up -d
Creating zipkin ... done
#查看服务日志
docker-compose logs

[root@foundation0 docker_booboo]# docker-compose up -d
Creating zipkin ... done
[root@foundation0 docker_booboo]# docker-compose logs
Attaching to zipkin
zipkin    | MySQL host: 10.200.6.53
zipkin    |
zipkin    |                   oo
zipkin    |                  oooo
zipkin    |                 oooooo
zipkin    |                oooooooo
zipkin    |               oooooooooo
zipkin    |              oooooooooooo
zipkin    |            ooooooo  ooooooo
zipkin    |           oooooo     ooooooo
zipkin    |          oooooo       ooooooo
zipkin    |         oooooo   o  o   oooooo
zipkin    |        oooooo   oo  oo   oooooo
zipkin    |      ooooooo  oooo  oooo  ooooooo
zipkin    |     oooooo   ooooo  ooooo  ooooooo
zipkin    |    oooooo   oooooo  oooooo  ooooooo
zipkin    |   oooooooo      oo  oo      oooooooo
zipkin    |   ooooooooooooo oo  oo ooooooooooooo
zipkin    |       oooooooooooo  oooooooooooo
zipkin    |           oooooooo  oooooooo
zipkin    |               oooo  oooo
zipkin    |
zipkin    |      ________ ____  _  _____ _   _
zipkin    |     |__  /_ _|  _ \| |/ /_ _| \ | |
zipkin    |       / / | || |_) | ' / | ||  \| |
zipkin    |      / /_ | ||  __/| . \ | || |\  |
zipkin    |     |____|___|_|   |_|\_\___|_| \_|
zipkin    |
zipkin    | :: version 2.21.5 :: commit 7f4f274 ::
zipkin    |
zipkin    | 2020-07-13 09:44:25.300  INFO 1 --- [           main] z.s.ZipkinServer                         : Starting ZipkinServer on foundation0.ilt.example.com with PID 1 (/zipkin/BOOT-INF/classes started by zipkin in /zipkin)
zipkin    | 2020-07-13 09:44:25.304  INFO 1 --- [           main] z.s.ZipkinServer                         : The following profiles are active: shared
zipkin    | 2020-07-13 09:44:26.161  INFO 1 --- [           main] o.s.s.c.ThreadPoolTaskExecutor           : Initializing ExecutorService
zipkin    | 2020-07-13 09:44:26.163  INFO 1 --- [           main] o.s.s.c.ThreadPoolTaskExecutor           : Initializing ExecutorService 'mysqlExecutor'
zipkin    | 2020-07-13 09:44:26.591  INFO 1 --- [           main] c.l.a.c.u.SystemInfo                     : hostname: foundation0.ilt.example.com (from /proc/sys/kernel/hostname)
zipkin    | 2020-07-13 09:44:26.994  INFO 1 --- [oss-http-*:9411] c.l.a.s.Server                           : Serving HTTP at /[0:0:0:0:0:0:0:0%0]:9411 - http://127.0.0.1:9411/
zipkin    | 2020-07-13 09:44:26.997  INFO 1 --- [           main] c.l.a.s.ArmeriaAutoConfiguration         : Armeria server started at ports: {/[0:0:0:0:0:0:0:0%0]:9411=ServerPort(/[0:0:0:0:0:0:0:0%0]:9411, [http])}
zipkin    | 2020-07-13 09:44:27.029  INFO 1 --- [           main] z.s.ZipkinServer                         : Started ZipkinServer in 2.658 seconds (JVM running for 3.605)
```



# python

https://blog.csdn.net/liumiaocn/article/details/80657943

Python项目依赖
为了在Python项目中使用zipkin，需要py_zipkin/pyramid/pyramid_zipkin 。在CentOS系Linux发行版上命令如下：


```
yum install python-devel
pip install –trusted-host pypi.org –trusted-host files.pythonhosted.org py_zipkin pyramid pyramid_zipkin
```
