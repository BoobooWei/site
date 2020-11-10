---
title: 使用 Doxygen 生成 MySQL 文档
---

# 关于 Doxygen

Doxygen是事实上的标准工具，可从带注释的C ++来源生成文档，但它也支持其他流行的编程语言，例如C，Objective-C，C＃，PHP，Java，Python，IDL（Corba，Microsoft和UNO / OpenOffice等） ），Fortran，VHDL和某种程度上是D。

Doxygen可通过三种方式为您提供帮助：

1. 它可以![$ \ mbox {\ LaTeX} $](https://www.doxygen.nl/form_0.png)从一组已记录的源文件中生成一个在线文档浏览器（HTML）和/或一个离线参考手册（in ）。还支持在RTF（MS-Word），PostScript，超链接的PDF，压缩的HTML和Unix手册页中生成输出。直接从源代码中提取文档，这使保持文档与源代码的一致性变得容易得多。
2. 您可以[配置](https://www.doxygen.nl/manual/starting.html#extract_all)doxygen从未记录的源文件中提取代码结构。这对于在大型源代码发行版中快速找到您的方式非常有用。Doxygen还可以通过包括依赖关系图，继承图和协作图来可视化各个元素之间的关系，这些依赖图，继承图和协作图都是自动生成的。
3. 您也可以使用doxygen来创建常规文档（就像我在doxygen用户手册和网站上所做的那样）。

Doxygen是在Mac OS X和Linux下开发的，但设置为高度可移植的。结果，它也可以在大多数其他Unix版本上运行。此外，还提供Windows可执行文件。

## Doxygen许可证

[Dimitri van Heesch](mailto:doxygen.com) 版权所有©1997-2018 。

特此授予根据GNU通用公共许可的条款使用，复制，修改和分发此软件及其文档的许可。对于本软件是否适合任何目的，不做任何陈述。它按“原样”提供，没有明示或暗示的保证。有关 更多详细信息，请参见 [GNU通用公共许可证](http://www.gnu.org/licenses/old-licenses/gpl-2.0.html)。

doxygen产生的文件是衍生自其生产中使用的输入的衍生作品；他们不受此许可证的影响。

## MySQL Doxygen

MySQL源代码包含使用 [Doxygen](https://www.doxygen.nl/index.html) 编写的内部文档。生成的Doxygen内容可从 [https://dev.mysql.com/doc/index-other.html](https://dev.mysql.com/doc/index-other.html) 获得。

# 安装 Doxygen

安装**doxygen** 1.8.11或更高版本。可以从http://www.doxygen.nl/获得发行版本 。

> MacOS 10.14.3 

**[Doxygen-1.8.20.dmg](http://doxygen.nl/files/Doxygen-1.8.20.dmg)**（93.8MB）
这是一个独立的磁盘映像，其中包含GUI前端。这些二进制文件仅支持64位Intel CPU。

[图形化界面](https://www.doxygen.nl/manual/doxywizard_usage.html)



