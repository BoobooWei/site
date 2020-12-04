---
title: Docker 学习笔记
---

> Docker 学习

# 内容

## 安装

* [安装 Mac 版 Docker 桌面](/linux/4_docker/a01_mac_install_docker.html)
* [安装 Linux 版 Docker-ce](/linux/4_docker/a02_linux_install_docker.html)

## 原理

* [理解 Docker 的知识体系](/linux/4_docker/b01_docker_basic.html)

## 应用

* [Docker 部署 Zipkin](/linux/4_docker/u01_docker_zipkin.html)
* [Docker 部署 MySQL 8.0.22](/linux/4_docker/u02_docker_mysql.html)
* [Docker 部署 2048游戏网站](/linux/4_docker/u03_docker_2048.html)

```bash
ll *.md | awk '{print "(/linux/4_docker/"$9")"}' | sed 's/.md//'|sed 's/.md/.html/g'
```
