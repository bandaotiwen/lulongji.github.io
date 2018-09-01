---
layout:     post
title:      CentOs之Redis搭建
subtitle:   Redis搭建
date:       2016-06-19
author:     lulongji
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - redis
    - linux
---

# 环境安装

### 配置编译环境：
    sudo yum install gcc-c++
    
### 下载源码：
    wget http://download.redis.io/releases/redis-3.2.8.tar.gz

### 解压源码：
    tar -zxvf redis-3.2.8.tar.gz

### 进入到解压目录：
    cd redis-3.2.8

### 执行make编译Redis：
    make MALLOC=libc

### 注意：make命令执行完成编译后，会在src目录下生成6个可执行文件，分别是
    redis-server、redis-cli、redis-benchmark、          
    redis-check-aof、redis-check-rdb、redis-sentinel。

### 安装Redis：
    make install 
    
### 配置Redis能随系统启动:
    ./utils/install_server.sh

### 显示结果信息如下：
    Welcome to the redis service installer
    This script will help you easily set up a running redis server

    Please select the redis port for this instance: [6379] 
    Selecting default: 6379
    Please select the redis config file name [/etc/redis/6379.conf] 
    Selected default - /etc/redis/6379.conf
    Please select the redis log file name [/var/log/redis_6379.log] 
    Selected default - /var/log/redis_6379.log
    Please select the data directory for this instance [/var/lib/redis/6379] 
    Selected default - /var/lib/redis/6379
    Please select the redis executable path [/usr/local/bin/redis-server] 
    Selected config:
    Port           : 6379
    Config file    : /etc/redis/6379.conf
    Log file       : /var/log/redis_6379.log
    Data dir       : /var/lib/redis/6379
    Executable     : /usr/local/bin/redis-server
    Cli Executable : /usr/local/bin/redis-cli
    Is this ok? Then press ENTER to go on or Ctrl-C to abort.
    Copied /tmp/6379.conf => /etc/init.d/redis_6379
    Installing service...
    Successfully added to chkconfig!
    Successfully added to runlevels 345!
    Starting Redis server...
    Installation successful!


### Redis服务查看、开启、关闭:
``` a.通过ps -ef|grep redis命令查看Redis进程```
```b.开启Redis服务操作通过/etc/init.d/redis_6379 start命令，也可通过（service redis_6379 start）```
```c.关闭Redis服务操作通过/etc/init.d/redis_6379 stop命令，也可通过（service redis_6379 stop）```

### 配置redis 远程连接 vim redis.conf
- 1.bind 127.0.0.1改为 #bind 127.0.0.1
- 2.protected-mode yes 改为 protected-mode no

### 去掉密码连接redis-cli
    config set requirepass 123456

### 如果远程链接还是不行，请查看防火墙端口

