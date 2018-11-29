---
layout: post
title:  Nginx 集成 Tomcat 动静分离
categories: 架构
tags: 服务器
description: nginx与tomcat动静分离
---

* content
{:toc}


### 说明
Nginx 是一个web服务器、用来做代理、转发，以及处理静态资源文件
Tomcat 是一个应用服务器、处理java开发文件

### 动态请求
动态请求、交给Tomcat处理:    
```
location / {
	proxy_next_upstream http_502 http_504 error timeout invalid_header;
	proxy_pass http://localhost:8080;
	# 真实的客户端IP
	proxy_set_header   X-Real-IP        $remote_addr;
	# 请求头中Host信息
	proxy_set_header   Host             $host;
	# 代理路由信息，此处取IP有安全隐患
	proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
	# 真实的用户访问协议
	proxy_set_header   X-Forwarded-Proto $scheme;
}
```

<!--more-->

### 静态请求
静态资源请求、交给Nginx处理:    
```
#静态文件交给nginx处理
location ~ .*\.(htm|html|gif|jpg|jpeg|png|bmp|swf|ioc|rar|zip|txt|flv|mid|doc|ppt|pdf|xls|mp3|wma)$
{
	root /usr/local/tomcat/apache-tomcat-7.0.65/webapps;
	expires 30d;
}
#静态文件交给nginx处理
location ~ .*\.(js|css)?$
{
	root /usr/local/tomcat/apache-tomcat-7.0.65/webapps;
	expires 1h;
}
```

- - -
上帝的最大光荣不在于财富的无限威力，而在于克己无私、受苦受难的爱!

-- 《汤姆叔叔的小屋》