---
title: Docker 部署 Archery
---

# 什么是Archery？

Archery是[archer](https://github.com/jly8866/archer)的分支项目，定位于SQL审核查询平台，旨在提升DBA的工作效率，支持多数据库的SQL上线和查询，同时支持丰富的MySQL运维功能，所有功能都兼容手机端操作。

## [Archery 官网](https://archerydms.com/)

[![star](https://camo.githubusercontent.com/cd15d499f18958f241e4b3fad674ec4cc6c609db8f80d8d21ee1527534d8666c/68747470733a2f2f67697465652e636f6d2f7274747474652f417263686572792f62616467652f737461722e7376673f7468656d653d677670)](https://gitee.com/rtttte/Archery) [![Build Status](https://camo.githubusercontent.com/55ab6c9a2f820b088e44a0f12e7c886d6c51d119be55114c8977f92158c6874a/68747470733a2f2f7472617669732d63692e6f72672f6868796f2f417263686572792e7376673f6272616e63683d6d6173746572)](https://travis-ci.org/hhyo/Archery) [![Release](https://camo.githubusercontent.com/34101a0678a8563a141d07446914ac8618f22efa35aac054fb6b521d14f7d7d8/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f72656c656173652f6868796f2f617263686572792e737667)](https://github.com/hhyo/archery/releases/) [![codecov](https://camo.githubusercontent.com/c9b1f70e174cc5dd2055574e1f6fc3c85e843d3f9af263c3cfd021d49fea73ea/68747470733a2f2f636f6465636f762e696f2f67682f6868796f2f617263686572792f6272616e63682f6d61737465722f67726170682f62616467652e737667)](https://codecov.io/gh/hhyo/archery) [![Codacy Badge](https://camo.githubusercontent.com/db59b93b6c48193bed49259ee8e69c549f0a1cc60e0947dc3ae267c1077c0d06/68747470733a2f2f6170692e636f646163792e636f6d2f70726f6a6563742f62616467652f47726164652f3934653835383765353037663435363561316561356561323166643934633332)](https://app.codacy.com/app/hhyo/Archery?utm_source=github.com&utm_medium=referral&utm_content=hhyo/Archery&utm_campaign=Badge_Grade_Dashboard) [![version](https://camo.githubusercontent.com/a66b4ae684ecc107849914cb2f06698e9608757ec65af240543cfc16b9b53c61/68747470733a2f2f696d672e736869656c64732e696f2f707970692f707976657273696f6e732f646a616e676f)](https://img.shields.io/pypi/pyversions/django/) [![version](https://camo.githubusercontent.com/f65085dd24765b7e88fa88ecea41ce71289f22b4abeb2b07ebfda219520bb022/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f646a616e676f2d332e312d627269676874677265656e2e737667)](https://docs.djangoproject.com/zh-hans/3.1/) [![docker_pulls](https://camo.githubusercontent.com/9c5cac1e8430d55c93ff98a9b6bc7994ae2384f29a6917b0048edde9a77bab8e/68747470733a2f2f696d672e736869656c64732e696f2f646f636b65722f70756c6c732f6868796f2f617263686572792e737667)](https://hub.docker.com/r/hhyo/archery/) [![HitCount](https://camo.githubusercontent.com/a64dd455244b30b7e6368c6e9a53ffc850ae2b520bf9197fd01d795b0169b843/687474703a2f2f686974732e6477796c2e696f2f6868796f2f6868796f2f417263686572792e737667)](http://hits.dwyl.io/hhyo/hhyo/Archery) [![License](https://camo.githubusercontent.com/2a2157c971b7ae1deb8eb095799440551c33dcf61ea3d965d86b496a5a65df55/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f4c6963656e73652d417061636865253230322e302d626c75652e737667)](http://github.com/hhyo/archery/blob/master/LICENSE) [![996.icu](https://camo.githubusercontent.com/88df9d1c7282f11e7579ce56f585ea096ddad3b44e6d3a90e59f6b2968118466/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6c696e6b2d3939362e6963752d7265642e737667)](https://996.icu/)

[文档](https://archerydms.com/) | [FAQ](https://github.com/hhyo/archery/wiki/FAQ) | [Releases](https://github.com/hhyo/archery/releases/)

## 系统体验

[在线体验](https://demo.archerydms.com/)

| 账号   | 密码   |
| ------ | ------ |
| archer | archer |

# 安装 Archery

## 安装 Docker

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

```
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

## 安装 docker-compose

Compose 是用于定义和运行多容器 Docker 应用程序的工具。通过 Compose，您可以使用 YML 文件来配置应用程序需要的所有服务。然后，使用一个命令，就可以从 YML 文件配置中创建并启动所有服务。

```bash
curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
docker-compose --version
```

## docker 部署 Archery

下载 [Releases](https://github.com/hhyo/archery/releases/)文件，解压后进入docker-compose文件夹

![](pic/archery01.png)

```bash
# 启动
docker-compose -f docker-compose.yml up -d

# 表结构初始化
docker exec -ti archery /bin/bash
cd /opt/archery
source /opt/venv4archery/bin/activate
python3 manage.py makemigrations sql  
python3 manage.py migrate

# 数据初始化
python3 manage.py dbshell<sql/fixtures/auth_group.sql
python3 manage.py dbshell<src/init_sql/mysql_slow_query_review.sql

# 创建管理用户
python3 manage.py createsuperuser

# 重启服务
# docker restart archery

# 日志查看和问题排查
# docker logs archery -f --tail=10
# logs/archery.log
```

## 登陆 Archery

http://127.0.0.1:9123/

![](pic/archery02.png)

