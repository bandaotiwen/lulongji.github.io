---
layout:     post
title:      Jersey
subtitle:   Jersey frame
date:       2018-08-06
author:     lulongji
header-img: img/post-bg-hacker.jpg
catalog: true
tags:
    - Java
    - Jersey
---

# 介绍
由于目前接手的项目中有很多不同框架组合起来项目，Jersey框架就是其中之一，所以总结一下。在网上查到，Jersey 这是一个非常优秀的框架：

- 实现了RESTful的webservice框架。
- 属于glassfish项目。
- [官网地址：https://www.jersey.com](https://www.jersey.com)
- Jersey目前的中文文档比较少

# 使用和区别

目前项目中用到的是```com.sun.jersey```  而不是```org.glassfish.jersey```，Jersey在Project使用时有两种jar实现，一种是使用sun的jar，一种是使用glassfish的jar。

### 使用

#### glassfish -> jersey

参考：[glassfish项目：https://www.jianshu.com/p/15c32cb52da1](https://www.jianshu.com/p/15c32cb52da1)

#### sun -> jersey
参考：[eclipse + maven + com.sun.jersey项目：https://www.cnblogs.com/grissom007/p/6895820.html](https://www.cnblogs.com/grissom007/p/6895820.html)


### 区别

1.web.xml加载jersey的servlet容器
- jersey1.X使用的是sun的com.sun.jersey.spi.container.servlet.ServletContainer
- jersey2.X使用的是glassfish的org.glassfish.jersey.servlet.ServletContainer

2.扫描jersey resource
- jersey1.X使用的是sun的com.sun.jersey.config.property.packages
- jersey2.X使用的是glassfish的jersey.config.server.provider.packages

3.jersey2.X可以使用servlet3的 @WebServlet扫描jersey resource。不需要特别配置web.xml

4.jersey2.X可以使用@ApplicationPath注解，加载jersey resouce。

5.jersey2.X可以使用web.xml加载Application


# 注解

[常用注解说明：http://blog.lulongji.cn/2018/08/08/注解系列二/](http://blog.lulongji.cn/2018/08/08/注解系列二/)


