---
title: systemd
---

# 简介

[redhat 7 systemd](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/chap-managing_services_with_systemd)

**Systemd**是Linux操作系统的系统和服务管理器。它被设计为与SysV初始化脚本向后兼容，并提供许多功能，例如在引导时并行启动系统服务，按需激活守护程序或基于依赖的服务控制逻辑。在Red Hat Enterprise Linux 7中，systemd将Upstart替换为默认的init系统。

Systemd引入了*系统单位*的概念。这些单元由位于[表10.2“系统单元文件位置”中](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/chap-Managing_Services_with_systemd#tabl-Managing_Services_with_systemd-Introduction-Units-Locations)列出的目录之一中的单元配置文件表示，并封装有关系统服务，侦听套接字以及与初始化系统相关的其他对象的信息。有关可用系统单位类型的完整列表，请[参见表10.1“可用的系统单位类型”](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/chap-Managing_Services_with_systemd#tabl-Managing_Services_with_systemd-Introduction-Units-Types)。

# Systemd管理服务

与SysV init或Upstart一起分发的Red Hat Enterprise Linux的早期版本使用位于目录中的*init脚本*`/etc/rc.d/init.d/`。这些初始化脚本通常使用Bash编写，并允许系统管理员控制其系统中服务和守护程序的状态。在Red Hat Enterprise Linux 7中，这些初始化脚本已被*服务单元*替换。

服务单元以`.service`文件扩展名结尾，其作用与初始化脚本相似。要视图，启动，停止，重新启动，启用或停用系统服务，使用`systemctl`如上述命令[表10.3，“与systemctl服务实用程序的比较”](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/chap-Managing_Services_with_systemd#tabl-Managing_Services_with_systemd-Services-systemctl)，[表10.4，“与systemctl的chkconfig的实用程序的比较”](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/chap-Managing_Services_with_systemd#tabl-Managing_Services_with_systemd-Services-chkconfig)，并且进一步在这个部分。该`service`和`chkconfig`命令仍然在体制和工作如预期可获得的，但只是为了兼容性的原因，应该避免。


# 配置文件详解

Unit files typically consist of three sections:

- [Unit] — contains generic options that are not dependent on the type of the unit. These options provide unit description, specify the unit’s behavior, and set dependencies to other units. For a list of most frequently used [Unit] options, see [Table 10.9, “Important [Unit\] Section Options”](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/chap-Managing_Services_with_systemd#tabl-Managing_Services_with_systemd-Unit_Sec_Options).
- [*unit type*] — if a unit has type-specific directives, these are grouped under a section named after the unit type. For example, service unit files contain the [Service] section, see [Table 10.10, “Important [Service\] Section Options”](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/chap-Managing_Services_with_systemd#tabl-Managing_Services_with_systemd-Service_Sec_Options) for most frequently used [Service] options.
- [Install] — contains information about unit installation used by `systemctl enable` and `disable` commands, see [Table 10.11, “Important [Install\] Section Options”](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/chap-Managing_Services_with_systemd#tabl-Managing_Services_with_systemd-Install_Sec_Options) for a list of [Install] options.



## Unit

| Unit            |                                |                                                              |
| --------------- | ------------------------------ | ------------------------------------------------------------ |
| After           | 表示服务需要在服务启动之后执行 | 无依赖                                                       |
| Before          | 表示服务需要在服务启动之前执行 | 无依赖                                                       |
| Wants           | 弱依赖关系                     |                                                              |
| Requires        | 强依赖关系                     | 停止之后本服务也必须停止                                     |
| Service         |                                |                                                              |
| EnvironmentFile | 环境参数文件                   | `EnvironmentFile=/etc/sysconfig/sshd以key=value`的形式保存以`$key`形式读取 |
| ExecStart       | 启动进程时执行的命令           |                                                              |
| ExecReload      | 重启服务时执行的命令           |                                                              |
| ExecStop        | 停止服务时执行的命令           |                                                              |
| ExecStartPre    | 启动服务之前执行的命令         |                                                              |
| ExecStartPost   | 启动服务之后执行的命令         |                                                              |
| ExecStopPost    | 停止服务之后执行的命令         |                                                              |

所有的启动设置之前，都可以加上一个连词号（-），表示"抑制错误"，即发生错误的时候，不影响其他命令的执行。比如，`EnvironmentFile=-/etc/sysconfig/sshd`（注意等号后面的那个连词号），就表示即使`/etc/sysconfig/sshd`文件不存在，也不会抛出错误。

## Type


| Type               |                                                              |
| ------------------ | ------------------------------------------------------------ |
| simple（默认值）： | ExecStart字段启动的进程为主进程                              |
| forking：          | ExecStart字段将以fork()方式启动，此时父进程将会退出，子进程将成为主进程 |
| oneshot：          | 类似于simple，但只执行一次，Systemd 会等它执行完，才启动其他服务 |
| dbus：             | 类似于simple，但会等待 D-Bus 信号后启动                      |
| notify：           | 类似于simple，启动结束后会发出通知信号，然后 Systemd 再启动其他服务 |
| idle：             | 类似于simple，但是要等到其他任务都执行完，才会启动该服务。一种使用场合是为让该服务的输出，不与其他服务的输出相混合 |

  

## KillMode  

| KillMode                  |                                                    |
| ------------------------- | -------------------------------------------------- |
| control-group（默认值）： | 当前控制组里面的所有子进程，都会被杀掉             |
| process：                 | 只杀主进程                                         |
| mixed：                   | 主进程将收到 SIGTERM 信号，子进程收到 SIGKILL 信号 |
| none：                    | 没有进程会被杀掉，只是执行服务的 stop 命令。       |

  

  ## Restart



| Restart        |                                                              |
| -------------- | ------------------------------------------------------------ |
| no（默认值）： | 退出后不会重启                                               |
| on-success：   | 只有正常退出时（退出状态码为0），才会重启                    |
| on-failure：   | 非正常退出时（退出状态码非0），包括被信号终止和超时，才会重启 |
| on-abnormal：  | 只有被信号终止和超时，才会重启                               |
| on-abort：     | 只有在收到没有捕捉到的信号终止时，才会重启                   |
| on-watchdog：  | 超时退出，才会重启                                           |
| always：       | 不管是什么退出原因，总是重启                                 |

 修改配置文件以后，需要重新加载配置文件，然后重新启动相关服务。

  ```bash
# 重新加载配置文件
$ systemctl daemon-reload
  ```

# 案例参考

> 通过systemd管理prometheus进程

1 添加systemctl的lib脚本

```bash
vim /usr/lib/systemd/system/prometheus.service
 
 
[Unit]
Description=Prometheus
Documentation=https://prometheus.io/docs/introduction/overview/
Wants=network-online.target
After=network-online.target
 
[Service]
User=root
Group=root
Type=simple
# 启动脚本
ExecStart=/usr/local/cloudcare/prometheus/bin/prometheus.sh
 
[Install]
WantedBy=multi-user.target

```

2 编写prometheus启动命令脚本

```bash
vim /usr/local/cloudcare/prometheus/bin/prometheus.sh
#!/bin/sh
cd /usr/local/cloudcare/prometheus/
/usr/local/cloudcare/prometheus/prometheus --web.enable-lifecycle --config.file=/usr/local/cloudcare/prometheus/prometheus.yml 1>>/usr/local/cloudcare/prometheus/log/start_prom.log 2>&1
 
 
chmod 755 /usr/local/cloudcare/prometheus/bin/prometheus.sh
```

3 验证启动

```bash
systemctl daemon-reload
# 配置开机加载
systemctl enable prometheus.service
# 启动
systemctl start prometheus
# 关闭
systemctl stop prometheus
```


