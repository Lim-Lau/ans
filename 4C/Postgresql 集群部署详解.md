---
title: Postgresql 集群部署详解 
tags: postgresql,集群,运维,2020-04-22
grammar_cjkRuby: true
---


# 环境准备


![环境信息](./attachments/1588227709930.table.html)


# 前提准备

更新安装包

``` shell?linenums
yum clean all && yum makecache && yum update -y
```
同步时间 （避免某些证书过期）
```shell?linenums
date -s "2020-03-03 10:00:20" && clock -w
yum install ntp -y
ntpdate -u 0.centos.pool.ntp.org
```
# 创建postgres用户
```shell?linenums
groupadd postgres
useradd -g postgres postgres
```
# 安装

> 可参加 [postgresql官网](https://www.postgresql.org/download/linux/redhat/)安装说明。


## install the repository RPM

```shell?linenums
yum install https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm -y
//或
yum install http://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm -y
```

## Install the client packages

```shell?linenums
yum install postgresql12 -y
```

## Optionally install the server packages

```shell?linenums
yum install postgresql12-server -y
```

## 部分工具、开发包

```shell?linenums
yum install postgresql12-contrib -y

yum install postgresql12-devel -y
//或进一步安装
yum install postgresql12 postgresql12-server postgresql12-contrib postgresql12-devel -y
```

## 初始化数据库
### 路径问题

 1. 使用默认路径，适用于单节点
   

``` shell?linenums
/usr/pgsql-12/bin/postgresql-12-setup initdb
```

 2. 指定路径，适用于主从复制（本文默认）

``` shell?linenums
# 数据目录

mkdir -p /app/pgsql/data && chown postgres:postgres /app/pgsql/data

# 用于归档的目录（主从复制时用） 
mkdir -p /app/pgsql/pg_archive && chown postgres:postgres /app/pgsql/pg_archive cd /app/pgsql && chmod 700 data; cd /app/pgsql && chmod 700 pg_archive

su - postgres

/usr/pgsql-12/bin/initdb -D /app/pgsql/data/
```

 3. 修改数据库路径，以root身份

``` shell?linenums
vim /usr/lib/systemd/system/postgresql-12.service

# Location of database directory

Environment=PGDATA=/app/pgsql/data/

```
## enable automatic start，以root身份
```shell?linenums
systemctl enable postgresql-12

systemctl start postgresql-12
```

## 