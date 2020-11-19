---
title: Redis 运维实践
---

# Redis 内存使用率偏高

## 问题原因

内存使用率居高不下很多情况下都是大Key导致的。

大Key一般情况下认为：String类型的Value大于10240的是大Key，List长度大于10240认为是大Key，Hash Field的数目大于10240认为是大Key。

Redis中的数据结构对服务的性能有着举足轻重的影响，如果大key较多，容易形成性能瓶颈，甚至降低业务稳定性。

定期分析内存并根据分析结果优化内存，可以保持服务的稳定和高效。

## 解决建议

1. 在业务低峰期定位大Key （内存分析法；Python工具法）
2. 优化大Key：（根据贵司的实际业务场景进行大key的删除和优化）
   1. 1. 优化big key的原则就是string减少字符串长度，list、hash、set、zset等减少成员数
      2. 以hash类型举例来说，对于field过多的场景，可以根据field进行hash取模，生成一个新的key，例如原来的
         hash_key:{filed1:value, filed2:value, filed3:value ...}，可以hash取模后形成如下key:value形式
         hash_key:mod1:{filed1:value}
         hash_key:mod2:{filed2:value}
         hash_key:mod3:{filed3:value}
         ...
         取模后，将原先单个key分成多个key，每个key filed个数为原先的1/N
      3. string类型的big key，如文章正文，建议不要存入redis，用文档型数据库MongoDB代替或者直接缓存到CDN上等方式优化

### 帮助文档

Redis内存分析方法，请详见： https://help.aliyun.com/document_detail/141763.html

Python工具法，请详见： https://help.aliyun.com/knowledge_detail/56949.html

# 批量删除redis的key

> 2018-02-27 驻云DBA组

## 需求

redis数据清理：

目前在0号库中，存在一个hash键mserver_redis_requestUk，数据量达到20G左右，需要在不影响生成环境的情况下，删除该Key。


## 解决方法

### 思路

从Redis2.8版本开始支持HSCAN命令，通过m次时间复杂度为O(1)的方式，遍历包含n个元素的大key。

**这样可以避免单个O(n)的大命令，从而避免Redis阻塞。 **



HSCAN 命令返回的每个元素都是一个键值对，一个键值对由一个键和一个值组成。



### 具体操作 

通过hscan命令，每次获取500个字段，再用hdel命令，每次删除1个字段。
Python代码：

```shell
# -*- coding:utf-8 -*-
import redis

def del_large_hash():
    r = redis.StrictRedis(host='云redis地址', port=6379, password='密码')
    large_hash_key ="mserver_redis_requestUk" #要删除的大hash键名
    cursor = '0'
    cursor, data = r.hscan(large_hash_key, cursor=cursor, count=500)
    for item in data.items():
        r.hdel(large_hash_key, item[0])

del_large_hash()
```

* 准备一台ECS服务器可以访问云redis
* python 版本 >= 2.7
* 安装python的redis包，命令如下

```shell
pip install redis
```

准备好环境和脚本后，就可以优雅的删除大Key了。

## 总结

* 该方法每次获取500个字段，再用hdel命令，每次删除1个字段；


* 通过m次时间复杂度为O(1)的方式，遍历大key`mserver_redis_requestUk`；


* m=大key的总字段/500 ；


* 每次获取的字段数，可以在脚本中自行调整。

**Hscan实现增量迭代从而避免Redis阻塞。 **





# Redis 迁移合并

> 2018-06-22 大宝

[TOC]

## 需求

目前client中存在db0，db1，db2，test库中存在db0，db1。
客户希望将test库中的数据与client合并，且不覆盖client库。


## 调研情况

目前test中存在db0，db1，client中也有db0，db1。

```shell
# client Keyspace
db0:keys=186,expires=4,avg_ttl=6348143
db1:keys=192,expires=9,avg_ttl=281534
db2:keys=1,expires=0,avg_ttl=0


# test Keyspace
db0:keys=25113,expires=103,avg_ttl=31130691210
db1:keys=954,expires=0,avg_ttl=0
```



