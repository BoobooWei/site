---
title: MongoDB Cloud 云实践
---

> 记录 MongoDB Cloud 之 MongoDB Altars 实践

# [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)

Move faster with a true multi-cloud database service for MongoDB built for agile teams who’d rather spend time building apps than managing databases.

Available on AWS, Google Cloud, and Azure.

* [01_get_started_with_atlas](/cloud/mongodb_cloud/01_get_started_with_atlas.html)
* [02_熟悉MongoDB Altars 云平台集群管理界面](/cloud/mongodb_cloud/02_熟悉MongoDB Altars 云平台集群管理界面.html)
* [03_熟悉MongoDB Altars 云平台监控Alter功能](/cloud/mongodb_cloud/03_熟悉MongoDB Altars 云平台监控Alter功能.html)

```bash
ll *.md | awk '{print "* ["$9"](/cloud/mongodb_cloud/"$9")"}' | sed 's/.md//'|sed 's/.md/.html/g'
```

# [mtools](http://blog.rueckstiess.com/mtools/index.html)

The following tools are in the mtools collection:

- [mlogfilter](http://blog.rueckstiess.com/mtools/mlogfilter.html#mlogfilter)

  slices log files by time, merges log files, filters slow queries, finds table scans, shortens log lines, filters by other attributes, convert to JSON

- [mloginfo](http://blog.rueckstiess.com/mtools/mloginfo.html#mloginfo)

  returns info about log file, like start and end time, version, binary, special sections like restarts, connections, distinct view

- [mplotqueries](http://blog.rueckstiess.com/mtools/mplotqueries.html#mplotqueries)

  visualize log files with different types of plots (requires `matplotlib`)

- [mlogvis](http://blog.rueckstiess.com/mtools/mlogvis.html#mlogvis)

  creates a self-contained HTML file that shows an interactive visualization in a web browser (as an alternative to `mplotqueries`)

- [mlaunch](http://blog.rueckstiess.com/mtools/mlaunch.html#mlaunch)

  a script to spin up local test environments quickly, including replica sets and sharded systems (requires `pymongo`)

- [mtransfer](http://blog.rueckstiess.com/mtools/mtransfer.html#mtransfer)

  an experimental script to transfer WiredTiger databases between MongoDB instances by copying data files (requires `pymongo` and `wiredtiger`)
