---
layout: post
title:  ActiveMq 安装
categories: 消息队列
tags: activemq
description: activemq 安装
typora-root-url: ..
---

* content
{:toc}


### 一、消息队列概述
消息队列中间件是分布式系统中重要的组件，主要解决应用解耦，异步消息，流量削锋等问题，实现高性能，高可用，可伸缩和最终一致性架构。目前使用较多的消息队列有ActiveMQ，RabbitMQ，ZeroMQ，Kafka，MetaMQ，RocketMQ

### 二、消息队列应用场景

异步处理，应用解耦，流量削锋和消息通讯四个场景。

<!--more-->

### 三、安装方式
#### 下载地址
下载地址：[http://activemq.apache.org/activemq-5158-release.html](http://activemq.apache.org/activemq-5158-release.html)
![activemq-download](/images/2018-11-29/activemq-download.png)

#### Win 安装方式
##### 前台运行
1. 进入`apache-activemq-5.15.8/bin/`
2. 不同操作系统进入不同目录
3. 右键执行`activemq.bat`
##### 后台运行(服务运行)
1. 右键执行`InstallService.bat`
2. doc进入服务列表`services.msc` 
3. 启动`activemq-server`服务
#### Linux 安装方式
1. 下载`apache-activemq-5.15.8-bin.tar.gz`到linux、解压
2. 运行`activemq`
> 启动：./activemq start
> 停止：./activemq stop

#### Docker 运行
1. 搜寻`activemq`镜像、或者第三方镜像拉取
>[root@localhost activemq]# docker search activemq

![docker-search-activemq](/images/2018-11-29/docker-search-activemq.png)
2. 运行刚pull的镜像

> docker run -d --name activemq-master -p 61616:61616 -p 8161:8161 docker.io/webcenter/activemq:latest


> 容器名：activemq-master    
> 开放端口：61616、8161    
> 运行状态：后台运行

### 四、访问ActiveMq
#### 端口
```
8161: web 访问端口
61616: java tcp 访问端口
```
#### web访问

地址：[http://127.0.0.1:8161](http://127.0.0.1:8161)

![activemq-web](/images/2018-11-29/activemq-web.png)

点击`Manage ActiveMQ broker`登录
```
登录帐号：admin
登录密码：admin
```




-- - -
记忆、不管多么微不足道，都是定义我们的篇章。

-- 《当上帝是只兔子》