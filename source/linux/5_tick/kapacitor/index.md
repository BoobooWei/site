---
title: Kapacitor 学习笔记
---

> Kapacitor 学习笔记

# Kapacitor

* [Kapacitor简介](/linux/5_tick/kapacitor/kapacitor-introduce.html)
* [TICKscripts开发规范](/linux/5_tick/kapacitor/TICKscriptsDevelopmentSpecification.html)

## 01_Kapacitor安装

* [CentOS7安装TICK技术栈](/linux/5_tick/kapacitor/01_Kapacitor安装/CentOS7安装TICK技术栈.html)

## 02_TICKscript语言

* [A_TICKscript基础语法和运算符](/linux/5_tick/kapacitor/02_TICKscript语言/A_TICKscript基础语法和运算符.html)
* [B_TICKscript数据类型](/linux/5_tick/kapacitor/02_TICKscript语言/B_TICKscript数据类型.html)
* [C_TICKscript变量声明](/linux/5_tick/kapacitor/02_TICKscript语言/C_TICKscript变量声明.html)
* [D_TICKscript数据库声明](/linux/5_tick/kapacitor/02_TICKscript语言/D_TICKscript数据库声明.html)
* [E_TICKscript变量标签和字段](/linux/5_tick/kapacitor/02_TICKscript语言/E_TICKscript变量标签和字段.html)
* [F_TICKscript节点属性方法](/linux/5_tick/kapacitor/02_TICKscript语言/F_TICKscript节点属性方法.html)
* [G_TICKscript节点链接方法](/linux/5_tick/kapacitor/02_TICKscript语言/G_TICKscript节点链接方法.html)
* [H_TICKscriptInfluxQL](/linux/5_tick/kapacitor/02_TICKscript语言/H_TICKscriptInfluxQL.html)
* [I_TICKscriptLambda表达式](/linux/5_tick/kapacitor/02_TICKscript语言/I_TICKscriptLambda表达式.html)


## 03_Kapacitor配置文件

* [A_Kapacitor配置文件简介](/linux/5_tick/kapacitor/03_Kapacitor配置文件/A_Kapacitor配置文件简介.html)

## 04_Kapacitor命令行

* [A_基础使用](/linux/5_tick/kapacitor/04_Kapacitor命令行/A_基础使用.html)


## 05_Kapacitor监控案例

* [01_从一个最基本的cpu.alert开始](/linux/5_tick/kapacitor/05_Kapacitor监控案例/01_从一个最基本的cpu.alert开始.html)
* [02_降采样](/linux/5_tick/kapacitor/05_Kapacitor监控案例/02_降采样.html)
* [03_创建任务模板脚本](/linux/5_tick/kapacitor/05_Kapacitor监控案例/03_创建任务模板脚本.html)



```bash
ll *.md | awk '{print "* ["$9"](/linux/5_tick/kapacitor/02_TICKscript语言/"$9")"}' | sed 's/.md//'|sed 's/.md/.html/g'
ll *.md | awk '{print "* ["$9"](/linux/5_tick/kapacitor/03_Kapacitor配置文件/"$9")"}' | sed 's/.md//'|sed 's/.md/.html/g'
ll *.md | awk '{print "* ["$9"](/linux/5_tick/kapacitor/04_Kapacitor命令行/"$9")"}' | sed 's/.md//'|sed 's/.md/.html/g'
ll *.md | awk '{print "* ["$9"](/linux/5_tick/kapacitor/05_Kapacitor监控案例/"$9")"}' | sed 's/.md//'|sed 's/.md/.html/g'
```