1. 合并两个库，目前客户是否使用了命名空间，来区分key呢？

2. 若客户使用了命名空间，则合并可以保证数据一致性，合并方法使用逻辑备份和导入即可：redis-dump 和redis-load命令

3. 若客户没有使用命名空间，则合并时遇到有冲突的key，会自动跳过，只保留目标库中的key，导致数据不一致。需要客户判断是否能够接受数据的不一致性。


**客户反馈key的命名空间有些有，有些没有。因此请客户考虑上述第三点。**

**A**: 可以接受数据不一致，冲突时保留目标库的key

## 迁移数据

```
# 备份test
[root@xym-csos-server ~]# redis-dump -u :6FtkIazwqzC@r-bp114417c5d282b4.redis.rds.aliyuncs.com:6379 > redis_test.json

# 为了保险也备份client
[root@xym-csos-server ~]# redis-dump -u :KIU495bIqL@r-bp1cdd51ee8a8824.redis.rds.aliyuncs.com:6379 > redis-client.json


[root@xym-csos-server ~]# ll -h
total 42M
-rw-r--r-- 1 root root 602K Jun 22 22:03 redis-client.json
-rw-r--r-- 1 root root  42M Jun 22 22:03 redis_test.json
-rwxr-xr-x 1 root root 5.0K Jun 21 14:30 zabbix_proxy-3.2.1.sh

# 将test导入client，报错utf8编码问题

[root@xym-csos-server ~]# redis-load -u :xxx@r-bp1cdd51ee8a8824.redis.rds.aliyuncs.com:6379 < redis_test.json 
ERROR (Yajl::ParseError): lexical error: invalid bytes in UTF8 string.
          ","value":"\u0000\u000F\u0004󮮰BappHostName󮮰B10.10.
                     (right here) ------^
					 

#如果报错，请使用-n选项，使用请参考官方，请谨慎使用！
#< test.json redis-load -n    //-n （以二进制形式导入）

Loading data with binary strings
If you have binary or serialized data in your Redis database, the YAJL parser may not load your dump file because it sees some of the binary data as 'invalid bytes in UTF8 string'. If you are certain that your data is binary and not malformed UTF8, you can use the -n flag to redis-load to tell YAJL to not check the input for UTF8 validity. Use with caution!

# 添加-n参数以二进制方式导入

[root@xym-csos-server ~]# redis-load -n -u :xxx@r-bp1cdd51ee8a8824.redis.rds.aliyuncs.com:6379 < redis_test.json 

# 查看当前的client情况如下

r-bp1cdd51ee8a8824.redis.rds.aliyuncs.com:6379>info
# Server
redis_version:2.8.19
redis_git_sha1:84975300
redis_git_dirty:1
redis_build_id:716ab38e57364d4
redis_mode:standalone
os:Linux  
arch_bits:64
multiplexing_api:epoll
gcc_version:0.0.0
process_id:51718
run_id:36c0d2baa517fad865786cff19a3b00d1497981a
server_id:0
tcp_port:6379
uptime_in_seconds:2362924
uptime_in_days:27
hz:10
lru_clock:2949784
config_file:redis.conf
# Clients
connected_clients:75
client_longest_output_list:0
client_biggest_input_buf:0
blocked_clients:0
# Memory
used_memory:34908112
used_memory_human:33.29M
used_memory_rss:34844672
used_memory_peak:35549288
used_memory_peak_human:33.90M
used_memory_lua:45056
mem_fragmentation_ratio:1.00
mem_allocator:jemalloc-3.6.0
# Stats
total_connections_received:524441
total_commands_processed:25328930
instantaneous_ops_per_sec:6
total_net_input_bytes:1092820494
total_net_output_bytes:6478654065
instantaneous_input_kbps:0.11
instantaneous_output_kbps:2.47
input_limit_tokens:31457280
output_limit_tokens:31457275
input_strict_limit:0
output_strict_limit:0
rejected_connections:0
rejected_connections_status:0
sync_full:0
sync_partial_ok:0
sync_partial_err:0
expired_keys:116358
evicted_keys:0
evicted_keys_per_sec:0
keyspace_hits:1521054
keyspace_misses:759414
hits_per_sec:0.00
misses_per_sec:0.00
hit_rate_percentage:100.00
pubsub_channels:0
pubsub_patterns:66
latest_fork_usec:692
traffic_control_read:0
traffic_control_read_status:0
traffic_control_write:0
traffic_control_write_status:0
total_bigkeys:0
bigkeys_status:0
stat_avg_rt:0
stat_max_rt:867
# CPU
used_cpu_sys:1394.92
used_cpu_user:1545.75
used_cpu_sys_children:0.06
used_cpu_user_children:0.06
# Keyspace
db0:keys=25302,expires=110,avg_ttl=19648674653
db1:keys=1089,expires=3,avg_ttl=2588349
db2:keys=1,expires=0,avg_ttl=0
# Cluster
databases:256
nodecount:1
# SSL
ssl_enabled:0
ssl_protocols:TLSv1,TLSv1.1,TLSv1.2
ssl_client_count:0
ssl_accept_count:0
ssl_reject_count:0
```


