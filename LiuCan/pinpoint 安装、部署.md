---
title: pinpoint 安装、部署 
tags: pinpoint,运维,技术
renderNumberedHeading: true
grammar_cjkRuby: true
---

 - 软件准备：
	jdk-8u111-linux-x64.tar.gz #jdk
	
	apache-tomcat-8.0.36.tar.gz #tomcat
	
	pinpoint-collector-1.8.4.war #收集器

	pinpoint-web-1.8.4.war #界面展现

	pinpoint-agent-1.8.4.tar.gz #探针

	hbase-create.hbase #表创建脚本

	hbase-1.0.3-bin.tar.gz #hbase数据库
	

 - 环境安装
	   1.安装jdk
		   略~
	  2.安装hbase
	 ``` sh?linenums
		#进入/usr/local目录
		 cd /usr/local 
		 #解压安装文件
		 tar -zxvf hbase-1.2.6.1-bin.tar.gz
		 #启动hbase
		 ./hbase-1.2.6.1/bin/start-hbase.sh
		 #创建pinpoint表
		 ./hbase-1.2.6.1/bin/hbase shell hbase-create.hbase
		 #脚本执行完成后可查看导入的表：ip:16010/master-status
	```
    3.安装tomcat、pinpoint-collector
    ```sh?linenums
		#进入安装目录
		cd /usr/local/
		#解压tomcat
		tar -zxvf apache-tomcat-9.0.24.tar.gz
		#重命名目录
		mv apache-tomcat-9.0.24/ apache-tomcat-1080/
		cd apache-tomcat-1080/webapps/
		#删除所有默认应用
		rm -rf * 
		#解压pinpoint-collector到/webapps/ROOT目录
		unzip /var/ftp/pub/pinpoint-collector-1.8.4.war -d ROOT 
		#修改/conf/server.xml文件，将8005、8080、8009端口分别改为1005、1080、1009
		cd /usr/local/apache-tomcat-1080/bin
		./startup.sh #启动tomcat容器
	```
    4.安装pinpoint-web
    ```sh?linenums
	#进入安装目录
	cd /usr/local/ 
	#解压tomcat
    tar -zxvf apache-tomcat-9.0.24.tar.gz 
	#重命名目录
    mv apache-tomcat-9.0.24/ apache-tomcat-2080/ 
    cd apache-tomcat-2080/webapps/
	#删除所有默认应用
    rm -rf * 
	#解压pinpoint-web到/webapps/ROOT目录
    unzip /var/ftp/pub/pinpoint-web-1.8.4.war -d ROOT 
	#修改/conf/server.xml文件，将8005、8080、8009端口分别改为2005、2080、2009
	cd /usr/local/apache-tomcat-2080/bin
	#启动tomcat容器
	./startup.sh 
	```
    5.安装pinpoint-web
    ```sh?linenums
	#进入安装目录
	cd /usr/local/ 
	#解压tomcat
    tar -zxvf apache-tomcat-9.0.24.tar.gz 
	#重命名目录
	mv apache-tomcat-9.0.24/ apache-tomcat-2080/ 
	cd apache-tomcat-2080/webapps/
	#删除所有默认应用
	rm -rf * 
	#解压pinpoint-web到/webapps/ROOT目录
	unzip pinpoint-web-1.8.4.war -d ROOT 
	#修改/conf/server.xml文件，将8005、8080、8009端口分别改为2005、2080、2009
	cd /usr/local/apache-tomcat-2080/bin
	#启动tomcat容器
	./startup.sh 
	```
    6.部署pinpoint-agent
    先解压pinpoint-agent到任意目录，本文解压到/home/agent目录
    ```sh?linenums
	cd /home #进入安装目录
	mkdir agent #创建文件夹
	cd agent #进入安装文件夹
	tar -zxvf pinpoint-agent-1.8.4.tar.gz #解压pinpoint-agent文件
	```
	6.1. pinpoint-agent配置和参数
	```sh?linenums
	pinpoint-agent的配置文件为/pinpoint.config，除profiler.collector.ip参数，其他参数可保持不变。profiler.collector.ip=127.0.0.1 #后面的ip地址为pinpoint-collector安装地址
	```
	6.2. 监控的项目启动方式
	```sh?linenums
	#Linux 环境
	在$TOMCAT_HOME/bin/目录新增setenv.sh文件（注意.sh文件头以“#!/bin/sh”为第一行），添加配置：          #!/bin/sh
	CATALINA_OPTS="$CATALINA_OPTS -javaagent:/home/agent/pinpoint-bootstrap-1.8.4.jar -Dpinpoint.agentId=test-01 -Dpinpoint.applicationName=test"
	
	#Windows 环境
	在$TOMCAT_HOME/bin/目录新增setenv.bat文件，添加配置：
	set CATALINA_OPTS=%CATALINA_OPTS% -javaagent:E:/agent/pinpoint-bootstrap-1.8.4.jar -Dpinpoint.agentId=test-01 -Dpinpoint.applicationName=test 
	
	#springboot环境配置
	只需在java命令后面加上-javaagent:/home/agent/pinpoint-bootstrap-1.8.4.jar -Dpinpoint.agentId=xxx -Dpinpoint.applicationName=xxx参数，如：java -javaagent:/home/agent/pinpoint-bootstrap-1.8.4.jar -Dpinpoint.agentId=test-01 -Dpinpoint.applicationName=test -jar test.jar
	#访问pinpoint-web
	 打开安装地址：http://ip:2080 可查看pinpoint收集情况
	
	```
