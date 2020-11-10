---
title: MySQL Utilities
---

MySQL Utilities is a package of utilities that are used for maintenance and administration of MySQL servers. These utilities encapsulate a set of primitive commands, and bundles them so they can be used to perform macro operations with a single command. They can be installed with MySQL Workbench or as a standalone package.

They are a set of command-line utilities and a Python library for making the common tasks easy to accomplish. The library is written entirely in Python, meaning that it is not necessary to have any other tools or libraries installed to make it work. It is currently designed to work with Python v2.6 or later and there is no support for Python v3.1.

The utilities are available under the GPLv2 license, and are extendable using the supplied library.

# Installing The MySQL Utilities

MySQL Utilities development is managed elsewhere, and requires a separate download. Attempting to start the MySQL Utilities when they are not installed will prompt for a download and their installation. See the MySQL Utilities manual for additional information.

Note

MySQL Workbench searches for the **mysqluc** MySQL Utility in the system `PATH` to determine if the MySQL Utilities are installed.

# Opening MySQL Utilities From MySQL Workbench

To open the MySQL Utility **mysqluc** (MySQL Utilities Unified Console) from MySQL Workbench, click **Tools** and then **Start Shell for MySQL Utilities** from the menu. The following output shows the MySQL Utilities console window with the `help` command executed.

```simple
Welcome to the MySQL Utilities Client (mysqluc) version 1.6.5
Copyright (c) 2010, 2017 Oracle and/or its affiliates. All rights reserved.
This is a release of dual licensed MySQL Utilities. For the avoidance of
doubt, this particular copy of the software is released
under a commercial license and the GNU General Public License does not apply.
MySQL Utilities is brought to you by Oracle.

Type 'help' for a list of commands or press TAB twice for list of utilities.

mysqluc> help
Command                   Description
----------------------    ---------------------------------------------------
help utilities            Display list of all utilities supported.
help <utility>            Display help for a specific utility.
show errors               Display errors captured during the execution of the
                          utilities.
clear errors              clear captured errors.
show last error           Display the last error captured during the
                          execution of the utilities
help | help commands      Show this list.
exit | quit               Exit the console.
set <variable>=<value>    Store a variable for recall in commands.
show options              Display list of options specified by the user on
                          launch.
show variables            Display list of variables.
<ENTER>                   Press ENTER to execute command.
<ESCAPE>                  Press ESCAPE to clear the command entry.
<DOWN>                    Press DOWN to retrieve the previous command.
<UP>                      Press UP to retrieve the next command in history.
<TAB>                     Press TAB for type completion of utility, option,
                          or variable names.
<TAB><TAB>                Press TAB twice for list of matching type
                          completion (context sensitive).
```
