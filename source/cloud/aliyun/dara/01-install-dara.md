---
title: 安装 Darabonba 
---

# Darabonba

[Darabonba(原名 TeaDSL)](https://github.com/aliyun/darabonba/tree/master/doc)

[![NPM version](https://camo.githubusercontent.com/10e3f53d78004bb992b56a05e3a35f06b7027b81/68747470733a2f2f696d672e736869656c64732e696f2f6e706d2f762f4064617261626f6e62612f7061727365722e7376673f7374796c653d666c61742d737175617265)](https://npmjs.org/package/@darabonba/parser) [![build status](https://camo.githubusercontent.com/cdcf1dd16801622b406300346ec2867b06b158dd/68747470733a2f2f696d672e736869656c64732e696f2f7472617669732f616c6979756e2f64617261626f6e62612e7376673f7374796c653d666c61742d737175617265)](https://travis-ci.org/aliyun/darabonba) [![codecov](https://camo.githubusercontent.com/127035df85c0c9e90ac54a437589b778080671d8/68747470733a2f2f636f6465636f762e696f2f67682f616c6979756e2f64617261626f6e62612f6272616e63682f6d61737465722f67726170682f62616467652e737667)](https://codecov.io/gh/aliyun/darabonba) [![David deps](https://camo.githubusercontent.com/b4d731df547b640a47a14f6486302536466baefb/68747470733a2f2f696d672e736869656c64732e696f2f64617669642f616c6979756e2f64617261626f6e62612e7376673f7374796c653d666c61742d737175617265)](https://david-dm.org/aliyun/darabonba) [![npm download](https://camo.githubusercontent.com/2815f6a3d474a7eddb5c36ea460ab64edf64473c/68747470733a2f2f696d672e736869656c64732e696f2f6e706d2f646d2f4064617261626f6e62612f7061727365722e7376673f7374796c653d666c61742d737175617265)](https://npmjs.org/package/@darabonba/parser)

一种 OpenAPI 应用的领域特定语言。可以利用它为任意风格的接口生成多语言的 SDK、代码示例、测试用例、接口编排等。

# Get Started


安装 darabonba 命令行工具：

```bash
$ npm install @darabonba/cli -g
```

设置仓库地址：

```bash
$ dara config set registry https://darabonba.api.aliyun.com
```

创建项目文件夹

```bash
$ mkdir <项目名称>
```

# 操作记录

```bash
Administrator@DQRV59CDE8AZCYG MINGW64 /g/GITHUB/aliyun-game
$ source ./venv/Scripts/activate
(venv)
Administrator@DQRV59CDE8AZCYG MINGW64 /g/GITHUB/aliyun-game
$ ll
total 0
drwxr-xr-x 1 Administrator 197121 0  9月  6 21:05 venv/
(venv)
Administrator@DQRV59CDE8AZCYG MINGW64 /g/GITHUB/aliyun-game
$ which npm
/c/Program Files/nodejs/npm
(venv)
Administrator@DQRV59CDE8AZCYG MINGW64 /g/GITHUB/aliyun-game
$ npm install @darabonba/cli -g
C:\Users\Administrator\AppData\Roaming\npm\dara -> C:\Users\Administrator\AppData\Roaming\npm\node_modules\@darabonba\cli\bin\dara.js
+ @darabonba/cli@1.1.3
added 303 packages from 257 contributors in 59.281s
(venv)

$ ls  /c/users/Administrator/AppData/Roaming/npm/dara
/c/users/Administrator/AppData/Roaming/npm/dara*
(venv)
Administrator@DQRV59CDE8AZCYG MINGW64 /g/GITHUB/aliyun-game
$ /c/users/Administrator/AppData/Roaming/npm/dara config set registry https://darabonba.api.aliyun.com

Update successfully!

(venv)
Administrator@DQRV59CDE8AZCYG MINGW64 /g/GITHUB/aliyun-game
$ export PATH=$PATH:/c/users/Administrator/AppData/Roaming/npm/
(venv)

Administrator@DQRV59CDE8AZCYG MINGW64 /g/GITHUB/aliyun-game
$ which dara
/c/users/Administrator/AppData/Roaming/npm/dara
(venv)


```

