---
title: Postgresql及插件安装 
tags: postgresql,插件,安装
grammar_cjkRuby: true
---

# yum命令安装pg及插件

> pg安装，参照[官网](https://www.postgresql.org/download/linux/redhat/)。
说明：服务器版本-centos7.x pg版本-12.x

```sh?linenums
# Install the repository RPM:
yum install https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm
# Install the client packages:
yum install postgresql12
# Optionally install the server packages:
yum install postgresql12-server
# Optionally initialize the database and enable automatic start:
/usr/pgsql-12/bin/postgresql-12-setup initdb
systemctl enable postgresql-12
systemctl start postgresql-12
```