## 总结

1. 物理备份恢复redis-port reload代表全量导入；sync代表全量加增量 
2. 逻辑备份恢复redis-dump redis-load

# Redis 常见报错解决

## RDB失败以及AOF文件异常大

### 报错原因

```shell
redis从库报错：
7022:S 23 Jul 21:30:10.681 * Starting automatic rewriting of AOF on 232547% growth
7022:S 23 Jul 21:30:10.681 # Can't rewrite append only file in background: fork: Cannot allocate memory
```

### 原因和解决

在线关闭从库aof功能

```shell
172.16.1.167:6379> CONFIG SET appendonly no
OK
```

删除aof文件

```shell
rm ‐rf appendonly.aof
```


从库无法固化以及aof文件出错原因
当Redis内存使用量约超过物理内存一半时，Redis在fork进程进行bgsave或者BGREWRITEAOF时将会失败，此时无法rdb持久化以及AOF rewrite，通常在日志中可以看到：
`Can't save in background: fork: Cannot allocate memory`
这种情况，可以将Linux的`vm.overcommit_memory = 1`，但理论上也会存在风险，如果在rdb dump期间业务有相当多的数据更新，将会导致系统内存不足而发生OOM
所以如果业务无需固化数据，直接将rdb和aof都关闭即可。

## MISCONF

### 报错明细

```shell
MISCONF Redis is configured to save RDB snapshots, but it is currently not able to persist on disk. Commands that may modify the data set are disabled, because this instance is configured to report errors during writes if RDB snapshotting fails (stop-writes-on-bgsave-error option). Please check the Redis logs for details about the RDB error.
```

### 原因和解决

其原因是因为强制把`redis`快照关闭了导致不能持久化的问题，在网上查了一些相关解决方案，通过`stop-writes-on-bgsave-error`值设置为`no`即可避免这种问题。

有两种修改方法，一种是通过redis命令行修改，另一种是直接修改redis.conf配置文件

命令行修改方式示例：

`127.0.0.1:6379> config set stop-writes-on-bgsave-error no`

修改`redis.conf`文件：`stop-writes-on-bgsave-error`字符串所在位置，接着把后面的`yes`设置为`no`即可。

# centos6.5 安装ruby 2.3.1

> 2018-06-25 Booboowei

## 步骤概览

```shell
#现在的版本
[root@hd4 /]# ruby --version
ruby 1.8.7 (2013-06-27 patchlevel 374) [x86_64-linux]

#升级过程
[root@hd4 /]# curl -sSL https://rvm.io/mpapis.asc | gpg2 --import -
[root@hd4 /]# curl -L get.rvm.io | bash -s stable
[root@hd4 /]# source /etc/profile.d/rvm.sh
[root@hd4 /]# rvm requirements
[root@hd4 /]# rvm install ruby 2.3.1

#设置默认版本
[root@hd4 /]# rvm --default use 2.3.1

#检查安装结果
[root@hd4 /]# ruby -v
ruby 2.3.1p112 (2016-04-26 revision 54768) [x86_64-linux]
```

