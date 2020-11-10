---
title: MySQL 8.0 Shell
---

MySQL Shell is an advanced client and code editor for MySQL Server. In addition to the provided SQL functionality, similar to [**mysql**](https://dev.mysql.com/doc/refman/8.0/en/mysql.html), MySQL Shell provides scripting capabilities for JavaScript and Python and includes APIs for working with MySQL. MySQL Shell is a component that you can install separately.

The following discussion briefly describes MySQL Shell's capabilities. For more information, see the MySQL Shell manual, available at <https://dev.mysql.com/doc/mysql-shell/en/>.

MySQL Shell includes the following APIs implemented in JavaScript and Python which you can use to develop code that interacts with MySQL.

- The X DevAPI enables developers to work with both relational and document data when MySQL Shell is connected to a MySQL server using the X Protocol. This enables you to use MySQL as a Document Store, sometimes referred to as "using NoSQL". For more information, see [Chapter 20, _Using MySQL as a Document Store_](https://dev.mysql.com/doc/refman/8.0/en/document-store.html). For documentation on the concepts and usage of X DevAPI, which is implemented in MySQL Shell, see [X DevAPI User Guide](https://dev.mysql.com/doc/x-devapi-userguide/en/).
- The AdminAPI enables database administrators to work with InnoDB Cluster, which provides an integrated solution for high availability and scalability using InnoDB based MySQL databases, without requiring advanced MySQL expertise. The AdminAPI also includes support for InnoDB ReplicaSet, which enables you to administer a set of MySQL instances running asynchronous GTID-based replication in a similar way to InnoDB Cluster. Additionally, the AdminAPI makes administration of MySQL Router easier, including integration with both InnoDB Cluster and InnoDB ReplicaSet. See [Chapter 21, _Using MySQL AdminAPI_](https://dev.mysql.com/doc/refman/8.0/en/admin-api-userguide.html).

MySQL Shell is available in two editions, the Community Edition and the Commercial Edition. The Community Edition is available free of charge. The Commercial Edition provides additional Enterprise features at low cost.

# Using MySQL as a Document Store

**Table of Contents**

- [20.1 Interfaces to a MySQL Document Store](https://dev.mysql.com/doc/refman/8.0/en/document-store-interfaces.html)
- [20.2 Document Store Concepts](https://dev.mysql.com/doc/refman/8.0/en/document-store-concepts.html)
- [20.3 JavaScript Quick-Start Guide: MySQL Shell for Document Store](https://dev.mysql.com/doc/refman/8.0/en/mysql-shell-tutorial-javascript.html)
- [20.4 Python Quick-Start Guide: MySQL Shell for Document Store](https://dev.mysql.com/doc/refman/8.0/en/mysql-shell-tutorial-python.html)
- [20.5 X Plugin](https://dev.mysql.com/doc/refman/8.0/en/x-plugin.html)

This chapter introduces an alternative way of working with MySQL as a document store, sometimes referred to as "using NoSQL". If your intention is to use MySQL in a traditional (SQL) way, this chapter is probably not relevant to you.

Traditionally, relational databases such as MySQL have usually required a schema to be defined before documents can be stored. The features described in this section enable you to use MySQL as a document store, which is a schema-less, and therefore schema-flexible, storage system for documents. For example, when you create documents describing products, you do not need to know and define all possible attributes of any products before storing and operating with the documents. This differs from working with a relational database and storing products in a table, when all columns of the table must be known and defined before adding any products to the database. The features described in this chapter enable you to choose how you configure MySQL, using only the document store model, or combining the flexibility of the document store model with the power of the relational model.

To use MySQL as a document store, you use the following server features:

- X Plugin enables MySQL Server to communicate with clients using X Protocol, which is a prerequisite for using MySQL as a document store. X Plugin is enabled by default in MySQL Server as of MySQL 8.0\. For instructions to verify X Plugin installation and to configure and monitor X Plugin, see [Section 20.5, "X Plugin"](https://dev.mysql.com/doc/refman/8.0/en/x-plugin.html).
- X Protocol supports both CRUD and SQL operations, authentication via SASL, allows streaming (pipelining) of commands and is extensible on the protocol and the message layer. Clients compatible with X Protocol include MySQL Shell and MySQL 8.0 Connectors.
- Clients that communicate with a MySQL Server using X Protocol can use X DevAPI to develop applications. X DevAPI offers a modern programming interface with a simple yet powerful design which provides support for established industry standard concepts. This chapter explains how to get started using either the JavaScript or Python implementation of X DevAPI in MySQL Shell as a client. See [X DevAPI User Guide](https://dev.mysql.com/doc/x-devapi-userguide/en/) for in-depth tutorials on using X DevAPI.

# Using MySQL AdminAPI

**Table of Contents**

- [21.1 MySQL AdminAPI](https://dev.mysql.com/doc/refman/8.0/en/admin-api-overview.html)
- [21.2 MySQL InnoDB Cluster](https://dev.mysql.com/doc/refman/8.0/en/mysql-innodb-cluster.html)
- [21.3 MySQL InnoDB ReplicaSet](https://dev.mysql.com/doc/refman/8.0/en/mysql-innodb-replicaset.html)
- [21.4 MySQL Router](https://dev.mysql.com/doc/refman/8.0/en/admin-api-integrating-router.html)
- [21.5 AdminAPI MySQL Sandboxes](https://dev.mysql.com/doc/refman/8.0/en/admin-api-sandboxes.html)

This chapter covers MySQL AdminAPI, provided with MySQL Shell, which enables you to administer MySQL instances, using them to create InnoDB Clusters, InnoDB ReplicaSets, and integrating MySQL Router.
