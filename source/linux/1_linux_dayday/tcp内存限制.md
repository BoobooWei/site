---
title: TCP 内存限制
---

```bash
# 每个tcp连接的内存页的限制（min 默认 max）
[root@ToBeRoot ~]# cat  /proc/sys/net/ipv4/tcp_wmem
4096	16384	3477504
[root@ToBeRoot ~]# cat  /proc/sys/net/ipv4/tcp_rmem
4096	87380	3477504
[root@ToBeRoot ~]# cat  /proc/sys/net/ipv4/tcp_mem
81504	108672	163008

# 显示一个页4096bytes
[root@ToBeRoot ~]# getconf PAGESIZE
4096

[root@ToBeRoot ~]# dmesg|grep TCP
TCP established hash table entries: 131072 (order: 8, 1048576 bytes)
TCP bind hash table entries: 65536 (order: 7, 524288 bytes)
TCP: Hash tables configured (established 131072 bind 65536)
TCP reno registered
TCP cubic registered


getconf PAGESIZE
cat  /proc/sys/net/ipv4/tcp_mem
cat  /proc/sys/net/ipv4/tcp_rmem
cat  /proc/sys/net/ipv4/tcp_wmem

dmesg|grep TCP
```


# 案例分析
```bash
root@od-mongdb-01:/etc/security# getconf PAGESIZE
4096
# 单位页的个数
root@od-mongdb-01:/etc/security# cat  /proc/sys/net/ipv4/tcp_mem
384489  512654  768978
1.4G     1.95G   2.93G

# 单位bytes
root@od-mongdb-01:/etc/security# cat  /proc/sys/net/ipv4/tcp_rmem
4096  87380  6291456
root@od-mongdb-01:/etc/security# cat  /proc/sys/net/ipv4/tcp_wmem
4096  16384  4194304
最小   默认  最大

# 查看
[    0.694256] TCP established hash table entries: 262144 (order: 9, 2097152 bytes)
[    0.695118] TCP bind hash table entries: 65536 (order: 8, 1048576 bytes)
[    0.695196] TCP: Hash tables configured (established 262144 bind 65536)
[  344.872546] TCP: request_sock_TCP: Possible SYN flooding on port 27017. Sending cookies.  Check SNMP counters.
[  686.483796] TCP: request_sock_TCP: Possible SYN flooding on port 27017. Sending cookies.  Check SNMP counters.
[ 2085.226327] TCP: request_sock_TCP: Possible SYN flooding on port 27017. Sending cookies.  Check SNMP counters.
[ 2262.665499] TCP: request_sock_TCP: Possible SYN flooding on port 27017. Sending cookies.  Check SNMP counters.
[ 6916.712860] TCP: request_sock_TCP: Possible SYN flooding on port 27017. Sending cookies.  Check SNMP counters.
[78045.963517] TCP: request_sock_TCP: Possible SYN flooding on port 27017. Sending cookies.  Check SNMP counters.
```

三个TCP调整语句为:

```bash
echo "384489 512654 4194304" > /proc/sys/net/ipv4/tcp_mem
```