## 操作明细

```shell
[root@xym-csos-server ~]# which ruby
/usr/bin/ruby
[root@xym-csos-server ~]# ruby -v
ruby 1.8.7 (2013-06-27 patchlevel 374) [x86_64-linux]
[root@xym-csos-server ~]#  curl -sSL https://rvm.io/mpapis.asc | gpg2 --import -
gpg: directory `/root/.gnupg' created
gpg: new configuration file `/root/.gnupg/gpg.conf' created
gpg: WARNING: options in `/root/.gnupg/gpg.conf' are not yet active during this run
gpg: keyring `/root/.gnupg/secring.gpg' created
gpg: keyring `/root/.gnupg/pubring.gpg' created
gpg: /root/.gnupg/trustdb.gpg: trustdb created
gpg: key D39DC0E3: public key "Michal Papis (RVM signing) <mpapis@gmail.com>" imported
gpg: Total number processed: 1
gpg:               imported: 1  (RSA: 1)
gpg: no ultimately trusted keys found
[root@xym-csos-server ~]#  curl -L get.rvm.io | bash -s stable
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 24361  100 24361    0     0   5412      0  0:00:04  0:00:04 --:--:-- 24458
Downloading https://github.com/rvm/rvm/archive/1.29.3.tar.gz
Downloading https://github.com/rvm/rvm/releases/download/1.29.3/1.29.3.tar.gz.asc
gpg: Signature made Mon 11 Sep 2017 04:59:21 AM CST using RSA key ID BF04FF17
gpg: Good signature from "Michal Papis (RVM signing) <mpapis@gmail.com>"
gpg:                 aka "Michal Papis <michal.papis@toptal.com>"
gpg:                 aka "[jpeg image of size 5015]"
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: 409B 6B17 96C2 7546 2A17  0311 3804 BB82 D39D C0E3
     Subkey fingerprint: 62C9 E5F4 DA30 0D94 AC36  166B E206 C29F BF04 FF17
GPG verified '/usr/local/rvm/archives/rvm-1.29.3.tgz'
Creating group 'rvm'

Installing RVM to /usr/local/rvm/
Installation of RVM in /usr/local/rvm/ is almost complete:

  * First you need to add all users that will be using rvm to 'rvm' group,
    and logout - login again, anyone using rvm will be operating with `umask u=rwx,g=rwx,o=rx`.

  * To start using RVM you need to run `source /etc/profile.d/rvm.sh`
    in all your open shell windows, in rare cases you need to reopen all shell windows.
[root@xym-csos-server ~]# source /etc/profile.d/rvm.sh
[root@xym-csos-server ~]# rvm requirements
Checking requirements for centos.
Installing requirements for centos.
Installing required packages: bison, gcc-c++, libffi-devel, libtool, readline-devel, sqlite-devel, libyaml-devel...........
Requirements installation successful.
[root@xym-csos-server ~]# rvm install ruby 2.3.1
Searching for binary rubies, this might take some time.
Found remote file https://rvm_io.global.ssl.fastly.net/binaries/centos/6/x86_64/ruby-2.3.1.tar.bz2
Checking requirements for centos.
Requirements installation successful.
ruby-2.3.1 - #configure
ruby-2.3.1 - #download
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 22.5M  100 22.5M    0     0  3730k      0  0:00:06  0:00:06 --:--:-- 6659k
ruby-2.3.1 - #validate archive
ruby-2.3.1 - #extract
ruby-2.3.1 - #validate binary
ruby-2.3.1 - #setup
ruby-2.3.1 - #gemset created /usr/local/rvm/gems/ruby-2.3.1@global
ruby-2.3.1 - #importing gemset /usr/local/rvm/gemsets/global.gems...................................
ruby-2.3.1 - #generating global wrappers........
ruby-2.3.1 - #gemset created /usr/local/rvm/gems/ruby-2.3.1
ruby-2.3.1 - #importing gemsetfile /usr/local/rvm/gemsets/default.gems evaluated to empty gem list
ruby-2.3.1 - #generating default wrappers........
[root@xym-csos-server ~]# rvm --default use 2.3.1
Using /usr/local/rvm/gems/ruby-2.3.1
[root@xym-csos-server ~]# ruby -v
ruby 2.3.1p112 (2016-04-26 revision 54768) [x86_64-linux]



