---
title: Docker 部署 2048游戏网站
---

> 2018-05-23 


## 1. 2048游戏网站的快速搭建

| 操作                                                         | 命令 |
| ------------------------------------------------------------ | ---- |
| 在线仓库寻找dao-2048镜像                                     |      |
| 将注册/仓库服务器上的镜像下载到本地                          |      |
| 显示本地已有镜像                                             |      |
| 显示docker的系统信息                                         |      |
| 将dao-2048镜像运行在内存中，创建并运行容器 2048，放在后台运行，映射80端口 |      |

## 2. 熟悉Docker容器命令

| 操作                                                         | 命令 |
| ------------------------------------------------------------ | ---- |
| 停止容器2048                                                 |      |
| 启动容器2048                                                 |      |
| 查看容器2048日志                                             |      |
| 查看当前正在运行或之前运行过的容器                           |      |
| 在当前运行的容器2048里新建文档`/test.txt`                    |      |
| 查看容器2048运行中的存储资源变化                             |      |
| 将宿主机上的文件`/tmp/file.out`拷贝到容器2048中的`/etc`目录下，并验证 |      |
| 将容器2045中的`/etc/hosts`文件拷贝到宿主机`/tmp`目录下，并验证 |      |
| 更新镜像`dao-2048`的名称标记为`dao-2048:0.01`                |      |
| 从修改过的容器2048创建新的镜像`dao-2048:0.02`，也叫做容器固化 |      |
| 导出`容器2048`的文件系统到一个`2048.tar`包归档文件中         |      |
| 再次停止容器2048                                             |      |
| 删除容器2048                                                 |      |

## 3. 熟悉Docker镜像命令

| 操作                                              | 命令 |
| ------------------------------------------------- | ---- |
| 删除镜像`dao-2048`                                |      |
| 将`dao-2048:0.02`镜像在本地保存为`dao.tar`镜像包  |      |
| 删除镜像  `dao-2048:0.02`                         |      |
| 再将刚才保存的`dao.tar`镜像文件重新装载到本地系统 |      |
| 显示本地已有镜像                                  |      |



### Docker 命令参考

#### Docker 管理命令

| 命令    | 描述                         |
| ------- | ---------------------------- |
| dockerd | 启动docker守护进程           |
| info    | 显示系统范围的信息           |
| inspect | 返回容器或镜像上的低级别信息 |
| version | 显示docker的版本信息         |

#### Docker 镜像命令

| 命令    | 描述                                      |
| ------- | ----------------------------------------- |
| build   | 根据Dockerfile创建镜像                    |
| commit  | 从更改的容器基础上创建新镜像              |
| export  | 导出容器的文件系统到一个tar包归档文件中   |
| history | 查看镜像创建的历史过程                    |
| images  | 列出本地镜像                              |
| import  | 从一个tar包归档文件导入容器的文件系统系统 |
| load    | 通过tar文件或标准输入载入镜像             |
| rmi     | 删除一个或多个镜像                        |
| save    | 保存镜像到一个tar包归档文件               |
| tag     | 标记镜像                                  |

#### Docker 容器命令

| 命令    | 描述                                     |
| ------- | ---------------------------------------- |
| attach  | 连接到运行的容器                         |
| cp      | 在容器和宿主机之间传输文件或文件夹       |
| create  | 创建一个新容器，但并不马上运行           |
| diff    | 检查容器在文件系统上的变化               |
| events  | 从服务器获取实时事件                     |
| exec    | 在容器内运行程序                         |
| kill    | 杀掉一个正在运行的容器，如果我们停不下它 |
| logs    | 取出容器运行时的日志                     |
| pause   | 在容器内冻结所有程序                     |
| port    | 列出端口映射或用于容器的特定映射         |
| ps      | 列出当前系统内的容器                     |
| rename  | 为容器改名                               |
| restart | 重启正在运行的容器                       |
| rm      | 删除一个或多个容器                       |
| run     | 运行一个新的容器                         |
| start   | 启动一个或多个后台停止的容器             |
| stats   | 显示容器资源使用统计信息的实时流         |
| stop    | 停止一个正在运行的容器                   |
| top     | 显示正在容器内运行的进程信息             |
| unpause | 解冻容器内之前冻结的进程                 |
| update  | 更新一个或多个容器的更新配置             |
| wait    | 阻塞直到某个容器关闭                     |

