---
layout: post
title:  Spring Task 定时任务
categories: JAVA
tags: spring
description: linux Crontab 注意细节
---

* content
{:toc}


### 说明
spring task 是一个定时任务、和Linux 的Crontab 相同

### 命令格式
命令基本格式:    
```
Java(Spring)
*    *    *    *    *    *    *
-    -    -    -    -    -    -
|    |    |    |    |    |    |
|    |    |    |    |    |    + year [optional]
|    |    |    |    |    +----- day of week (0 - 7) (Sunday=0 or 7)
|    |    |    |    +---------- month (1 - 12)
|    |    |    +--------------- day of month (1 - 31)
|    |    +-------------------- hour (0 - 23)
|    +------------------------- min (0 - 59)
+------------------------------ second (0 - 59)
``` 

<!--more-->
### 配置
spring.xml    
schema:    
`xmlns:task="http://www.springframework.org/schema/task"`    

xsi:    
`http://www.springframework.org/schema/task http://www.springframework.org/schema/task/spring-task.xsd`

配置：
```xml
<!-- 开启这个配置，spring才能识别@Scheduled注解   -->
<context:component-scan base-package="com.task"/>
<task:annotation-driven/>
```
### 任务类
```java
package com.task;
@Component
public class PlayerTask {

    @Scheduled(cron = "0 */1 * * * ?")
    public void taskJob(){
        System.out.println("每一分钟次打印下："+ new Date().toString());
    }

    @Scheduled(cron = "*/10 * * * * ?")
    public void taskSecond(){
        System.out.println("每10秒执行一次：" + new Date().toString());
    }
}
```


- - - 
从伟大到可笑，相差只有一步。

-- 《人间喜剧》