root@xym-csos-server ~]# which gem
/usr/local/rvm/rubies/ruby-2.3.1/bin/gem


[root@xym-csos-server ~]# gem install redis-dump -V
GET https://rubygems.org/latest_specs.4.8.gz
200 OK
GET https://rubygems.org/quick/Marshal.4.8/redis-dump-0.4.0.gemspec.rz
200 OK
GET https://rubygems.org/specs.4.8.gz
200 OK
GET https://rubygems.org/quick/Marshal.4.8/yajl-ruby-1.4.0.gemspec.rz
200 OK
GET https://rubygems.org/quick/Marshal.4.8/redis-4.0.1.gemspec.rz
200 OK
GET https://rubygems.org/quick/Marshal.4.8/uri-redis-0.4.2.gemspec.rz
200 OK
GET https://rubygems.org/quick/Marshal.4.8/drydock-0.6.9.gemspec.rz
200 OK
Downloading gem yajl-ruby-1.4.0.gem
GET https://rubygems.org/gems/yajl-ruby-1.4.0.gem
Fetching: yajl-ruby-1.4.0.gem (100%)
200 OK
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/.codeclimate.yml
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/.gitignore
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/.rspec
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/.travis.yml
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/CHANGELOG.md
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/Gemfile
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/LICENSE
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/README.md
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/Rakefile
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/benchmark/encode.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/benchmark/encode_json_and_marshal.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/benchmark/encode_json_and_yaml.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/benchmark/http.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/benchmark/parse.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/benchmark/parse_json_and_marshal.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/benchmark/parse_json_and_yaml.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/benchmark/parse_stream.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/benchmark/subjects/item.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/benchmark/subjects/ohai.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/benchmark/subjects/ohai.marshal_dump
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/benchmark/subjects/ohai.yml
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/benchmark/subjects/twitter_search.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/benchmark/subjects/twitter_stream.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/benchmark/subjects/unicode.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/examples/encoding/chunked_encoding.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/examples/encoding/one_shot.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/examples/encoding/to_an_io.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/examples/http/twitter_search_api.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/examples/http/twitter_stream_api.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/examples/parsing/from_file.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/examples/parsing/from_stdin.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/examples/parsing/from_string.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/ext/yajl/api/yajl_common.h
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/ext/yajl/api/yajl_gen.h
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/ext/yajl/api/yajl_parse.h
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/ext/yajl/api/yajl_version.h
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/ext/yajl/extconf.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/ext/yajl/yajl.c
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/ext/yajl/yajl_alloc.c
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/ext/yajl/yajl_alloc.h
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/ext/yajl/yajl_buf.c
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/ext/yajl/yajl_buf.h
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/ext/yajl/yajl_bytestack.h
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/ext/yajl/yajl_encode.c
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/ext/yajl/yajl_encode.h
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/ext/yajl/yajl_ext.c
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/ext/yajl/yajl_ext.h
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/ext/yajl/yajl_gen.c
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/ext/yajl/yajl_lex.c
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/ext/yajl/yajl_lex.h
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/ext/yajl/yajl_parser.c
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/ext/yajl/yajl_parser.h
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/ext/yajl/yajl_version.c
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/lib/yajl.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/lib/yajl/bzip2.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/lib/yajl/bzip2/stream_reader.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/lib/yajl/bzip2/stream_writer.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/lib/yajl/deflate.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/lib/yajl/deflate/stream_reader.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/lib/yajl/deflate/stream_writer.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/lib/yajl/gzip.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/lib/yajl/gzip/stream_reader.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/lib/yajl/gzip/stream_writer.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/lib/yajl/http_stream.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/lib/yajl/json_gem.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/lib/yajl/json_gem/encoding.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/lib/yajl/json_gem/parsing.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/lib/yajl/version.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/script/bootstrap
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/encoding/encoding_spec.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/global/global_spec.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/http/fixtures/http.bzip2.dump
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/http/fixtures/http.chunked.dump
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/http/fixtures/http.deflate.dump
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/http/fixtures/http.error.dump
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/http/fixtures/http.gzip.dump
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/http/fixtures/http.html.dump
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/http/fixtures/http.raw.dump
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/http/http_delete_spec.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/http/http_error_spec.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/http/http_get_spec.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/http/http_post_spec.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/http/http_put_spec.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/http/http_stream_options_spec.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/json_gem_compatibility/compatibility_spec.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/active_support_spec.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/chunked_spec.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/fail.15.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/fail.16.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/fail.17.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/fail.26.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/fail11.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/fail12.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/fail13.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/fail14.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/fail19.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/fail20.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/fail21.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/fail22.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/fail23.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/fail24.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/fail25.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/fail27.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/fail28.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/fail3.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/fail4.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/fail5.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/fail6.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/fail9.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.array.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.codepoints_from_unicode_org.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.contacts.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.db100.xml.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.db1000.xml.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.dc_simple_with_comments.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.deep_arrays.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.difficult_json_c_test_case.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.difficult_json_c_test_case_with_comments.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.doubles.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.empty_array.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.empty_string.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.escaped_bulgarian.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.escaped_foobar.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.item.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.json-org-sample1.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.json-org-sample2.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.json-org-sample3.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.json-org-sample4-nows.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.json-org-sample4.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.json-org-sample5.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.map-spain.xml.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.ns-invoice100.xml.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.ns-soap.xml.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.numbers-fp-4k.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.numbers-fp-64k.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.numbers-int-4k.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.numbers-int-64k.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.twitter-search.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.twitter-search2.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.unicode.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass.yelp.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass1.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass2.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures/pass3.json
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/fixtures_spec.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/large_number_spec.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/parsing/one_off_spec.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/projection/project_file.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/projection/projection.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/rcov.opts
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/spec/spec_helper.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/tasks/compile.rake
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/tasks/rspec.rake
/usr/local/rvm/gems/ruby-2.3.1/gems/yajl-ruby-1.4.0/yajl-ruby.gemspec
Building native extensions.  This could take a while...
/usr/local/rvm/rubies/ruby-2.3.1/bin/ruby extconf.rb
creating Makefile
make "DESTDIR="
compiling yajl_buf.c
compiling yajl_lex.c
yajl_lex.c: In function ‘yajl_tok_name’:
yajl_lex.c:42: warning: enumeration value ‘yajl_tok_comment’ not handled in switch
compiling yajl_version.c
compiling yajl_parser.c
compiling yajl_encode.c
compiling yajl_ext.c
compiling yajl_gen.c
compiling yajl_alloc.c
compiling yajl.c
linking shared-object yajl/yajl.so
make "DESTDIR=" install
/usr/bin/install -c -m 0755 yajl.so ./.gem.20180622-9454-y24tn4/yajl

