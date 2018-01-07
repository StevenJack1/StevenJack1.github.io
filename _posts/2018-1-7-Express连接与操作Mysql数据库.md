---
layout: post
title: Express连接与操作Mysql数据库
description: Express连接与操作Mysql数据库
category: Node.js
---

## 第一步
### 首先确定你的node,以及express已经安装好

## 第二步：下载mysql依赖包
### 全局安装mysql的驱动包
    npm install -g mysql
### 安装node supervisor工具包
    npm install -g supervisor 
### supervisor工具包可以监视代码的变动，一旦代码有所改变就自动重启服务

## 第三步：在数据库中任意建一张表
### 我们需要在数据库中有一张表，以便于使用express去连接与操作
### 假设创建database为testDemo,表名为test
    create database testDemo;
	CREATE TABLE `test` (
  		`id` int(11) NOT NULL AUTO_INCREMENT,
  		`book_name` varchar(200) NOT NULL,
  		`book_author` varchar(200) NOT NULL,
  		`book_press` varchar(200) NOT NULL,
  		`book_isbn` varchar(200) NOT NULL,
  		 PRIMARY KEY (`id`)
	) ENGINE=InnoDB DEFAULT CHARSET=utf8

### 添加几条数据
	INSERT INTO books VALUES(null,'Java编程思想（第4版）','[美]埃克尔','机械工业出版社','9787111213826'); 
	INSERT INTO books VALUES(null,'算法导论（原书第3版）','（美）Thomas H.Cormen，Charles E.Leiserson，Ronald L.Rivest，Clifford Stein','机械工业出版社','9787111407010'); 
	INSERT INTO books VALUES(null,'编程珠玑（第2版·修订版）','[美]乔恩·本特利（Jon Bentley）','人民邮电出版社','9787115357618'); 
	INSERT INTO books VALUES(null,'深入浅出Node.js','朴灵','人民邮电出版社','9787115335500'); 
	INSERT INTO books VALUES(null,'Node.js权威指南','陆凌牛','机械工业出版社','9787111460787'); 
	INSERT INTO books VALUES(null,'HTML5权威指南','（美）弗里曼','人民邮电出版社','9787115338365');

## 第四步：代码实现
### 在routes中添加一个book.js文件，将如下内容写入book.js中
    var express = require('express');
	var router = express.Router();
	
	var bookDao = require('../dao/bookDao');
	
	//拦截/book请求
	router.get('/', function(req, res, next) {
	    res.send('respond with a resource');
	});
	/** * 查询全部书籍信息
	 *
	 * 访问链接 localhost:3000/book/queryAll
	 * @param {[type]} req [description]
	 * @param {[type]} res [description]
	 * @param {[type]} next) { bookDao.query(req, res, next);} [description]
	 * @return {[type]} [description]
	 */
	router.get('/queryAll', function(req, res, next) {
	    bookDao.queryAll(req, res, next);
	});
	
	module.exports = router;
### 项目根目录新建dao文件夹，并且在文件夹中新建bookDao.js,写入如下代码
	var mysql = require('mysql');
	var client = mysql.createConnection({
	    host: 'localhost', //数据库的ip地址
	    user: 'root',       //自己本地mysql数据库的账号
	    password: 'root',    // 自己本地mysql的密码
	    database: 'testDemo',    // database的名字
	    port: 3306
	});
	client.connect();
	module.exports = {
	    queryAll: function(req, res, next) {
	        client.query('select * from 'test', function(err, results, fields) {
	            if (err) {
	                throw err;
	            }
	            if (results) {
	                var arr = [];
	                for (var i = 0; i < results.length; i++) {
	                    console.log("%d\t%s\t%s", results[i].id, results[i].book_name, results[i].book_author, results[i].book_press, results[i].book_isbn);
	                    var book = {};
	                    book.id = results[i].id;
	                    book.name = results[i].book_name;
	                    book.author = results[i].book_author;
	                    book.press = results[i].book_press;
	                    book.isbn = results[i].book_isbn;
	                    arr.push(book);
	                }
	                //返回视图
	                res.render('book', { "data": arr, "name": "李明", "pwd": "123456" });
	            }
	            client.end();
	        });
	    }
	}

### 在views文件夹中新建book.ejs文件，在文件中写入如下内容
	<!DOCTYPE html>
	<html>
	<head>
	    <title>图书信息列表</title>
	    <link rel='stylesheet' href='/stylesheets/style.css' />
	</head>
	<body>
	<h1>图书信息列表</h1>
	<table border="1">
	    <thead>
	        <tr>
	            <th>ID</th>
	            <th>书名</th>
	            <th>作者</th>
	            <th>出版社</th>
	            <th>ISBN</th>
	        </tr>
	    </thead>
	    <tbody>
	    <% data.forEach(function (item) {   %>
	        <tr>
	            <td><%= item.id %></td>
	            <td><%= item.name %></td>
	            <td><%= item.author %></td>
	            <td><%= item.press %></td>
	            <td><%= item.isbn %></td>
	        </tr>
	    <% }) %>
	
	    </tbody>
	</table>
	</body>
	</html>

### 修改项目根目录的app.js文件在var users = require('./routes/users');后面添加
	var book = require('./routes/book'); 

### 在app.use('/users', users);后面添加
	app.use('/book', book)

### 简单修改下table和h1的样式，在public/stylesheets/style.css文件中添加如下代码
	h1{
	  font-size:36px;
	}
	table, table td, table th {
	  border: 1px solid #999999;
	  border-collapse: collapse;
	  padding:10px;
	}
	table th{
	  background-color: #e3e3e3;
	  color: #cf4646;
	  font-size:16px;
	  font-family: '微软雅黑'
	}

## 第五步：启动项目
	supervisor ./bin/www

## 第六步：查看效果
	http://localhost:3000/book/queryAll     // 浏览器访问


