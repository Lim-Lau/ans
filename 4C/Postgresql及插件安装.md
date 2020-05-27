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
>postgis插件安装参照[网址](https://www.cnblogs.com/sunnyeveryday/p/11442332.html)

```sh?linenums
yum search postgis
yum install -y postgis30_12.x86_64
// 开启插件  
# su postgres  
# psql  
// 开启pgsql的插件  
postgres=# create extension postgis;  
postgres=# create extension postgis_topology;  
postgres=# create extension fuzzystrmatch;  
postgres=# create extension address_standardizer;  
postgres=# create extension address_standardizer_data_us;  
postgres=# create extension postgis_tiger_geocoder; 
```
# 源码安装pg及插件