Successfully installed yajl-ruby-1.4.0
Downloading gem redis-4.0.1.gem
GET https://rubygems.org/gems/redis-4.0.1.gem
Fetching: redis-4.0.1.gem (100%)
200 OK
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/.gitignore
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/.travis.yml
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/.travis/Gemfile
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/.yardopts
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/CHANGELOG.md
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/Gemfile
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/LICENSE
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/README.md
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/benchmarking/logging.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/benchmarking/pipeline.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/benchmarking/speed.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/benchmarking/suite.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/benchmarking/worker.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/bors.toml
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/examples/basic.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/examples/consistency.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/examples/dist_redis.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/examples/incr-decr.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/examples/list.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/examples/pubsub.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/examples/sentinel.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/examples/sentinel/sentinel.conf
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/examples/sentinel/start
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/examples/sets.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/examples/unicorn/config.ru
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/examples/unicorn/unicorn.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/lib/redis.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/lib/redis/client.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/lib/redis/connection.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/lib/redis/connection/command_helper.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/lib/redis/connection/hiredis.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/lib/redis/connection/registry.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/lib/redis/connection/ruby.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/lib/redis/connection/synchrony.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/lib/redis/distributed.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/lib/redis/errors.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/lib/redis/hash_ring.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/lib/redis/pipeline.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/lib/redis/subscribe.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/lib/redis/version.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/makefile
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/redis.gemspec
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/bitpos_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/blocking_commands_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/client_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/command_map_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/commands_on_hashes_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/commands_on_hyper_log_log_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/commands_on_lists_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/commands_on_sets_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/commands_on_sorted_sets_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/commands_on_strings_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/commands_on_value_types_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/connection_handling_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/connection_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/db/.gitkeep
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/distributed_blocking_commands_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/distributed_commands_on_hashes_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/distributed_commands_on_hyper_log_log_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/distributed_commands_on_lists_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/distributed_commands_on_sets_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/distributed_commands_on_sorted_sets_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/distributed_commands_on_strings_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/distributed_commands_on_value_types_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/distributed_commands_requiring_clustering_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/distributed_connection_handling_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/distributed_internals_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/distributed_key_tags_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/distributed_persistence_control_commands_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/distributed_publish_subscribe_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/distributed_remote_server_control_commands_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/distributed_scripting_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/distributed_sorting_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/distributed_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/distributed_transactions_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/encoding_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/error_replies_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/fork_safety_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/helper.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/helper_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/internals_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/lint/blocking_commands.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/lint/hashes.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/lint/hyper_log_log.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/lint/lists.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/lint/sets.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/lint/sorted_sets.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/lint/strings.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/lint/value_types.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/persistence_control_commands_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/pipelining_commands_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/publish_subscribe_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/remote_server_control_commands_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/scanning_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/scripting_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/sentinel_command_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/sentinel_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/sorting_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/ssl_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/support/connection/hiredis.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/support/connection/ruby.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/support/connection/synchrony.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/support/redis_mock.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/support/ssl/gen_certs.sh
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/support/ssl/trusted-ca.crt
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/support/ssl/trusted-ca.key
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/support/ssl/trusted-cert.crt
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/support/ssl/trusted-cert.key
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/support/ssl/untrusted-ca.crt
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/support/ssl/untrusted-ca.key
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/support/ssl/untrusted-cert.crt
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/support/ssl/untrusted-cert.key
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/support/wire/synchrony.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/support/wire/thread.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/synchrony_driver.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/test.conf.erb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/thread_safety_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/transactions_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/unknown_commands_test.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-4.0.1/test/url_param_test.rb
Successfully installed redis-4.0.1
Downloading gem uri-redis-0.4.2.gem
GET https://rubygems.org/gems/uri-redis-0.4.2.gem
Fetching: uri-redis-0.4.2.gem (100%)
200 OK
/usr/local/rvm/gems/ruby-2.3.1/gems/uri-redis-0.4.2/CHANGES.txt
/usr/local/rvm/gems/ruby-2.3.1/gems/uri-redis-0.4.2/LICENSE.txt
/usr/local/rvm/gems/ruby-2.3.1/gems/uri-redis-0.4.2/README.rdoc
/usr/local/rvm/gems/ruby-2.3.1/gems/uri-redis-0.4.2/Rakefile
/usr/local/rvm/gems/ruby-2.3.1/gems/uri-redis-0.4.2/VERSION.yml
/usr/local/rvm/gems/ruby-2.3.1/gems/uri-redis-0.4.2/lib/uri/redis.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/uri-redis-0.4.2/try/10_uri_redis_try.rb
Successfully installed uri-redis-0.4.2
Downloading gem drydock-0.6.9.gem
GET https://rubygems.org/gems/drydock-0.6.9.gem
Fetching: drydock-0.6.9.gem (100%)
200 OK
/usr/local/rvm/gems/ruby-2.3.1/gems/drydock-0.6.9/CHANGES.txt
/usr/local/rvm/gems/ruby-2.3.1/gems/drydock-0.6.9/LICENSE.txt
/usr/local/rvm/gems/ruby-2.3.1/gems/drydock-0.6.9/README.rdoc
/usr/local/rvm/gems/ruby-2.3.1/gems/drydock-0.6.9/Rakefile
/usr/local/rvm/gems/ruby-2.3.1/gems/drydock-0.6.9/bin/example
/usr/local/rvm/gems/ruby-2.3.1/gems/drydock-0.6.9/drydock.gemspec
/usr/local/rvm/gems/ruby-2.3.1/gems/drydock-0.6.9/lib/drydock.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/drydock-0.6.9/lib/drydock/console.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/drydock-0.6.9/lib/drydock/mixins.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/drydock-0.6.9/lib/drydock/mixins/object.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/drydock-0.6.9/lib/drydock/mixins/string.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/drydock-0.6.9/lib/drydock/screen.rb
Successfully installed drydock-0.6.9
Downloading gem redis-dump-0.4.0.gem
GET https://rubygems.org/gems/redis-dump-0.4.0.gem
Fetching: redis-dump-0.4.0.gem (100%)
200 OK
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-dump-0.4.0/CHANGES.txt
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-dump-0.4.0/LICENSE.txt
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-dump-0.4.0/README.rdoc
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-dump-0.4.0/Rakefile
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-dump-0.4.0/VERSION
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-dump-0.4.0/bin/redis-dump
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-dump-0.4.0/bin/redis-load
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-dump-0.4.0/bin/redis-report
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-dump-0.4.0/gem-public_cert.pem
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-dump-0.4.0/lib/redis/dump.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-dump-0.4.0/redis-dump.gemspec
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-dump-0.4.0/try/10_redis_dump_try.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-dump-0.4.0/try/20_dump_specific_keys_try.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-dump-0.4.0/try/30_dump_base64_try.rb
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-dump-0.4.0/try/db0.json
/usr/local/rvm/gems/ruby-2.3.1/gems/redis-dump-0.4.0/try/redis.conf
/usr/local/rvm/gems/ruby-2.3.1/bin/redis-dump
/usr/local/rvm/gems/ruby-2.3.1/bin/redis-load
/usr/local/rvm/gems/ruby-2.3.1/bin/redis-report
Successfully installed redis-dump-0.4.0
Parsing documentation for drydock-0.6.9
Parsing sources...
100% [ 9/ 9]  lib/drydock/screen.rb       
Installing ri documentation for drydock-0.6.9
Parsing documentation for redis-4.0.1
Parsing sources...
100% [14/14]  lib/redis/version.rb                  
Installing ri documentation for redis-4.0.1
Parsing documentation for redis-dump-0.4.0
Parsing sources...
100% [ 3/ 3]  lib/redis/dump.rb
Installing ri documentation for redis-dump-0.4.0
Parsing documentation for uri-redis-0.4.2
Parsing sources...
100% [ 3/ 3]  lib/uri/redis.rb
Installing ri documentation for uri-redis-0.4.2
Parsing documentation for yajl-ruby-1.4.0
Parsing sources...
100% [16/16]  lib/yajl/yajl.so                 
Installing ri documentation for yajl-ruby-1.4.0
Done installing documentation for drydock, redis, redis-dump, uri-redis, yajl-ruby after 1 seconds
5 gems installed
```




