---
layout: post
title: Express框架与Node.js的使用
description: Express框架与Node.js的使用
category: Node.js
---

## 第一步：安装Node.Js
### 先在官网根据自己的系统来下载node.js，推荐下载.mis格式，不需要自己手动去配置环境。
### 在cmd中输入
    node -v
### 如果成功，则显示nodejs的版本号,之后输入
    npm -v
### 如果成功，则显示npm的版本号
### 这样nodejs就安装好了

## 第二步:全局安装所需的依赖包和工具包
### 在node安装好之后，在cmd框中，全局安装express，输入以下语句
    npm install -g express
### 输入以下，查看安装是否成功
    express --version
### 安装express-generator应用生成器
    npm install express-generator -g

## 第三步：安装WebStorm软件
### 直接在官网上根据自己系统的版本下载WebStorm软件。

## 第四步：使用WebStorm，创建基本框架
### 打开WebStorm后，->new Project->Node.js Express App
> **Location** 选择自己的仓库位置
> 
> **Node Interprete**r 会直接默认选择你node.exe的位置
> 
> **Version** express的版本
> 
> **Templete** 选择模板，新手推荐使用ejs，其基本与html相似，jade的话，不建议新手去常识
> 
> **Css Plai**n 选择默认就行

## 第五步：运行项目
### 直接点击Run的按钮即可
