---
layout: post
title:  "Intellij IDEA 中如何部署项目到 Tomcat？"
categories: JAVA
tags: 编辑器
description: Intellij IDEA 中如何部署项目到 Tomcat
---

* content
{:toc}


## 配置项目
- - -
### 添加Project Structure
打开项目设置，快捷键`ctrl + shift + alt`
![ProjectStructure|center](http://7xnudh.com1.z0.glb.clouddn.com/17-3-27/76771216-file_1490623552523_338b.png)
 1. 设置Modules
 2. 设置`classer` 编译文件目录
 >  `提示`：**WEB-INF**  默认没有**classes**目录，手动在 WEB-INF 目录中添加 classes 文件夹

 3.  把`classes`文件夹设置为`Excluded`
 
 <!--more-->
- - -
#### 设置`Paths`
 ![path设置|center](http://7xnudh.com1.z0.glb.clouddn.com/17-3-27/43292426-file_1490624247629_603f.png)
 
 设置部署输出路径、路径为`WEB-INF`下新建的`classes`目录
 - - - 

#### 添加需要部署的`lib`
![lib|center](http://7xnudh.com1.z0.glb.clouddn.com/17-3-27/19141554-file_1490624724563_5e13.png)

> 选中 `Dependencies` 右边添加`lib`,出现一个下拉框、选择`java`

会弹出现有的`lib`、选择 `Tomcat 7.0.23`
![Tomcatlib|center](http://7xnudh.com1.z0.glb.clouddn.com/17-3-27/23933082-file_1490624925529_3182.png)
- - - 
### 添加 Tomcat
IDEA 右上角点击图片红圈位置

![TomcatEdit|center](http://7xnudh.com1.z0.glb.clouddn.com/17-3-27/60666771-file_1490625151520_12066.png)
- - -

##### 添加一个本地Tomcat 
![addLocalTomcat|center](http://7xnudh.com1.z0.glb.clouddn.com/17-3-27/59191481-file_1490625323088_133e6.png)
- - -

##### 配置Tomcat信息
![@图七|center](http://7xnudh.com1.z0.glb.clouddn.com/17-3-27/15945265-file_1490625606399_32af.png)

1. 给这个Tomcat 取个名字吧
2. 选择Tomcat目录
3. 启动完Tomcat之后会自动用选定的浏览器打开项目
4. 给Tomcat一个访问地址
5. 选择JDK
6. Tomcat 端口 *默认即可*

- - - 
##### 添加部署项目名
![添加部署项目名|center](http://7xnudh.com1.z0.glb.clouddn.com/17-3-27/87187346-file_1490626146379_f2d1.png)

项目部署名浏览器访问项目的名字

与`图七`中第`4`点搭配使用，如改图片的`项目部署名`是`/`，那么访问地址是：
>  http://localhost:8081/

假如把`项目部署名`更改为`hello`,那么访问地址应该是：
> http://localhost:8081/hello

- - - 

#### 启动Tomcat
点击Tomact小绿点、启动Tomcat<br/>
![StartTomcat|center](http://7xnudh.com1.z0.glb.clouddn.com/17-3-27/44966291-file_1490627033782_3842.png)

启动成功：
![DeploySuccess|center](http://7xnudh.com1.z0.glb.clouddn.com/17-3-27/96445494-file_1490627485660_11b46.png)
- - -
#### 浏览器访问
输入地址：`http://localhost:8081/`
> **提示**：如果你`图七`中第`3`个步骤勾选了、浏览器会自动打开这个地址，否则需要手动输入地址！


- - - 

效果图：

![](http://7xnudh.com1.z0.glb.clouddn.com/17-3-27/76528-file_1490627755387_20dc.png)
