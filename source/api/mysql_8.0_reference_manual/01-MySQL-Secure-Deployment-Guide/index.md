---
title: MySQL 8.0 安全部署指南
---

**Abstract**

This is the MySQL 8.0 Secure Deployment Guide. It documents procedures for deploying a Linux-generic binary distribution of MySQL Enterprise Edition Server with features for implementing and managing the security of a MySQL installation. The deployment described in this guide is performed on Oracle Linux.

For help with using MySQL, please visit the [MySQL Forums](http://forums.mysql.com/), where you can discuss your issues with other MySQL users.

Document generated on: 2020-11-05 (revision: 67908)

**Table of Contents**

- [Preface and Legal Notices](https://dev.mysql.com/doc/mysql-secure-deployment-guide/8.0/en/preface.html)
- [1 Introduction](https://dev.mysql.com/doc/mysql-secure-deployment-guide/8.0/en/secure-deployment-overview.html)
- [2 Downloading the MySQL for Linux Generic Binary Package](https://dev.mysql.com/doc/mysql-secure-deployment-guide/8.0/en/secure-deployment-download.html)
- [3 Verifying Package Integrity](https://dev.mysql.com/doc/mysql-secure-deployment-guide/8.0/en/secure-deployment-verify-package.html)
- [4 Installing the MySQL Binary Package](https://dev.mysql.com/doc/mysql-secure-deployment-guide/8.0/en/secure-deployment-install.html)
- [5 Post Installation Setup](https://dev.mysql.com/doc/mysql-secure-deployment-guide/8.0/en/secure-deployment-post-install.html)
- [6 Installing the MySQL Password Validation Component](https://dev.mysql.com/doc/mysql-secure-deployment-guide/8.0/en/secure-deployment-password-validation.html)
- [7 Installing MySQL Enterprise Audit](https://dev.mysql.com/doc/mysql-secure-deployment-guide/8.0/en/secure-deployment-audit.html)
- [8 Installing MySQL Enterprise Firewall](https://dev.mysql.com/doc/mysql-secure-deployment-guide/8.0/en/secure-deployment-firewall.html)
- [9 Installing Connection Control Plugins](https://dev.mysql.com/doc/mysql-secure-deployment-guide/8.0/en/secure-deployment-connection-control.html)
- [10 Block Encryption Mode Configuration](https://dev.mysql.com/doc/mysql-secure-deployment-guide/8.0/en/secure-deployment-block-encryption-mode.html)
- [11 Enabling Authentication](https://dev.mysql.com/doc/mysql-secure-deployment-guide/8.0/en/secure-deployment-configure-authentication.html)
- [12 Configuring MySQL to Use Secure Connections](https://dev.mysql.com/doc/mysql-secure-deployment-guide/8.0/en/secure-deployment-secure-connections.html)
- [13 Creating User Accounts](https://dev.mysql.com/doc/mysql-secure-deployment-guide/8.0/en/secure-deployment-user-accounts.html)
- [14 Connecting to the Server](https://dev.mysql.com/doc/mysql-secure-deployment-guide/8.0/en/secure-deployment-connect.html)
- [A Transparent Data Encryption (TDE) and MySQL Keyring](https://dev.mysql.com/doc/mysql-secure-deployment-guide/8.0/en/secure-deployment-data-encryption.html)
- [B Data Masking and De-Identification](https://dev.mysql.com/doc/mysql-secure-deployment-guide/8.0/en/secure-deployment-data-masking.html)
- [C FIPS Support](https://dev.mysql.com/doc/mysql-secure-deployment-guide/8.0/en/secure-deployment-fips.html)
- [D SQL Roles and Dynamic Privileges](https://dev.mysql.com/doc/mysql-secure-deployment-guide/8.0/en/secure-deployment-roles-dynamic-privileges.html)
- [E Installation Directory and File Permissions](https://dev.mysql.com/doc/mysql-secure-deployment-guide/8.0/en/secure-deployment-permissions.html)
- [F Deployment Configuration File](https://dev.mysql.com/doc/mysql-secure-deployment-guide/8.0/en/secure-deployment-configuration-file.html)