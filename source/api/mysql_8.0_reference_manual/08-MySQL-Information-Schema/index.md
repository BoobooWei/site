---
title: MySQL 8.0 Information Schema
---

**Abstract**

This is the MySQL Information Schema extract from the MySQL 8.0 Reference Manual.

For legal information, see the [Legal Notices](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/preface.html#legalnotice).

For help with using MySQL, please visit the [MySQL Forums](http://forums.mysql.com/), where you can discuss your issues with other MySQL users.

Document generated on: 2020-11-09 (revision: 67967)

------

**Table of Contents**

- [Preface and Legal Notices](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/preface.html)
- [1 INFORMATION_SCHEMA Tables](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema.html)
- [2 The INFORMATION_SCHEMA ADMINISTRABLE_ROLE_AUTHORIZATIONS Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-administrable-role-authorizations-table.html)
- [3 The INFORMATION_SCHEMA APPLICABLE_ROLES Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-applicable-roles-table.html)
- [4 The INFORMATION_SCHEMA CHARACTER_SETS Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-character-sets-table.html)
- [5 The INFORMATION_SCHEMA CHECK_CONSTRAINTS Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-check-constraints-table.html)
- [6 The INFORMATION_SCHEMA COLLATIONS Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-collations-table.html)
- [7 The INFORMATION_SCHEMA COLLATION_CHARACTER_SET_APPLICABILITY Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-collation-character-set-applicability-table.html)
- [8 The INFORMATION_SCHEMA COLUMNS Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-columns-table.html)
- [9 The INFORMATION_SCHEMA COLUMN_PRIVILEGES Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-column-privileges-table.html)
- [10 The INFORMATION_SCHEMA COLUMN_STATISTICS Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-column-statistics-table.html)
- [11 The INFORMATION_SCHEMA ENABLED_ROLES Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-enabled-roles-table.html)
- [12 The INFORMATION_SCHEMA ENGINES Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-engines-table.html)
- [13 The INFORMATION_SCHEMA EVENTS Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-events-table.html)
- [14 The INFORMATION_SCHEMA FILES Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-files-table.html)
- [15 The INFORMATION_SCHEMA KEY_COLUMN_USAGE Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-key-column-usage-table.html)
- [16 The INFORMATION_SCHEMA KEYWORDS Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-keywords-table.html)
- [17 The INFORMATION_SCHEMA OPTIMIZER_TRACE Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-optimizer-trace-table.html)
- [18 The INFORMATION_SCHEMA PARAMETERS Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-parameters-table.html)
- [19 The INFORMATION_SCHEMA PARTITIONS Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-partitions-table.html)
- [20 The INFORMATION_SCHEMA PLUGINS Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-plugins-table.html)
- [21 The INFORMATION_SCHEMA PROCESSLIST Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-processlist-table.html)
- [22 The INFORMATION_SCHEMA PROFILING Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-profiling-table.html)
- [23 The INFORMATION_SCHEMA REFERENTIAL_CONSTRAINTS Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-referential-constraints-table.html)
- [24 The INFORMATION_SCHEMA RESOURCE_GROUPS Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-resource-groups-table.html)
- [25 The INFORMATION_SCHEMA ROLE_COLUMN_GRANTS Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-role-column-grants-table.html)
- [26 The INFORMATION_SCHEMA ROLE_ROUTINE_GRANTS Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-role-routine-grants-table.html)
- [27 The INFORMATION_SCHEMA ROLE_TABLE_GRANTS Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-role-table-grants-table.html)
- [28 The INFORMATION_SCHEMA ROUTINES Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-routines-table.html)
- [29 The INFORMATION_SCHEMA SCHEMATA Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-schemata-table.html)
- [30 The INFORMATION_SCHEMA SCHEMATA_EXTENSIONS Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-schemata-extensions-table.html)
- [31 The INFORMATION_SCHEMA SCHEMA_PRIVILEGES Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-schema-privileges-table.html)
- [32 The INFORMATION_SCHEMA STATISTICS Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-statistics-table.html)
- [33 The INFORMATION_SCHEMA ST_GEOMETRY_COLUMNS Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-st-geometry-columns-table.html)
- [34 The INFORMATION_SCHEMA ST_SPATIAL_REFERENCE_SYSTEMS Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-st-spatial-reference-systems-table.html)
- [35 The INFORMATION_SCHEMA ST_UNITS_OF_MEASURE Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-st-units-of-measure-table.html)
- [36 The INFORMATION_SCHEMA TABLES Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-tables-table.html)
- [37 The INFORMATION_SCHEMA TABLESPACES Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-tablespaces-table.html)
- [38 The INFORMATION_SCHEMA TABLE_CONSTRAINTS Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-table-constraints-table.html)
- [39 The INFORMATION_SCHEMA TABLE_PRIVILEGES Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-table-privileges-table.html)
- [40 The INFORMATION_SCHEMA TRIGGERS Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-triggers-table.html)
- [41 The INFORMATION_SCHEMA USER_ATTRIBUTES Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-user-attributes-table.html)
- [42 The INFORMATION_SCHEMA USER_PRIVILEGES Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-user-privileges-table.html)
- [43 The INFORMATION_SCHEMA VIEWS Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-views-table.html)
- [44 The INFORMATION_SCHEMA VIEW_ROUTINE_USAGE Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-view-routine-usage-table.html)
- [45 The INFORMATION_SCHEMA VIEW_TABLE_USAGE Table](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/information-schema-view-table-usage-table.html)
- [46 Extensions to SHOW Statements](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/extended-show.html)
- [47 MySQL 8.0 FAQ: INFORMATION_SCHEMA](https://dev.mysql.com/doc/mysql-infoschema-excerpt/8.0/en/faqs-information-schema.html)