#### Docker 注册/仓库操作命令

| 命令   | 描述                            |
| ------ | ------------------------------- |
| login  | 注册或登录注册/仓库服务器       |
| logout | 从注册/仓库服务器退出           |
| pull   | 从注册/仓库服务器下载镜像       |
| push   | 将本地镜像上传到注册/仓库服务器 |
| search | 在Docker Hub上搜索镜像          |

#### Docker 网络和连接命令

| 命令               | 描述                           |
| ------------------ | ------------------------------ |
| network connect    | 连接容器到特定Docker网络       |
| network create     | 创建一个新的Docker网络         |
| network disconnect | 断开容器到特定Docker网络的连接 |
| network inspect    | 显示特定Docker网络的信息       |
| network ls         | 列出当前Docker 环境下的网络    |
| network rm         | 删除一个或多个Docker网络       |

#### Docker 共享资源卷管理命令

| 命令           | 描述                               |
| -------------- | ---------------------------------- |
| volume create  | 创建一个新的卷，用来给容器存储数据 |
| volume inspect | 显示一个卷的信息                   |
| volume ls      | 列出现有Docker环境下的卷           |
| volume rm      | 删除一个或多个Docker卷             |

## 答案

### 1. 2048游戏网站的快速搭建

```shell
[root@mastera ~]# docker search dao-2048
NAME                          DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
alexwhen/docker-2048          simple is better                                13                                      [OK]
daocloud/builder-image-base                                                   1                                       
cpk1224/docker-2048                                                           1                                       [OK]
blackicebird/2048             2048 with logging                               1                                       
jonneywu/dao-2048             wode2048                                        0                                       [OK]
runseb/2048                   The 2048 game                                   0                                       
ttmass/dao-2048               sdas                                            0                                       [OK]
gilgameshs/dao-2048           自动构建demo                                        0                                       [OK]
ckeyer/2048                   2048                                            0                                       [OK]
suchja/ng2-2048               Angular 2 implementation of game 2048 (sourc…   0                                       [OK]
yankay/dao-2048               haha                                            0                                       [OK]
daocloud/ci-python                                                            0                                       
ponsfrilus/2048nginx          A nginx containter wich run 2048                0                                       [OK]
daocloud/ci-golang                                                            0                                       
daocloud/ci-java                                                              0                                       
china394337002/dao-2048       This is my first automated build.               0                                       [OK]
jgreat/2048                   2048 game by Gabriele Cirulli                   0                                       
faycheng/dao-2048                                                             0                                       
ckeyergame/2048               2048                                            0                                       [OK]
tobert/f7u12-2048             My clone of 2048 in Javascript with no exter…   0                                       
daowen/waiter-kitchen                                                         0                                       
boycee/docker-2048            Docker version of 2048                          0                                       
daocloud/ci-mysql                                                             0                                       
evilroot/docker-2048          A smaller docker version of 2048 Forked from…   0                                       [OK]
daocloud/ci-node                                                              0                                       

[root@mastera ~]# docker pull alexwhen/docker-2048
Using default tag: latest
latest: Pulling from alexwhen/docker-2048
c862d82a67a2: Pull complete 
a3ed95caeb02: Pull complete 
69dbbd8c451d: Pull complete 
e9b345a0f742: Pull complete 
Digest: sha256:4913452e5bd092db9c8b005523127b8f62821867021e23a9acb1ae0f7d2432e1
Status: Downloaded newer image for alexwhen/docker-2048:latest


[root@mastera ~]# docker images
REPOSITORY             TAG                 IMAGE ID            CREATED             SIZE
alexwhen/docker-2048   latest              7929bcd70e47        2 years ago         8.01MB


[root@mastera ~]# docker images
REPOSITORY             TAG                 IMAGE ID            CREATED             SIZE
alexwhen/docker-2048   latest              7929bcd70e47        2 years ago         8.01MB
[root@mastera ~]# docker info
Containers: 0
 Running: 0
 Paused: 0
 Stopped: 0
Images: 1
Server Version: 18.05.0-ce
Storage Driver: devicemapper
 Pool Name: docker-8:3-1049277-pool
 Pool Blocksize: 65.54kB
 Base Device Size: 10.74GB
 Backing Filesystem: xfs
 Udev Sync Supported: true
 Data file: /dev/loop0
 Metadata file: /dev/loop1
 Data loop file: /var/lib/docker/devicemapper/devicemapper/data
 Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata
 Data Space Used: 39.98MB
 Data Space Total: 107.4GB
 Data Space Available: 1.235GB
 Metadata Space Used: 643.1kB
 Metadata Space Total: 2.147GB
 Metadata Space Available: 1.235GB
 Thin Pool Minimum Free Space: 10.74GB
 Deferred Removal Enabled: true
 Deferred Deletion Enabled: true
 Deferred Deleted Device Count: 0
 Library Version: 1.02.146-RHEL7 (2018-01-22)
Logging Driver: json-file
Cgroup Driver: cgroupfs
Plugins:
 Volume: local
 Network: bridge host macvlan null overlay
 Log: awslogs fluentd gcplogs gelf journald json-file logentries splunk syslog
Swarm: inactive
Runtimes: runc
Default Runtime: runc
Init Binary: docker-init
containerd version: 773c489c9c1b21a6d78b5c538cd395416ec50f88
runc version: 4fc53a81fb7c994640722ac585fa9ca548971871
init version: 949e6fa
Security Options:
 seccomp
  Profile: default
Kernel Version: 3.10.0-327.el7.x86_64
Operating System: Red Hat Enterprise Linux Server 7.2 (Maipo)
OSType: linux
Architecture: x86_64
CPUs: 2
Total Memory: 1.782GiB
Name: mastera
ID: DBLV:5WPW:PMBT:RLBI:BJDX:DYKB:Z2OB:DEMH:ORXZ:PSFY:FA2B:HEPL
Docker Root Dir: /var/lib/docker
Debug Mode (client): false
Debug Mode (server): false
Registry: https://index.docker.io/v1/
Labels:
Experimental: false
Insecure Registries:
 127.0.0.0/8
Live Restore Enabled: false

WARNING: devicemapper: usage of loopback devices is strongly discouraged for production use.
         Use `--storage-opt dm.thinpooldev` to specify a custom block storage device.
WARNING: bridge-nf-call-iptables is disabled
WARNING: bridge-nf-call-ip6tables is disabled

[root@mastera ~]# docker run -d --name 2048 -p 80:80 alexwhen/docker-2048
6bc2ec292820fe3ae23553e00f29891346f60d40a5e96f7c14004306d2fe7519
```

