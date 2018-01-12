---
layout: post
title: Sequelize中Model的API
description: Sequelize中Model的API
category: Node.js
---

## 1 findAll()- 查询多条数据
> 根据属性查询所有符合条件的数据

	// 查询attr1 = 42 , attr2 = cake 的所有数据
	Model.findAll({
	  where: {
	    attr1: 42,
	    attr2: 'cake'
	  }
	})
	
	// $gt即表示大于(attr1 > 50)，$lte即小于等于，$in即在这个scope之中，$ne即不等于
	Model.findAll({
	  where: {
	    attr1: {
	      $gt: 50
	    },
	    attr2: {
	      $lte: 45
	    },
	    attr3: {
	      $in: [1,2,3]
	    },
	    attr4: {
	      $ne: 5
	    }
	  }
	})
	
	

## 2 findById() - 通过Id查询单条数据
> 根据id来获取单条数据

	User.findById('yanzefan').then(function (result) {
        res.render('Admin/index',{
            name: result.name,
            phoneNumber: result.phoneNumber,
            sex: result.sex
        })
    }).catch(next);

## 3 findOne() - 通过条件获取单条数据

	Project.findOne({ 
		where: {
			title: 'aProject',
			name : 'stevenjack'
		} 
	})

## 4 findAndCountAll - 查找并统计

	Project.findAndCountAll({
	     where: {
	        title: 'stevenjack'
	        }
	     },
	     offset: 10,
	     limit: 2
	  })
  
## 5 create() - 创建保存新实例

	User.create({
        userName: 'gaoyuyue',
        name: 'stevenjack',
        phoneNumber: '158222',
        sex: true,
        isDelete: true,
        password: 'asdasdsadasd',
        role: 'user'
    }).then(function (result) {
        res.json({
           status:1,
           data:result
        });
    }).catch(next);

## 6 update() -- 更新数据

	User.update(data,{
        where:{
            userName:data.userName
        }
    }).then(function (result) {
        res.json({
            status:1,
            data:result
        });
    }).catch(next);

## 7 destroy() - 删除记录

	 User.destroy({
        where:{
            userName:userName
        }
     }).then(function () {
         res.json({
 
         });
     }).catch(next);
