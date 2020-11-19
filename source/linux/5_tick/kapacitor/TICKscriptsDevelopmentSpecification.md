---
title: TICKscripts开发规范
categories:
  - - 培训教程
    - TICK技术栈之kapacitor
tags:
  - 监控告警
  - TICK技术栈
date: 2020-04-14 11:28:41
---

<!-- MDTOC maxdepth:6 firsth1:1 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

- [TICKscripts开发规范](#tickscripts开发规范)
	- [开发环境](#开发环境)
		- [Chronograf Web UI](#chronograf-web-ui)
		- [Vim集成](#vim集成)
		- [Emacs主要模式](#emacs主要模式)
		- [Atom集成](#atom集成)
		- [JetBrains 插件](#jetbrains-插件)
		- [Visual Studio代码插件](#visual-studio代码插件)
	- [推荐的开发工作流程](#推荐的开发工作流程)
	- [源控制是必须的](#源控制是必须的)
	- [存储库结构](#存储库结构)
	- [测试和部署更改](#测试和部署更改)
	- [结论](#结论)

<!-- /MDTOC -->


# TICKscripts开发规范

> [The State of TICKscript Development](https://dzone.com/articles/the-state-of-tickscript-development)
>
> 了解使用TICK堆栈进行开发的工具以及如何编写TICKscripts以充分利用Kapacitor的监控和警报。

为了充分利用[Kapacitor](https://www.influxdata.com/time-series-platform/kapacitor/)（[TICK Stack](https://www.influxdata.com/time-series-platform/)的'K' ）及其提供的监控和警报，您和您的团队将需要编写TICKscripts。[TICKscript](https://docs.influxdata.com/kapacitor/latest/tick/)是一种简单而强大的方法，可以对流经InfluxDB的数据进行聚合，分析和警报。在这篇文章中，我将介绍可用于编写TICKscripts的各种工具，并重点介绍我在组织脚本开发过程时看到的一些常见模式。让我们首先重点介绍一些可用于编写TICKscripts的开发工具。各种编辑器和IDE的插件由社区提供，不由InfluxData维护。如果您有一个可能缺少TICKscript编辑器的收藏编辑器，我们建议您创建一个并在[社区页面](https://community.influxdata.com/)上共享它。

## 开发环境

每个开发人员都有一个他们用来编写和部署代码的最喜欢的IDE。下面是我们了解开发人员用于编写TICKscripts的各种工具的快速概述。

### Chronograf Web UI

启动和运行编写TICKscripts的最快方法之一是通过Chronograf UI中的内置编辑器完成。要开始使用，请确保您可以访问在某处运行的完整TICK堆栈。如果您想在机器上试用，请查看[本教程，了解如何设置](https://docs.influxdata.com/kapacitor/latest/working/kapa-and-chrono/)和连接[所有内容](https://docs.influxdata.com/kapacitor/latest/working/kapa-and-chrono/)。可以在创建子菜单的“警报”选项卡下找到TICKscript编辑器。

![img](pic/043.png)

从那里，您有两个选择：您可以使用UI中的向导创建警报规则，也可以创建和编辑自定义TICKscript以执行您喜欢的任何操作。在幕后，警报向导将生成一个TICKscript，然后您可以进入并稍后手动编辑。这可以作为您可以用于进一步自定义的起点。要记住的一件事是Chronograf UI只会与您选择的链接Kapacitor实例进行交互。这是它将提取脚本列表以及显示的实际代码的位置。如果您在Kapacitor机器上本地存储了文件，则在通过此UI进行更改时，需要手动同步这些文件。此外，在Chronograf的1.4版本中，我们推出了一款新的Kapacitor日志查看器，可以让您查看任何输出`log()`命令就在你的浏览器中。这对调试脚本非常有用。

![img](pic/044.png)

### Vim集成

对于那些不喜欢离开终端的人来说，还有一个由InfluxData自己的[Nathaniel Cook](https://github.com/nathanielc)维护的Vim插件。它可以在[vim-tickscript GitHub repo中找到](https://github.com/nathanielc/vim-tickscript)。使用您最喜欢的Vim插件管理器快速安装，但是README文件中提供[了Pathogen和vim-plug的说明](https://github.com/nathanielc/vim-tickscript#install)。安装后，您可以运行`:TickInstallBinaries`将添加`tickfmt`到路径以格式化脚本的命令。当我安装它时，由于我不使用Vim进行开发，我还需要安装[vim-go插件](https://github.com/fatih/vim-go/)，否则当我尝试运行脚本安装时我看到了一堆错误`tickfmt`。默认情况下，此插件会在您每次保存时自动格式化脚本，但可以在首选项中更改。

![img](pic/045.gif)

### Emacs主要模式

当然，如果我们谈论Vim，我们需要给予Emacs相同的时间。[Marc Sherry ](https://github.com/msherry)[为TICKscript文件制作](https://github.com/msherry/tickscript-mode)了一个非常棒的[Emacs主要模式，](https://github.com/msherry/tickscript-mode)并与社区分享。除了语法高亮和格式化之外，它还允许您通过Emacs命令运行常见的Kapacitor功能。您可以在[README文件中](https://github.com/msherry/tickscript-mode#usage)找到命令列表。


![img](pic/046.png)

### Atom集成

Atom是一个非常灵活的开源文本编辑器，许多开发人员喜欢使用它。[Bubba Hines](https://github.com/bubba-h57)为[TICKscript语法高亮显示](https://atom.io/packages/language-tick)了一个很好的Atom包。通过Atom软件包安装程序快速简便地进行安装，只需搜索即可完成安装`language-tick`。它不会自动格式化您的脚本，但语法突出显示有助于快速查找问题。如果您在一个屏幕上查找所有内容，则可以安装该[`platformio-atom-ide-terminal`插件](https://atom.io/packages/platformio-ide-terminal)并将其用于在Kapacitor中测试和安装脚本。

![img](pic/047.png)

### JetBrains 插件

JetBrains IntelliJ平台还支持Tikscript编辑，由[Vladislav Rassokhin](https://github.com/VladRassokhin)和他的[intellij-kapacitor插件提供](https://github.com/VladRassokhin/intellij-kapacitor)。这可以在任何基于IntelliJ的IDE上使用，包括[GoLand](https://www.jetbrains.com/go/)或[IDEA](https://www.jetbrains.com/idea/)。与Atom插件一样，它具有语法高亮功能，内置的终端面板可以在同一个窗口中轻松更新Kapacitor中的脚本。您还可以使用命令单击快速跳转到变量定义，就像编写Java或Go代码一样。

![img](pic/048.png)

### Visual Studio代码插件

最后但并非最不重要的是，对于那些爱上[微软Visual Code](https://code.visualstudio.com/download)工具的人来说，[Matt Jones](https://gitlab.com/mattyjones)也[为此](https://marketplace.visualstudio.com/items?itemName=mattyjones.vscode-tickscript)编写了一个[TICKscript语法荧光笔插件](https://marketplace.visualstudio.com/items?itemName=mattyjones.vscode-tickscript)。安装Visual Code后，通过浏览器中的安装按钮可以轻松安装。与其他IDE一样，Visual Code附带一个集成的终端面板，可用于向Kapacitor发送命令。


![img](pic/049.png)

## 推荐的开发工作流程

正如您所看到的，可用于编写TICKscripts的工具并不缺乏。但是，拥有合适的工具只是解决方案的一部分。您需要标准化将TICKscripts从本地开发人员计算机移动到生产系统的过程。

![img](pic/050.png)

让我们从部署环境开始。开发TICKscripts应该像组织中的任何其他类型的代码一样处理，并且不应该直接在生产中编辑脚本。我们的大多数大型用户至少有三个环境正在运行：本地开发环境，集成环境和生产环境。您可能拥有更多，但不管怎样，您将大大增加推出无法正常运行或监控的脚本的机会。所有新脚本或更改都是从左到右流动，而不是相反，无论多小。

## 源控制是必须的

我们的许多客户都将他们的TICKscripts检查到某种源控制系统，通常是Git或SVN。我们建议围绕负责监控的不同团队组织存储库。单个repo应该包含将由单个团队在其Kapacitor实例或集群上部署的所有TICKscripts。这不仅使部署脚本变得更容易（存储库根目录下的一个shell脚本可以快速扫描仓库中的所有TICKscripts并通过Kapacitor API加载它们），但它也使得查找，添加更容易并修复它们，因为每个团队都管理自己的。

我们看到的另一种常见模式是，当有一个集中式基础架构团队跟踪部署到公司中每个Kapacitor实例的核心TICKscripts时。然后，每个团队也可以拥有自己的存储库和自己的脚本集。这里的想法是确保团队不必请求彼此的许可进行更改，因为每个团队都有自己的存储库，可以使用自己的TICKscripts进行管理。

## 存储库结构

有许多不同的方法来构建包含TICKscripts的存储库，包括将根目录中的所有脚本整齐地分类到文件夹中。如果您计划在Kapacitor中利用[基于文件的脚本加载](https://github.com/influxdata/kapacitor/tree/master/examples/load)，则需要维护预定义的结构。无论它们是如何组织的，当它们被发送到Kapacitor时，每个都只是通过它们的名称引用，因此我们建议将有用的关键字编码到文件和脚本的实际名称中。例如，如果数据组具有监视Kafka群集上的高CPU的警报，则脚本文件名可能是`data-team-kafka-high-cpu.tick`使用相应的脚本名称。当然，您可以自由选择您喜欢的任何格式，但我们的建议是在每个Kapacitor实例上保持一致，并尽可能多地将可搜索的信息编码到其中。还要记住，结果是从Kapacitor API按字母顺序返回的。

## 测试和部署更改

对TICKscripts进行更改的过程也应遵循标准开发最佳实践。开发人员应首先从源代码控制中检出存储库，进行更改（可能在不同的分支上），并在将其更新回主分支之前让其他团队成员审查更改代码。您也可以编写[单元测试您的TICKscripts](https://github.com/gpestana/kapacitor-unit)使用此真棒库[贡萨洛佩斯塔纳](https://github.com/gpestana)。使用此单元测试框架，您可以定义应触发警报的示例数据，并验证它是否按预期工作。这些单元测试应与TICKscripts位于同一个存储库中。

将代码合并回主分支后，应将其部署到测试环境，以便使用自动脚本进行集成测试。您的集成环境应尽可能地模仿您的生产环境，以便可以在此处发现与其他脚本或警报的任何问题或冲突，而不是生产。您应该能够使用样本数据生成模拟您在集成环境中尝试提醒的问题。

最后，一旦您的TICKscript被验证在您的集成环境中工作，它应该使用部署脚本自动加载到您的生产环境中。

所有这些听起来可能听起来像开始时需要做很多工作，但是前面的一点投资会多次回报，提高灵活性并为您的生产系统提供适当的警报。

## 结论

对于那些刚刚开始使用TICKscripts的人，我希望我能够在工具和开发过程方面为您指明正确的方向。如果您已经部署了大量TICKscripts，请在评论中告诉我们您是否有一些有用的提示和技巧来开发和维护脚本。

* 编辑器：atom 安装插件 language-tick
* 测试库：[kapacitor-unit](https://github.com/gpestana/kapacitor-unit)
* 开发过程：本地开发环境，集成环境和生产环境
* 存储结构： [基于文件的脚本加载](https://github.com/influxdata/kapacitor/tree/master/examples/load)