### 2. 熟悉Docker容器命令

```shell
[root@mastera ~]# docker ps
CONTAINER ID        IMAGE                  COMMAND                  CREATED              STATUS              PORTS                NAMES
6bc2ec292820        alexwhen/docker-2048   "nginx -g 'daemon of…"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp   2048
[root@mastera ~]# docker stop 2048
2048
[root@mastera ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
[root@mastera ~]# docker ps -a
CONTAINER ID        IMAGE                  COMMAND                  CREATED             STATUS                      PORTS               NAMES
6bc2ec292820        alexwhen/docker-2048   "nginx -g 'daemon of…"   2 minutes ago       Exited (0) 12 seconds ago  
[root@mastera ~]# docker start 2048
2048
[root@mastera ~]# docker logs 2048
[root@mastera ~]# docker ps -a
CONTAINER ID        IMAGE                  COMMAND                  CREATED             STATUS              PORTS                NAMES
6bc2ec292820        alexwhen/docker-2048   "nginx -g 'daemon of…"   4 minutes ago       Up 38 seconds       0.0.0.0:80->80/tcp   2048
[root@mastera ~]# docker exec 2048 touch /test.txt
[root@mastera ~]# docker exec 2048 ls /test.txt
/test.txt
[root@mastera ~]# docker exec 2048 ls -l /test.txt
-rw-r--r--    1 root     root             0 Jun  6 10:47 /test.txt
[root@mastera ~]# docker diff 2048 
C /var
C /var/lib
C /var/lib/nginx
C /var/lib/nginx/tmp
A /var/lib/nginx/tmp/proxy
A /var/lib/nginx/tmp/scgi
A /var/lib/nginx/tmp/uwsgi
A /var/lib/nginx/tmp/client_body
A /var/lib/nginx/tmp/fastcgi
C /var/log
C /var/log/nginx
A /var/log/nginx/access.log
A /var/log/nginx/error.log
C /var/run
C /var/run/nginx
A /var/run/nginx/nginx.pid
A /test.txt
[root@mastera ~]# touch /tmp/file.out
[root@mastera ~]# docker cp /tmp/file.out 2048:/etc
[root@mastera ~]# docker exec 2048 ls -l /etc/file.out
-rw-r--r--    1 root     root             0 Jun  6 10:49 /etc/file.out
[root@mastera ~]# docker cp 2048:/etc/hosts /tmp
[root@mastera ~]# ll /tmp/hosts 
-rw-r--r-- 1 root root 174 Jun  6 03:46 /tmp/hosts
[root@mastera ~]# cat /tmp/hosts 
127.0.0.1	localhost
::1	localhost ip6-localhost ip6-loopback
fe00::0	ip6-localnet
ff00::0	ip6-mcastprefix
ff02::1	ip6-allnodes
ff02::2	ip6-allrouters
172.17.0.2	6bc2ec292820


# 标记容器的镜像为新的版本
[root@mastera ~]# docker images
REPOSITORY             TAG                 IMAGE ID            CREATED             SIZE
alexwhen/docker-2048   latest              7929bcd70e47        2 years ago         8.01MB
[root@mastera ~]# docker tag alexwhen/docker-2048 booboo-2048
[root@mastera ~]# docker images
REPOSITORY             TAG                 IMAGE ID            CREATED             SIZE
alexwhen/docker-2048   latest              7929bcd70e47        2 years ago         8.01MB
booboo-2048            latest              7929bcd70e47        2 years ago         8.01MB
[root@mastera ~]# docker tag alexwhen/docker-2048 dao-2048:0.01
[root@mastera ~]# docker images
REPOSITORY             TAG                 IMAGE ID            CREATED             SIZE
alexwhen/docker-2048   latest              7929bcd70e47        2 years ago         8.01MB
booboo-2048            latest              7929bcd70e47        2 years ago         8.01MB
dao-2048               0.01                7929bcd70e47        2 years ago         8.01MB

# 固化容器为新的image镜像
[root@mastera ~]# docker commit -a booboowei 2048 dao-2048:0.02
sha256:141c918abbbf7c272088b1b1987c9b4fd6307c846b7f7986cdecb0f11ada63c0
[root@mastera ~]# docker images
REPOSITORY             TAG                 IMAGE ID            CREATED             SIZE
dao-2048               0.02                141c918abbbf        11 seconds ago      8.01MB
alexwhen/docker-2048   latest              7929bcd70e47        2 years ago         8.01MB
booboo-2048            latest              7929bcd70e47        2 years ago         8.01MB
dao-2048               0.01                7929bcd70e47        2 years ago         8.01MB

# 导出容器的文件系统
[root@mastera ~]# docker export 2048 > 2048.tar

[root@mastera ~]# docker stop 2048
2048
[root@mastera ~]# docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                     PORTS               NAMES
6bc2ec292820        7929bcd70e47        "nginx -g 'daemon of…"   21 minutes ago      Exited (0) 4 minutes ago                       2048
[root@mastera ~]# docker rm 2048
2048
[root@mastera ~]# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES2048

```

