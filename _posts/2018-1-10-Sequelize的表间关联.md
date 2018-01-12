---
layout: post
title: Sequelize的表间关联
description: Sequelize的表间关联
category: Node.js
---



# 在Sequeelize模型中表间关联

* 一对一(One-To-One)关联
* 一对多(One-To-Many)关联
* 多对多(Belongs-To-Many)关联

## 1.1 一对一(One-To-One)关联

### hasOne()关联表示一对一关系的**外键**存在于目标模型

> 假设有两个表一个是User(用户表),一个Team(队伍表),每个队伍都有一个用户，即`Team.hasOne(User)`
> ，则外键会存在目标模型User中，即在User表中会添加属性teamId，如果想规范名称，则需要如下：`Team.hasOne(User, { foreignKey: 'team_id' });`

### belongsTo()关联表示一对一关系的**外键**存在于**源模型**

> 还是假设有两个表User(用户表),Team(队伍表)，每个用户属于一个队伍，即`User.belongsTo(Team)`,
> 则外键会存在源模型User中，即在User表中会添加属性teamId，如果想规范名称，则需要如下：`User.hasOne(Team, { foreignKey: 'team_id' });`

## 1.2 一对多(One-To-Many)关联

### hasMany()关联是指一个源模型连接多个目标模型,且外键存在于目标模型
> User.hasMany(Comment, { foreignKey: 'user_id' });        //  外键存在于Conmment表中，属性名为user_id
> 
> Comment.belongsTo(User, { foreignKey: 'user_id' });      //  外键存在于Conmment表中，属性名为user_id

## 1.3 多对多(Belongs-To-Many)关联

### 这个关联的构造与前两个有很大的区分，使用Belongs-To-Many 关联是指一个源模型连接多个目标模型。而且，目标模型也可以有多个相关的源。代码构造如下：

> Project.belongsToMany(User, {as: 'Tasks',through: 'worker_tasks',foreignKey: 'userId'});
> 
> User.belongsToMany(Project, {as: 'Workers',through: 'worker_tasks', foreignKey: 'projectId' });

### 这样会创建一张UserProject的表，有userId和projectId属性

## [具体的表间关联源码](https://github.com/StevenJack1/movie/blob/master/%E4%BB%A3%E7%A0%81/models/ref.js)
