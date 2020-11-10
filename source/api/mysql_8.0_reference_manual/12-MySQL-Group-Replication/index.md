---
title: MySQL 8.0 组复制
---

**Table of Contents**

- [18.1 Group Replication Background](https://dev.mysql.com/doc/refman/8.0/en/group-replication-background.html)
- [18.2 Getting Started](https://dev.mysql.com/doc/refman/8.0/en/group-replication-getting-started.html)
- [18.3 Monitoring Group Replication](https://dev.mysql.com/doc/refman/8.0/en/group-replication-monitoring.html)
- [18.4 Group Replication Operations](https://dev.mysql.com/doc/refman/8.0/en/group-replication-operations.html)
- [18.5 Group Replication Security](https://dev.mysql.com/doc/refman/8.0/en/group-replication-security.html)
- [18.6 Group Replication Performance](https://dev.mysql.com/doc/refman/8.0/en/group-replication-performance.html)
- [18.7 Upgrading Group Replication](https://dev.mysql.com/doc/refman/8.0/en/group-replication-upgrade.html)
- [18.8 Group Replication System Variables](https://dev.mysql.com/doc/refman/8.0/en/group-replication-options.html)
- [18.9 Requirements and Limitations](https://dev.mysql.com/doc/refman/8.0/en/group-replication-requirements-and-limitations.html)
- [18.10 Frequently Asked Questions](https://dev.mysql.com/doc/refman/8.0/en/group-replication-frequently-asked-questions.html)



This chapter explains MySQL Group Replication and how to install, configure and monitor groups. MySQL Group Replication enables you to create elastic, highly-available, fault-tolerant replication topologies.

Groups can operate in a single-primary mode with automatic primary election, where only one server accepts updates at a time. Alternatively, groups can be deployed in multi-primary mode, where all servers can accept updates, even if they are issued concurrently.

There is a built-in group membership service that keeps the view of the group consistent and available for all servers at any given point in time. Servers can leave and join the group and the view is updated accordingly. Sometimes servers can leave the group unexpectedly, in which case the failure detection mechanism detects this and notifies the group that the view has changed. This is all automatic.

Group Replication guarantees that the database service is continuously available. However, it is important to understand that if one of the group members becomes unavailable, the clients connected to that group member must be redirected, or failed over, to a different server in the group, using a connector, load balancer, router, or some form of middleware. Group Replication does not have an inbuilt method to do this. For example, see [MySQL Router 8.0](https://dev.mysql.com/doc/mysql-router/8.0/en/).

Group Replication is provided as a plugin to MySQL Server. You can follow the instructions in this chapter to configure the plugin on each of the server instances that you want in the group, start up the group, and monitor and administer the group. An alternative way to deploy a group of MySQL server instances is by using InnoDB Cluster, which uses Group Replication and wraps it in a programmatic environment that enables you to easily work with the server instances in [MySQL Shell 8.0 (part of MySQL 8.0)](https://dev.mysql.com/doc/mysql-shell/8.0/en/). In addition, InnoDB Cluster interfaces seamlessly with MySQL Router and simplifies deploying MySQL with high availability. For instructions to deploy a group using InnoDB Cluster, see [Chapter 21, *Using MySQL AdminAPI*](https://dev.mysql.com/doc/refman/8.0/en/admin-api-userguide.html).

The chapter is structured as follows:

- [Section 18.1, “Group Replication Background”](https://dev.mysql.com/doc/refman/8.0/en/group-replication-background.html) provides an introduction to groups and how Group Replication works.
- [Section 18.2, “Getting Started”](https://dev.mysql.com/doc/refman/8.0/en/group-replication-getting-started.html) explains how to configure multiple MySQL Server instances to create a group.
- [Section 18.3, “Monitoring Group Replication”](https://dev.mysql.com/doc/refman/8.0/en/group-replication-monitoring.html) explains how to monitor a group.
- [Section 18.4, “Group Replication Operations”](https://dev.mysql.com/doc/refman/8.0/en/group-replication-operations.html) explains how to work with a group.
- [Section 18.5, “Group Replication Security”](https://dev.mysql.com/doc/refman/8.0/en/group-replication-security.html) explains how to secure a group.
- [Section 18.6, “Group Replication Performance”](https://dev.mysql.com/doc/refman/8.0/en/group-replication-performance.html) explains how to fine tune performance for a group.
- [Section 18.7, “Upgrading Group Replication”](https://dev.mysql.com/doc/refman/8.0/en/group-replication-upgrade.html) explains how to upgrade a group.
- [Section 18.8, “Group Replication System Variables”](https://dev.mysql.com/doc/refman/8.0/en/group-replication-options.html) is a reference for the system variables specific to Group Replication.
- [Section 18.9, “Requirements and Limitations”](https://dev.mysql.com/doc/refman/8.0/en/group-replication-requirements-and-limitations.html) explains technical requirements and limitations for Group Replication.
- [Section 18.10, “Frequently Asked Questions”](https://dev.mysql.com/doc/refman/8.0/en/group-replication-frequently-asked-questions.html) provides answers to some technical questions about deploying and operating Group Replication.
