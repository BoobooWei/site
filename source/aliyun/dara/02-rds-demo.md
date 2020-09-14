---
title: rds demo
---

# rds

```bash
https://github.com/aliyun/alibabacloud-sdk/blob/master/rds-20140815/main.tea
https://pypi.org/project/alibabacloud-rds20140815/
```

# Darafile

```json
{
  "scope": "alibabacloud",
  "name": "sample",
  "version": "0.0.1",
  "main": "main.dara",
  "libraries": {
    "Console": "darabonba:Console:*",
    "RDS": "alibabacloud:Rds20140815:*",
    "RPC": "alibabacloud:RPC:*",
    "Util": "darabonba:Util:*"
  },
  "java": {
    "package": "aliyun.com.alibabacloud.sample"
  },
  "csharp": {
    "namespace": "Alibabacloud.Sample"
  },
  "php": {
    "package": "Alibabacloud.Sample"
  },
  "python": {
    "package": "alibabacloud_sample"
  }
}
```

## main

```bash
import RDS;
import RPC;
import Util;
import Console;

/**
* Initialization  初始化公共请求参数
*/
static function Initialization(regionId: string)throws : RDS{

    var config = new RPC.Config{};
    // 您的AccessKey ID
    config.accessKeyId = "<accessKeyId>";
    // 您的AccessKey Secret
    config.accessKeySecret = "<accessKeySecret>";
    // 您的可用区ID
    config.regionId = regionId;

    return new RDS(config);
}


/**
* DescribeZones    查询一个阿里云地域下的可用区
*/
static async function DescribeZones(client: RDS, regionId: string)throws: void{
    var req = new RDS.DescribeAvailableZonesRequest {};
    // 可用区所在的地域ID。您可以调用DescribeRegions查看最新的阿里云地域列表
    req.regionId = regionId;
    // 根据汉语、英语和日语筛选返回结果。更多详情，请参见RFC7231
    // 取值范围：
    // zh-CN
    // en-US
    // ja
    // 默认值：zh-CN。
    req.acceptLanguage = "zh-CN";
    var resp = client.describeZones(req);
    Console.log("--------------------查询地域下的可用区--------------------");
    Console.log(Util.toJSONString(resp));
}



static async function main(args: [string]) throws: void {
    // 可用区域Id （请自行配置）
    var regionId = "<regionId>";

    var client = Initialization(regionId);

    // 查询阿里云地域下的可用区
    DescribeZones(client, regionId);

    // 查询创建的安全组的基本信息
    DescribeSecurityGroups(client, regionId);
}
```


## 操作记录

```bash
export PATH=$PATH:/c/users/Administrator/AppData/Roaming/npm/
$ dara install
G:\GITHUB\aliyun-game\Darafile found 4 libraries

fetching from remote repository

G:\GITHUB\aliyun-game\libraries\darabonba_Console_0.1.1\Teafile found 0 libraries

fetching from remote repository

G:\GITHUB\aliyun-game\libraries\alibabacloud_Rds20140815_1.0.0\Teafile found 4 libraries

fetching from remote repository

G:\GITHUB\aliyun-game\libraries\alibabacloud_RPC_0.1.3\Teafile found 3 libraries

fetching from remote repository

G:\GITHUB\aliyun-game\libraries\darabonba_Util_0.1.7\Teafile found 0 libraries

fetching from remote repository

G:\GITHUB\aliyun-game\libraries\alibabacloud_RPC_0.1.3\Teafile found 3 libraries

fetching from remote repository

G:\GITHUB\aliyun-game\libraries\darabonba_Util_0.1.7\Teafile found 0 libraries

fetching from remote repository

G:\GITHUB\aliyun-game\libraries\alibabacloud_RPCUtil_0.1.2\Teafile found 0 libraries

fetching from remote repository

G:\GITHUB\aliyun-game\libraries\alibabacloud_EndpointUtil_0.1.1\Teafile found 0 libraries

fetching from remote repository

G:\GITHUB\aliyun-game\libraries\darabonba_Util_0.1.7\Teafile found 0 libraries

fetching from remote repository

G:\GITHUB\aliyun-game\libraries\alibabacloud_RPCUtil_0.1.2\Teafile found 0 libraries

fetching from remote repository

G:\GITHUB\aliyun-game\libraries\alibabacloud_Credential_0.4.8\Teafile found 0 libraries

fetching from remote repository

G:\GITHUB\aliyun-game\libraries\darabonba_Util_0.1.7\Teafile found 0 libraries

fetching from remote repository

G:\GITHUB\aliyun-game\libraries\alibabacloud_RPCUtil_0.1.2\Teafile found 0 libraries

fetching from remote repository

G:\GITHUB\aliyun-game\libraries\alibabacloud_Credential_0.4.8\Teafile found 0 libraries

fetching from remote repository

14 libraries installed. (0 local, 14 remote)

$ dara codegen python ./tmp
Administrator@DQRV59CDE8AZCYG MINGW64 /g/GITHUB/aliyun-game
$ ll
total 18
-rw-r--r-- 1 Administrator 197121  492  9月  7 21:47  Darafile
drwxr-xr-x 1 Administrator 197121    0  9月  7 21:33  libraries/
-rw-r--r-- 1 Administrator 197121  442  9月  7 21:15  main.dara
-rw-r--r-- 1 Administrator 197121 6382  9月  7 21:30 'new 1.sql'
-rw-r--r-- 1 Administrator 197121 1115  9月  7 21:39  readme.md
drwxr-xr-x 1 Administrator 197121    0  9月  7 21:53  tmp/
drwxr-xr-x 1 Administrator 197121    0  9月  6 21:05  venv/


Administrator@DQRV59CDE8AZCYG MINGW64 /g/GITHUB/aliyun-game
$ ll tmp
total 0
drwxr-xr-x 1 Administrator 197121 0  9月  7 21:53 alibabacloud_sample/

Administrator@DQRV59CDE8AZCYG MINGW64 /g/GITHUB/aliyun-game
$ cat tmp/alibabacloud_sample/client.py
# -*- coding: utf-8 -*-
# This file is auto-generated, don't edit it. Thanks.
from alibabacloud_rds20140815.client import Client as RDSClient
from alibabacloud_tea_rpc import models as rpc_models


class Client(object):
    def __init__(self):
        pass

    @staticmethod
    def initialization(region_id):
        """
        Initialization  初始化公共请求参数
        """
        config = rpc_models.Config(

        )
        # 您的AccessKey ID
        config.access_key_id = "<accessKeyId>"
        # 您的AccessKey Secret
        config.access_key_secret = "<accessKeySecret>"
        # 您的可用区ID
        config.region_id = region_id
        return RDSClient(config)

```

