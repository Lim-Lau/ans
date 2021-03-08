---
title: Linux java 源码安装 
tags: java,linux,源码
renderNumberedHeading: true
grammar_cjkRuby: true
---


# 卸载Linux自带java
```sh?linenums
#查看自带Java版本
java -version
#openjdk version "1.8.0_161"
#OpenJDK Runtime Environment (build 1.8.0_161-b14)
#OpenJDK 64-Bit Server VM (build 25.161-b14, mixed mode)

#查询OpenJDK 
rpm -qa|grep java
# java-1.8.0-openjdk-headless-1.8.0.161-2.b14.el7.x86_64
# javapackages-tools-3.4.1-11.el7.noarch
# java-1.7.0-openjdk-1.7.0.171-2.6.13.2.el7.x86_64
# java-1.8.0-openjdk-1.8.0.161-2.b14.el7.x86_64
# java-1.7.0-openjdk-headless-1.7.0.171-2.6.13.2.el7.x86_64
# tzdata-java-2018c-1.el7.noarch

#删除openJDK版本
 rpm -e --nodeps java-1.7.0-openjdk-1.7.0.171-2.6.13.2.el7.x86_64
 rpm -e --nodeps java-1.8.0-openjdk-1.8.0.161-2.b14.el7.x86_64
 rpm -e --nodeps java-1.8.0-openjdk-headless-1.8.0.161-2.b14.el7.x86_64
 rpm -e --nodeps java-1.7.0-openjdk-headless-1.7.0.171-2.6.13.2.el7.x86_64

#再次查询确保删除完毕
rpm -qa|grep java
#python-javapackages-3.4.1-11.el7.noarch
#javapackages-tools-3.4.1-11.el7.noarch
#tzdata-java-2018c-1.el7.noarch

```
# 下载JDK
```sh?linenums

#查看Linux版本位数
getconf LONG_BIT
# 64
#下载JDK jdk-8u221-linux-x64.tar.gz 
wget https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
#https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
```