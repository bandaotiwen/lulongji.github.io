---
layout:     post
title:      CentOs7 [Update the yum source]
subtitle:   更新yum源
date:       2015-12-25
author:     lulongji
header-img: img/post-bg-hacker.jpg
catalog: true
tags:
    - centos
    - yum
---


# 说明
Centos7更新yum源。

# 首先备份yum
- /ect/yum.repos.d/CentOS-Base.repo
- mv  /etc/yum.repos.d/CentOS-Base.repo  /ect/yum.repos.d/CentOS-Base.repo.backup

# 替换yum源为阿里源

    wget -O  /etc/yum.repos.d/CentOS-Base.repo  http://mirrors.aliyun.com/repo/Centos-7.repo

# 生成缓存
    yum makecache

# 更新系统
    yum -y update
 

# 如果yum正在运行，要先关掉进程   

    rm -f  /var/run/yum.pid


# 如果yum install没有可用软件包

    yum install epel-release;


# 说明

本文只做学习参考，如有任何不准确的地方欢迎指正。

我的邮箱：
- ```lulongji2011@163.com```








 

