---
title: Jenkins+Gitea 安装、部署
tags: Jenkins,Gitea ,运维,自动发布
renderNumberedHeading: true
grammar_cjkRuby: true
---
### 环境及版本
	1.Centos v7.x 及以上
	2.Jenkins v2.555 及以上
	3.git v1.8.3.1 及以上
	4.gitea v1.13.0 及以上
	5.open-jdk v1.8 及以上
	6.node v10.15.0 及以上
	7.maven v3.5.2 及以上
	
### 安装

#### 安装Jenkins
- 安装相关源
  
``` sh?linenums
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
yum install java  -y //这里我安装的java-1.8.0
yum install jenkins -y 
###############################
# jenkins 相关目录
###############################
[root@jenkins ~]# rpm -ql jenkins
/etc/init.d/jenkins
/etc/logrotate.d/jenkins
/etc/sysconfig/jenkins
/usr/lib/jenkins
/usr/lib/jenkins/jenkins.war
/usr/sbin/rcjenkins
/var/cache/jenkins
/var/lib/jenkins
/var/log/jenkins
```

- 启动服务
  ``` sh?|linenums
  systemctl start jenkins
  ```
- 使用浏览器打开安装页面 (http://ip:8080)
  
  初始化操作，将/var/lib/jenkins/secrets/initialAdminPassword文件里面保存的密码，输入进去。
  ![初始化操作](./images/1610003559300.png)
  安装插件，这里可以选择推荐插件，也可以选择自定选择插件，效果如下图。
  ![安装插件1](./images/1610003721043.png)
  ![安装插件2](./images/1610003768351.png)
  创建用户，我们选择不去创建，使用默认的admin用户，和初始密码。就行。所以我们选择continue as admin。
  ![enter description here](./images/1610003928796.png)
- 登录Jenkins
  在浏览器中输入：(http://ip:8080),填写admin用户和初始化密码就登陆进去了。
  jenkins 主界面
  ![enter description here](./images/1610004034070.png)
- 安装插件（git、node、maven等）
  点进去之后，点击Jenkins右侧GlobalToolConfiguration后，进去该页面进行勾选、安装。
 ![enter description here](./images/1610004377152.png)
 
 #### 安装open-jdk (略)
 
 #### 安装gitea
 
