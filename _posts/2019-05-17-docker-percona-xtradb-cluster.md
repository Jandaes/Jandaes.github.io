---
layout: post
title:  Docker搭建MySQL集群
categories: mysql
tags: docker mysql
description: Docker搭建MySQL集群,percona-xtradb-cluster
typora-root-url: ..
---

* content
{:toc}


## 目标
搭建一个5个数据库的集群、数据实时同步

## 1. 安装镜像

### 1.1 安装地址
	https://hub.docker.com/r/percona/percona-xtradb-cluster
	

### 1.2 Docker 拉取镜像
	[root@localhost ~]# docker pull percona/percona-xtradb-cluster

## 2.创建Docker 网络
### 2.1 Docker网段命令
	
	#创建网段、取名为：net1
	[root@localhost ~]# docker network create net1
	#查看指定网段详细信息
	[root@localhost ~]# docker network inspect net1
	#删除指定网段
	[root@localhost ~]# docker network rm net1


### 2.2 创建网段
	[root@localhost ~]# docker network create --subnet=172.18.0.0/24 net1


<!--more-->

### 2.3 查询网段信息
```java
[root@localhost ~]# docker network inspect net1
[
    {
        "Name": "net1",
        "Id": "0e197c8d61e37aad6e52188b2a185de7a723087fd88e57f36a2ac98443e2823a",
        "Created": "2019-05-16T15:25:46.447740525+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/24"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
```
## 3. 创建Volume
### 3.1 Docke Volume卷命令
```java
#创建网段、取名为：v1
[root@localhost ~]# docker volume create v1
#查看指定卷详细信息
[root@localhost ~]# docker volume inspect v1
#删除指定卷
[root@localhost ~]# docker volume rm v1
#查看卷信息
[root@localhost ~]# docker volume ls
```
### 3.2 创建卷
因为要搭建5个数据库的集群、所以创建5个卷
```java
[root@localhost ~]# docker volume create v1
[root@localhost ~]# docker volume create v2
[root@localhost ~]# docker volume create v3
[root@localhost ~]# docker volume create v4
[root@localhost ~]# docker volume create v5
```
### 3.3 查看卷
```java
[root@localhost ~]# docker volume inspect v1
[
    {
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/v1/_data",
        "Name": "v1",
        "Options": {},
        "Scope": "local"
    }
]
```
## 4. 创建MySQL PXC容器
### 4.1 创建Master数据库
mysql的默认帐号是: `root`
```java
[root@localhost images]# docker run -d -p 3306:3306 
-v v1:/var/lib/mysql 
-e MYSQL_ROOT_PASSWORD=root123456 
-e CLUSTER_NAME=PXC 
-e XTRABACKUP_PASSWORD=root123456 
--privileged 
--name=my1 
--net=net1 
--ip 172.18.0.2
percona-xtradb-cluster:latest
```
参数说明：
​	-d：后端启动容器
​	-p:宿主机端口:容器端口，把容器中的端口映射到宿主机的端口上，访问宿主机端口就是直接访问容器
​	-v:卷:容器目录，把卷挂载在容器里面
​	-e:启动参数
​		MYSQL_ROOT_PASSWORD:mysql的密码(自定义)
​		CLUSTER_NAME:集群名字(自定义)
​		XTRABACKUP_PASSWORD:数据库节点间数据同步的密码(自定义)
​	--privileged:授予权限
​	--name:容器命名
​	--net:绑定网段
​	--ip:绑定ip,ip地址为绑定网段内ip
​	percona-xtradb-cluster:latest:运行的镜像

### 4.2 创建备库
#### 说明
加四个从数据库

**特别注意：创建备库的时候、必须要等主库可以成功连接上了、才可以创建、不然会出现容器闪退的问题。原因是容器启动快、但是容器里服务启动慢。**

#### 注意事项
​	1. 修改挂载的卷     
​	2. 修改宿主机端口，端口唯一    
​	3. 添加指定的群名,-e CLUSTER_JOIN=my1    
​	4. 修改容器名    
​	5. 修改ip    
​	6. 必须等第一个容器服务创建成功才能创建第二个、通过客户端工具验证，不然会出现闪退情况

my2: 

```java
[root@localhost images]# docker run -d -p 3307:3306 
-v v2:/var/lib/mysql 
-e MYSQL_ROOT_PASSWORD=root123456 
-e CLUSTER_NAME=PXC 
-e XTRABACKUP_PASSWORD=root123456 
-e CLUSTER_JOIN=my1 
--privileged 
--name=my2
--net=net1 
--ip 172.18.0.3
percona-xtradb-cluster:latest
```
参数说明：
​	CLUSTER_JOIN：需要加入到主集群中、主mysql名字,跟主数据数据同步

my3:
```java
[root@localhost images]# docker run -d -p 3307:3306 
-v v3:/var/lib/mysql 
-e MYSQL_ROOT_PASSWORD=root123456 
-e CLUSTER_NAME=PXC 
-e XTRABACKUP_PASSWORD=root123456 
-e CLUSTER_JOIN=my1 
--privileged 
--name=my3
--net=net1 
--ip 172.18.0.4
percona-xtradb-cluster:latest
```
my4:
```java
[root@localhost images]# docker run -d -p 3307:3306 
-v v4:/var/lib/mysql 
-e MYSQL_ROOT_PASSWORD=root123456 
-e CLUSTER_NAME=PXC 
-e XTRABACKUP_PASSWORD=root123456 
-e CLUSTER_JOIN=my1 
--privileged 
--name=my4
--net=net1 
--ip 172.18.0.5
percona-xtradb-cluster:latest
```
my5:
```java
[root@localhost images]# docker run -d -p 3307:3306 
-v v5:/var/lib/mysql 
-e MYSQL_ROOT_PASSWORD=root123456 
-e CLUSTER_NAME=PXC 
-e XTRABACKUP_PASSWORD=root123456 
-e CLUSTER_JOIN=my1 
--privileged 
--name=my5
--net=net1 
--ip 172.18.0.6
percona-xtradb-cluster:latest
```
### 连接效果

**注意事项：连接的是宿主机的ip和port、并不是容器内部地址**

![测试连接](/images/2019-05-17/db.png)

### 数据同步测试
1. 打开db1数据库、创建一个schemas(数据库)名为：blank
2. 查看db2、db3、db4、db5 数据库是否有自动创建blank数据库
3. 在该数据库(db1)创建一个表名：student、并添加数据
4. 在任意数据库查看数据是否同步过来

  ![创建数据库](/images/2019-05-17/db1.png)    
  创建数据库

  ![添加一条数据](/images/2019-05-17/db2.png)    
  添加一条数据

  ![查看其它数据库同步](/images/2019-05-17/db3.png)    
  查看其它数据库同步

## 总结
至此、数据库集群已经处理好了，数据同步也已经可以了！
项目只要连接任意一个数据库就可以同步了！
缺点：
​	单节点处理所有请求、负载高、性能差

![请求不能分发](/images/2019-05-17/tb.png)    
请求不能分发

> 解决方法：
 使用Haproxy做负载均衡、请求被均匀分发给节点，单点负载低，性能好    
 

 ![请求不能分发](/images/2019-05-17/lb.png)
 
 
 
-- - -
沏与你的这碗茶尚温热，一点十都如本心，九分人间烟火。

-- 《好吗好的》