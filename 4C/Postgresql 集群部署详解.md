---
title: Postgresql 集群部署详解 
tags: postgresql,集群,运维
grammar_cjkRuby: true
---


# 环境准备

![环境信息](./attachments/1588227709930.table.html)

# 前提准备
更新安装包

``` shell?linenums
yum clean all && yum makecache && yum update -y
```
同步时间
```shell?linenums
date -s "2020-03-03 10:00:20" && clock -w
yum install ntp -y
ntpdate -u 0.centos.pool.ntp.org
```