### 3. 熟悉Docker镜像命令

```shell
[root@mastera ~]# docker rmi alexwhen/docker-2048
Untagged: alexwhen/docker-2048:latest
Untagged: alexwhen/docker-2048@sha256:4913452e5bd092db9c8b005523127b8f62821867021e23a9acb1ae0f7d2432e1
[root@mastera ~]# docker save dao-2048:0.02 > dao.tar
[root@mastera ~]# docker rmi dao-2048:0.02
Untagged: dao-2048:0.02
Deleted: sha256:141c918abbbf7c272088b1b1987c9b4fd6307c846b7f7986cdecb0f11ada63c0
Deleted: sha256:b8df0be97a09b39f23a9d932743eb140aba3d11726b742a6198081758535b500
[root@mastera ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
booboo-2048         latest              7929bcd70e47        2 years ago         8.01MB
dao-2048            0.01                7929bcd70e47        2 years ago         8.01MB
[root@mastera ~]# docker load < dao.tar
35ab183cea2a: Loading layer  11.26kB/11.26kB
Loaded image: dao-2048:0.02
[root@mastera ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
dao-2048            0.02                141c918abbbf        7 minutes ago       8.01MB
booboo-2048         latest              7929bcd70e47        2 years ago         8.01MB
dao-2048            0.01                7929bcd70e47        2 years ago         8.01MB
```

