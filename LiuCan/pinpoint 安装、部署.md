---
title: pinpoint 安装、部署 
tags: pinpoint,运维,技术
renderNumberedHeading: true
grammar_cjkRuby: true
---

## 序章

 - 软件准备：
	jdk-8u111-linux-x64.tar.gz #jdk
	
	apache-tomcat-8.0.36.tar.gz #tomcat
	
	pinpoint-collector-1.8.4.war #收集器

	pinpoint-web-1.8.4.war #界面展现

	pinpoint-agent-1.8.4.tar.gz #探针

	hbase-create.hbase #表创建脚本

	hbase-1.0.3-bin.tar.gz #hbase数据库
	

 - 环境安装
	   安装jdk
		   略~
	  安装hbase
	  

``` sh?linenums
#进入/usr/local目录
 cd /usr/local 
 #解压安装文件
 tar -zxvf hbase-1.2.6.1-bin.tar.gz
```

 - 