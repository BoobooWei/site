---
title: Screen
---


# 新建screen

screen -S rds

# 以上命令按回车后即登陆到screen中
# 执行下载命令
curl --limit-rate 9M -o hins4764419_data_20180806115449.tar.gz "http://rdsbak-shanghai-v2.oss-cn-shanghai-internal.aliyuncs.com/custins6151273/hins4764419_data_20180806115449.tar.gz?OSSAccessKeyId=LTAIyKzxtSYNknVO&Expires=1533623708&Signature=T0vfa5rSrRErTdx%2Br4PtHcra1ss%3D"

# 登陆新的bash后执行
screen -ls # 查看当前所有的screen窗口

# 登陆到某一个
screen -dr rds

# 退出exit
