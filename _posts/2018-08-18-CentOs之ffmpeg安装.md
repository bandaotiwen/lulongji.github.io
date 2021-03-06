---
layout:     post
title:      ffmpeg
subtitle:   ffmpeg安装
date:       2018-08-18
author:     lulongji
header-img: img/post-bg-hacker.jpg
catalog: true
tags:
   - linux
   - ffmpeg
---

# 介绍

由于公司需要做微信和坐席IM通话处理语音，所以需要在centos服务器上安装ffmpeg，用来处理音频。

# 下载
ffmpeg下载链接：[http://ffmpeg.org/download.html](http://ffmpeg.org/download.html)

lame下载接：[https://sourceforge.net/projects/lame/files/lame/](https://sourceforge.net/projects/lame/files/lame/)


# 安装需求包

### 安装lame
    tar -zxf lame-3.100.tar.gz 
    cd lame-3.100 
    ./configure 
    make && make install
### 安装yasm

    yum install yasm

# 编译ffmpeg

    ./configure --enable-shared --enable-libmp3lame  --prefix=/usr/local/ffmpeg

```注意：时间比较长，需要等待几分钟```

# 安装

    make && make install

```注意：时间比较长。```

# 说明

安装完成后在/usr/local/ffmpeg下出现四个目录：

- bin：可执行文件目录
- lib：动态链接库目录
- include：编程用到的头文件目录
- share

不管是编程还是可执行程序的执行都需要依赖lib下面的动态库，有两个方法可以做到这一点：

1. 直接将lib文件夹下的所有.so文件拷贝到/usr/lib下。
2. 通过如下方式修改配置文件：

查看/etc/ld.so.conf文件，发现里面只有一句话：

    include ld.so.conf.d/*.conf

表明其包含了```ld.so.conf.d```下所有的conf文件，于是可以在```/etc/ld.so.conf.d/```创建一个新的文件```ffmpeg.conf```，其中只包含一句话，即为ffmpeg的lib目录：

    /usr/local/ffmpeg/lib

再运行：

     ldconfig


更新```ld.so.cache```，使修改生效。

为了在任何地方能够直接用ffmpeg运行，而不用使用如./ffmpeg或者 /usr/local/ffmpeg/bin/ffmpeg的方式运行程序，可以把可执行程序复制到bin目录下：

     cp /usr/local/ffmpeg/bin/ffmpeg /usr/local/bin/ 
     cp /usr/local/ffmpeg/bin/ffprobe /usr/local/bin/ 
     cp /usr/local/ffmpeg/bin/ffserver /usr/local/bin/


或者也可以用创建链接的方式：

     ln -s /usr/local/ffmpeg/bin/ffmpeg /usr/local/bin/ 
     ln -s /usr/local/ffmpeg/bin/ffprobe /usr/local/bin/ 
     ln -s /usr/local/ffmpeg/bin/ffserver /usr/local/bin/




# shell

如果你特别懒嫌麻烦的话，那么直接copy以下脚本直接执行就可以了。


        #!/bin/sh
        #Based on instructions found here: http://wiki.razuna.com/display/ecp/FFMpeg+Installation+on+CentOS+and+RedHat#FFMpegInstallationonCentOSandRedHat-InstallX264

        if [ "`/usr/bin/whoami`" != "root" ]; then
        echo "You need to execute this script as root."
        exit 1
        fi

        yum -y update

        yum -y install glibc gcc gcc-c++ autoconf automake libtool git make nasm pkgconfig
        yum -y install SDL-devel a52dec a52dec-devel alsa-lib-devel faac faac-devel faad2 faad2-devel
        yum -y install freetype-devel giflib gsm gsm-devel imlib2 imlib2-devel lame lame-devel libICE-devel libSM-devel libX11-devel
        yum -y install libXau-devel libXdmcp-devel libXext-devel libXrandr-devel libXrender-devel libXt-devel
        yum -y install libogg libvorbis vorbis-tools mesa-libGL-devel mesa-libGLU-devel xorg-x11-proto-devel zlib-devel
        yum -y install libtheora theora-tools
        yum -y install ncurses-devel
        yum -y install libdc1394 libdc1394-devel
        yum -y install amrnb-devel amrwb-devel opencore-amr-devel

        cd /opt
        wget https://ffmpeg.org/releases/ffmpeg-4.1.tar.bz2


        ###安装lame####
        wget https://sourceforge.net/projects/lame/files/lame/3.100/lame-3.100.tar.gz
        tar -zxf lame-3.100.tar.gz
        cd lame-3.100
        ./configure && make && make install

        ### 安装yasm
        yum -y install yasm

        ###安装ffmpeg###
        cd /opt  && tar -xvf ffmpeg-4.1.tar.bz2 && cd ffmpeg-4.1
        ./configure --enable-shared --enable-libmp3lame  --prefix=/usr/local/ffmpeg && make && make install

        ##环境配置##
        echo "/usr/local/ffmpeg/lib" > /etc/ld.so.conf.d/ffmpeg.conf

        ldconfig

        cp /usr/local/ffmpeg/bin/ffmpeg /usr/local/bin/
        cp /usr/local/ffmpeg/bin/ffprobe /usr/local/bin/




