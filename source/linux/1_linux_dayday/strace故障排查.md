---
title: strace 故障排查
---

## 查看进程目前在做什么

```shell
strace -o /tmp/strace_output.txt -T -tt -f -e trace=read,open -p 3962
```

