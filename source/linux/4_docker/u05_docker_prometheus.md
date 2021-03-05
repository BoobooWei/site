---
title: Docker 部署 Prometheus+Grafana+node-exporter
---

# 架构图

## Prometheus的生态

![img](https://cdn.rawgit.com/prometheus/prometheus/e761f0d/documentation/images/architecture.svg)

Prometheus的生态中，Exporter扮演了重要的角色。对于“知名”应用程序，服务器或数据库，Prometheus官方提供了足够多的Exporters。这也是Prometheus监视目标的主要方式。

当需要监控的中间件或是数据库类型比较少的时候，并没有什么问题。

目前我们待监控的 AWS RDS 集群数量和实例数量非常大， 那可能需要维护启动非常多的 mysqld-exporter，将来数量甚至是到了上千的时候，需要一部分精力用于维护和升级exporter，管理他们的部署。


# 环境准备

| 操作系统        | docker版本                 | ip                                                           | 容器                             |
| --------------- | -------------------------- | ------------------------------------------------------------ | -------------------------------- |
| Red Hat 7.3.1-5 | 19.03.13-ce, build 4484c46 | 10.0.0.1 | Prometheus+Grafana+node-exporter |

## Install Docker

```
sudo su - root
# 安装docker
yum install -y docker
systemctl start docker
systemctl enable docker
```

## Running Prometheus on Docker

```
# 编辑配置文件prometheus.yml
mkdir rds_monitor
cd rds_monitor
cat > prometheus.yml << ENDF
global:
  scrape_interval:     15s # By default, scrape targets every 15 seconds.
 
  # Attach these labels to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
    monitor: 'rds-monitor'
 
# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'prometheus'
 
    # Override the global default and scrape targets from this job every 5 seconds.
    scrape_interval: 5s
 
    static_configs:
      - targets: ['localhost:9090']
ENDF
# 获取 prometheus image
docker pull prom/prometheus
# 启动 prometheus
docker run  -d \
  -p 9090:9090 \
  -v /root/rds_monitor/prometheus.yml:/etc/prometheus/prometheus.yml  \
  --restart=always \
  --name prometheus \
  prom/prometheus
# 查看
docker ps -a
```

## Running Grafana on Docker

```
# 安装 grafana
mkdir /root/rds_monitor/grafana-storage
chmod 777 -R /root/rds_monitor/grafana-storage
docker pull grafana/grafana
docker run -d \
  -p 3000:3000 \
  --name=grafana \
  -v /root/rds_monitor/grafana-storage:/var/lib/grafana \
  --restart=always \
  --name grafana \
  grafana/grafana
```

## Running mysqld-exporter on Docker

```
docker pull prom/mysqld-exporter
docker run -d -p 9104:9104
        -e DATA_SOURCE_NAME="user:password@(ip:port)/database" prom/mysqld-exporter
```


# 指标采集配置

## 问题1：Required Grants

MySQL 采集器需要一个只读用户来连接数据库，权限如下：

```
CREATE USER 'exporter'@'localhost' IDENTIFIED BY 'XXXXXXXX';
GRANT PROCESS, REPLICATION CLIENT ON *.* TO 'exporter'@'localhost';
GRANT SELECT ON performance_schema.* TO 'exporter'@'localhost';
```



## 问题2：密码明文问题

采集器是通过配置文件的方式记录数据库连接方式，这时候密码明文问题如何解决呢？

解决方法：使用 [AWS Secrets Manager](https://aws.amazon.com/cn/secrets-manager/)

# 帮助

- [Running Prometheus on Docker](https://prometheus.io/docs/prometheus/latest/installation/)
- [EXPORTERS AND INTEGRATIONS](https://prometheus.io/docs/instrumenting/exporters/)
- [mysqld-exporter](https://hub.docker.com/r/prom/mysqld-exporter)

