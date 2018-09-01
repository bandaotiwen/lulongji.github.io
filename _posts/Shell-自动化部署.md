---
layout:     post
title:      自动化部署
subtitle:  	自动化部署
date:       2017-10-30
author:     lulongji
header-img: img/post-bg-hacker.jpg
catalog: true
tags:
    - linux
    - shell
---

# 介绍
公司最近新弄了一台测试服务器，以前的测试服务器是和别的部门公用的，所以想自己搭建一台测试服务器。环境 centOs7。

# 开始
以下写了两个简单的部署脚本。

### 同步脚本
	set -m
	rm -rf /root/.m2/repository/com/haoyisheng/*
	rm -rf /root/.m2/repository/com/haoys/*

	#update
	cd /digital/website  && svn up
	sleep 0.5
	#mvn
	cd /digital/website/digital-medical && mvn clean install
	cd /digital/website/medical-cms && mvn clean install
	cd /digital/website/hys-cms && mvn clean install
	cd /digital/website/hys-website && mvn clean install
	cd /digital/website/hys-web && mvn clean install
	#scp instruments
	#web
	scp -r  /digital/website/digital-medical/medical-web/target/medical-web.war   47.95.122.97:/opt/projects/243/instruments
	#pic
	scp -r /digital/website/medical-cms/medicalcms-pic/target/medicalcms-pic.war  47.95.122.97:/opt/projects/243/instruments
	#cms
	scp -r /digital/website/medical-cms/medicalcms-web/target/medicalcms-web.war  47.95.122.97:/opt/projects/243/instruments
	# scp website
	#web
	scp -r /digital/website/hys-website/target/hys-website.war  47.95.122.97:/opt/projects/243/website
	#cms
	scp -r /digital/website/hys-cms/target/hys-cms.war  47.95.122.97:/opt/projects/243/website

	#scp others
	#cms
	scp -r /digital/website/hys-web/web-site/cms/target/cms.war  47.95.122.97:/opt/projects/243/others
	#web
	scp -r /digital/website/hys-web/web-site/web/target/web.war  47.95.122.97:/opt/projects/243/others
	#end
	echo "同步结束！"


### 部署脚本
	set -m
	rm -rf /root/.m2/repository/com/haoyisheng/*
	rm -rf /root/.m2/repository/com/haoys/*
	echo "--从svn更新website--"
	cd /opt/haoys-website  && svn up
	echo "--打包--"
	cd /opt/haoys-website  && mvn clean install -U -Dmaven.test.skip=true
	echo "--停止website项目进程--"
	pid=`netstat -nltp | grep 8080 | awk {'print $7'} | awk -F "/" {'print $1'}`
	sleep 0.5
	if [ -n "$pid" ];then
			echo "kill jboss instance of pid: $pid"
			`kill -9 $pid`
	fi
	cd  /webserver/website.war/ && find . -maxdepth 1 ! -name upload -exec rm -rf {} \;
	echo "--部署website项目--"
	cp -r /opt/haoys-website/website-web-admin/target/website-web-admin.war /webserver/website.war/
	rm -rf /usr/local/jboss/standalone/deployments/website-web.war   &&   cp -r /opt/haoys-website/website-web/target/website-web.war /usr/local/jboss/standalone/deployments/
	cd  /webserver/website.war/  && jar -xvf website-web-admin.war  && rm -rf website-web-admin.war
	echo "--启动website工程--"
	sleep 0.5
	nohup /usr/local/jboss/bin/standalone.sh > output 2>&1 &


	