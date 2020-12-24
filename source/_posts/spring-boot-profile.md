---
title: spring boot profile
author: yuu
avatar: https://cdn.jsdelivr.net/gh/yuu2b/cdn/img/custom/avatar.jpg
authorLink: nzynl.top
categories: 转载
comments: true
date: 2020-12-19 19:23:40
authorAbout: 有所作为是生活的最高境界。
authorDesc: 学习学习
tags: spring-boot
keywords: profile
description: 'spring boot profile配置和启动时no active profile set, falling back to default profiles: default的问题'
photos: https://cdn.jsdelivr.net/gh/yuu2b/cdn/img/blog/spring-boot-profile/avatar.png
---

### 原文:
> https://blog.csdn.net/benbenniaono1/article/details/105632264

### 1.启动时no active profile set, falling back to default profiles: default

> 相信有不少细心小伙伴在新启动一个spring boot项目时候会发现这样一条信息no active profile set, falling back to default profiles: default。为什么会出现这样的提示呢，我们先来看看profile，profile是Spring对不同环境提供不同配置功能的支持，可以通过激活、 指定参数等方式快速切换环境Profile。举个例子我们在开发，测试，生产用到的配置是不同的，那么通过profile就可以帮我们指定对应的环境用对应的配置，那么如何来指定呢？

```java
2020-04-20 12:20:17.922  INFO 7824 --- [  restartedMain] c.e.s.SpringbootcacheApplication         : No active profile set, falling back to default profiles: default
```

### 2.多profile文件形式：

> – 格式：application-{profile}.properties/yml： application-dev.properties、application-prod.properties .我们可以通过application-{profile}.properties/yml的方式指定并激活不同的profile配置。

### 3.多profile配置激活

> 先给出我们需要配置的文件目录，这里举例是yml文件，如下图所示。然后具体有以下5种方法可以来指定并激活我们需要的配置。

![img](https://img-blog.csdnimg.cn/20200420124305267.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JlbmJlbm5pYW9ubzE=,size_16,color_FFFFFF,t_70)

### 3.1通过properties文件指定

> 很简单，比如我们要使用application-dev.yml的配置，那么在application.properties中写上spring.profiles.active=dev,那么我们的application-dev.yml的配置就被激活了，那么如何测试结果呢，很简单，我们在application-dev.yml指定服务启动端口，启动spring boot 服务，看看是不是8081端口就好了。

```java
server:
  port: 8081
```

>  可以看到The following profiles are active: dev，然后启动的端口是8081说明我们想要的配置生效了，指定其他配置比如test,只要修改spring.profiles.active=test。

```java
2020-04-20 13:02:05.144  INFO 19832 --- [  restartedMain] c.e.s.SpringbootcacheApplication         : The following profiles are active: dev
2020-04-20 13:02:05.785  INFO 19832 --- [  restartedMain] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8081 (http)
```

### 3.2通过yml文档块指定

> 在yml配置文件中通过---的形式来划分文档块，通过图中active: xx (注意中间有个空格，这是yml的基本格式)指定的值来激活相应的配置文件。
> 比如active: dev，那么application-dev.yml这个配置文件被激活。

![img](https://img-blog.csdnimg.cn/2020042013092429.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JlbmJlbm5pYW9ubzE=,size_16,color_FFFFFF,t_70)

### 3.3通过configuration中的Program agruments指定

> 在configuration中的program arguments 中写入--spring.profiles.active=dev。

![img](https://img-blog.csdnimg.cn/20200420131426222.png)

![img](https://img-blog.csdnimg.cn/20200420131513759.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JlbmJlbm5pYW9ubzE=,size_16,color_FFFFFF,t_70)

### 3.4通过configuration中的VM options指定

> 在configuration中的VM options配置项中填写：-Dspring.profiles.active=dev.

![img](https://img-blog.csdnimg.cn/20200420140416209.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JlbmJlbm5pYW9ubzE=,size_16,color_FFFFFF,t_70)

### 3.5通过configuration中的Enviroment variables指定

> 在configuration中的Enviroment variables 配置项上写入：spring.profiles.active=dev

![img](https://img-blog.csdnimg.cn/20200420140718975.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JlbmJlbm5pYW9ubzE=,size_16,color_FFFFFF,t_70)

### 4.配置优先顺序

> program arguments > VM options > Enviroment variables >proties 文件> yml文件