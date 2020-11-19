---
title: 自建MySQL物理备份计划生效核实demo
---

# 检查物理备份

```bash
# 全备增备并还原
全备份       innobackupex --user=root --password=uplooking /tmp/backup
增量备份1    innobackupex --user=root --password=uplooking --incremental-basedir=/tmp/backup/2016-09-01_11-32-43 --incremental /tmp/backup
增量备份2    innobackupex --user=root --password=uplooking --incremental-basedir= --incremental
增量备份3    innobackupex --user=root --password=uplooking --incremental-basedir= --incremental

全备份还原    
        innobackupex --apply-log  --redo-only /tmp/backup/全备份
        innobackupex --apply-log  --redo-only /tmp/backup/全备份 --incremental-dir=增量1
        innobackupex --apply-log  --redo-only /tmp/backup/全备份 --incremental-dir=增量2
        innobackupex --apply-log  --redo-only /tmp/backup/全备份 --incremental-dir=增量3
        innobackupex --apply-log  /tmp/backup/全备份
        innobackupex --copy-back  /tmp/backup/全备份
```

备份恢复的步骤

```bash
1）停止服务
2）请环境
3）导入数据
    1> apply-log
    2> copy-back
4）修改权限
5）启动服务
6）测试
```

# 总结

