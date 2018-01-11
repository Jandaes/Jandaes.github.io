---
layout: post
title:  Linux Crontab 注意事项
categories: linux
tags: 服务器
description: linux Crontab 注意细节
---

* content
{:toc}


### 说明
linux 的 Crontab 定时任务命令 可以 在 linux 规定的时间做出相应的操作

### 命令格式
命令基本格式:    
```
# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed
``` 

<!--more-->
#### 文件定义
该文件处于：`/etc/crontab`

`*  *  *  *  * user-name command to be executed`

在文件中定义定时任务需要加`user-name`

因为文件是针对整个系统而言的,添加了`user-name` 就是说明这个条定时任务是谁执行的

#### 当前用户
`*  *  *  *  * command to be executed`    
`crontab -l` 查看当前系统登录下的定时任务    
`crontab -e` 编辑当前系统登录下的定时任务    
`crontab -r` 清空当前系统登录下的定时任务


可以不指定用户名、表示当前登录用户的定时任务

#### 常见定时
实例1：每1分钟执行一次myCommand    
`* * * * * myCommand`



实例2：每小时的第3和第15分钟执行    
`3,15 * * * * myCommand`

实例3：在上午8点到11点的第3和第15分钟执行    
`3,15 8-11 * * * myCommand`

实例4：每隔两天的上午8点到11点的第3和第15分钟执行    
`3,15 8-11 */2  *  * myCommand`

实例5：每周一上午8点到11点的第3和第15分钟执行    
`3,15 8-11 * * 1 myCommand`

实例6：每晚的21:30重启smb    
`30 21 * * * /etc/init.d/smb restart`

实例7：每月1、10、22日的4 : 45重启smb    
`45 4 1,10,22 * * /etc/init.d/smb restart`

实例8：每周六、周日的1 : 10重启smb    
`10 1 * * 6,0 /etc/init.d/smb restart`

实例9：每天18 : 00至23 : 00之间每隔30分钟重启smb    
`0,30 18-23 * * * /etc/init.d/smb restart`

实例10：每星期六的晚上11 : 00 pm重启smb    
`0 23 * * 6 /etc/init.d/smb restart`

实例11：每一小时重启smb    
`* */1 * * * /etc/init.d/smb restart`


### 环境变量
`/etc/profile` 定义的环境变量,单独执行可以使用，但是如果放在crontab 里面却出现环境问题.

解决方法:

- 在.sh的shell的脚本头部、写上`source /etc/profile`
- 在定义任务的时候,加载环境变量    
`*  *  *  *  * source /etc/profile /home/restartTomcat.sh`



- - - 
时间决定你会在生命中遇见谁，你的心决定你想要谁出现在你的生命里，而你的行为决定最后谁能留下。

-- 《瓦尔登湖》