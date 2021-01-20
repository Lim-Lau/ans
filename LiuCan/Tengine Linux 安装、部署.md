---
title: Tengine Linux 安装、部署 
tags: ops,nginx,linux
renderNumberedHeading: true
grammar_cjkRuby: true
---
---
Tengine 在Linux 系统上安装、部署。
Tengine v2.3.0
前置依赖：pcre-devel openssl openssl-devel 

---

## 安装前置依赖包

``` sh?linenums
yum -y install pcre-devel openssl openssl-devel
```
## 下载、安装Tengine

```sh?linenums
wget http://tengine.taobao.org/download/tengine-2.3.0.tar.gz

tar -zvxf tengine-2.3.0.tar.gz

cd tengine-2.3.0

./configure --prefix=/usr/local/nginx

make && make install

# 创建软链接
ln -s /usr/local/nginx/sbin/nginx /usr/local/bin/nginx

# 启动、重启
nginx 

nginx -s reload
```