---
layout: post
title: IntellijIDEA中JavaWeb之软件安装及项目导入
description: IntellijIDEA中JavaWeb之软件安装及项目导入
category: javaweb
---

# Intellij IDEA中JavaWeb之软件安装及项目导入
## 准备
- gradle压缩包
- Intellij IDEA压缩包
- tomcat压缩包
- JRebel
- Git压缩包
- SourceTree压缩包
- MySql压缩包
- Redis压缩包
- JDK

## 安装gradle
解压缩gradel，并将其默认安装在C目录下

## 解压缩其他软件包
将其他压缩包分别解压并安装，并对JDK进行环境变量的配置，并将Redis以及MySql分配设置成开机自启

## 项目导入
### 第一次导入项目
1.如果是第一次导入项目，则需在打开IDEA后，点击Configure,选择Plugins
![1](http://stevenjack1.github.io/picture/2017-7-15_1.png)

2.在搜索栏中输入gradle，并选中打勾，最后点击OK.
![2](http://stevenjack1.github.io/picture/2017-7-15_2.png)

3.之后点击Import Project,找到gradle,点ok,输入GroupId及ArtifactId

4.这时，会弹出以下图，构中前两个，并Use local gradle distribution,选择在c盘根目录下的gradle.
![3](http://stevenjack1.github.io/picture/2017-7-15_3.png)

5.对IDEA中的每一个Project都需要配置JDK及tomcat,配置如下：
![4](http://stevenjack1.github.io/picture/2017-7-15_4.png)

6.点击Edit Configuraion中后，点击+,选择Tomcat，选择Local,将以下两个分配选择update class and resources,HTTP Port 一般为8080,JMX port 可随意设置(我也忘记一般为多少)
![5](http://stevenjack1.github.io/picture/2017-7-15_5.png)

7.配置deployment,如图选择第二个
![6](http://stevenjack1.github.io/picture/2017-7-15_6.png)

8.可以去Run你的java 项目了

转载请注明出处：[http://www.yanzefan.top/2017/07/15/IntellijIDEA中JavaWeb之软件安装及项目导入/](http://www.yanzefan.top/2017/07/15/IntellijIDEA中JavaWeb之软件安装及项目导入/)

