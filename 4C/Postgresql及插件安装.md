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

>pg源码安装，参照[网址](http://www.postgres.cn/v2/download)
```sh?linenums
wget https://ftp.postgresql.org/pub/source/v12.2/postgresql-12.2.tar.bz2
tar xjvf postgresql*.bz2 #解压至一个目录
cd potgresql-12.2
./configure --prefix=/opt/pgsql #拟安装至/opt/pgsql
make world
make install-world
adduser postgres #增加新用户，系统提示要给定新用户密码
mkdir /opt/pgsql/data #创建数据库目录
chown -R postgres:postgres /opt/pgsql/data
su - postgres #使用postgres帐号操作
/opt/pgsql/bin/initdb -D /opt/pgsql/data #初始化数据库
/opt/pgsql/bin/pg_ctl -D /opt/gsql/data -l logfile start #启动数据库
/opt/pgsql/bin/createdb genericdb #假定数据库名为gerericdb)
/opt/pgsql/bin/psql genericdb # (进入数据库内部)
```

> postgis插件安装，参照[网址](https://blog.csdn.net/ceshell/article/details/103749632)
> 下载 [geos](http://download.osgeo.org/geos/)、[gdal](http://download.osgeo.org/gdal/2.4.1/)、[proj](http://download.osgeo.org/proj/)、[libiconv](https://ftp.gnu.org/pub/gnu/libiconv/)、[libxml2](http://xmlsoft.org/sources/ ftp://xmlsoft.org/libxml2)、[json-c](https://github.com/json-c/json-c/releases)
> 下载后上传至服务器
> 编译上述依赖
```sh?linenums
# compile  gdal
tar -zxvf gdal-2.4.1.tar.gz
cd gdal-2.4.1
./configure --prefix=/opt/gdal-2.4.1
make
make install
# compile geos
tar -jxvf geos-3.7.1.tar.bz2
cd geos-3.7.1
./configure --prefix=/opt/geos-3.7.1
make
make install

#compile proj
tar -zxvf proj-6.2.0.tar.gz
cd proj-6.2.0/
./configure --prefix=/opt/proj-6.2.0
make
make install

#compile libiconv
tar -zxvf libiconv-1.16.tar.gz
cd libiconv-1.16
./configure --prefix=/opt/libiconv-1.16
make
make install

#compile libxml2
tar -zxvf libxml2-2.9.1.tar.gz
cd libxml2-2.9.1
./configure --prefix=/opt/libxml2-2.9.1
make
make install

#compile json-c
tar -zxvf json-c-json-c-0.13.1-20180305.tar.gz
cd json-c-json-c-0.13.1-20180305
./configure --prefix=/opt/json-c
make
make install

```

>添加Id信息
>在/etc/ld.so.conf.d/下新建gdal-2.4.1.conf文件并添加安装路径/opt/gdal-2.4.1/lib。
>要为上面每个依赖库新建一个 .conf 文件并在文件内添加 installed_path/lib
>然后执行ldconfig
>![add idconfig](http://image.kerbores.com/xsj/1590545059703.png)
>编译[PostGIS](http://www.postgis.net/stuff/`)
```sh?linenums
#根据各依赖包安装路径，在命令中使用绝对路径，防止编译错误。
tar -zxvf postgis-2.5.2.tar.gz
cd postgis-2.5.2
./configure --with-pgconfig=/home/work/postgresql/bin/pg_config --with-geosconfig=/opt/geos-3.7.1/bin/geos-config --with-gdalconfig=/opt/gdal-2.4.1/bin/gdal-config --with-xml2config=/opt/libxml2-2.9.1/bin/xml2-config --with-projdir=/opt/proj-6.2.0 --with-jsondir=/opt/json-c --with-libiconv=/opt/libiconv-1.16
make
make install

```
>补充安装数据库的扩展模块
```sh?linenums
# 数据库编译安装时，该扩展不会自动安装，需要自行编译安装。
cd /home/work/install_package/PostgreSQL/postgresql-11.0/contrib/fuzzystrmatch
make
make install

```
>测试
```sh?linenums
# 进入数据库终端，测试空间数据库是否能够创建成功。
su - postgres
psql
create database gistest;  # 创建普通数据库
\c gistest  # 切换到该数据库
\dx  # 显示扩展模块
# 为普通数据库增加空间扩展

CREATE EXTENSION postgis;
CREATE EXTENSION fuzzystrmatch;
CREATE EXTENSION postgis_tiger_geocoder;
CREATE EXTENSION address_standardizer;
CREATE EXTENSION postgis_topology;

```