# express增删改查crud
***
[TOC]
***
### 路由设计
|     功能     | 请求方法 |  请求路径   | get参数 |         post参数         |                             备注                             |
| :----------: | :------: | :---------: | :-----: | :----------------------: | :----------------------------------------------------------: |
| 首页渲染列表 |   GET    |      /      |         |                          |      模板数据列表，增加功能a链接和删除功能a链接?id=xxx       |
|   跳转增加   |   GET    | /login/add  |         |                          | 空白列表，渲染表单action的url路径以及method方法post，以备增加列表提交 |
|   增加提交   |   POST   | /login/add  |         |    ?name=xxx&age=xxx     |               无需渲染页面，提交数据以及重定向               |
|   跳转编辑   |   GET    | /login/edit | ?id=xxx |                          | 渲染id相关数据页面，表单action的url链接和method方法post，以备编辑列表提交 |
|   编辑提交   |   POST   | /login/edit |         | ?id=xxx&name=xxx&age=xxx |                 提交数据更新请求，以及重定向                 |
| 首页列表删除 |   GET    |   /delete   | ?id=xxx |                          |                   附带id的url请求，重定向                    |
***
### 路由模块管理
#### node原生路由管理 
**入口文件app.js**
```js
var router = require('./router/router.js')；
router(app,fs)；
```
**路由文件router.js**
```js
module.exports = function(app,fs) {
	app
		.get("/", function(req, res) {
		})
		.get("/login", function(req, res) {
		});
};
```
#### express路由管理
**入口文件app.js**
```js
var express = reqire('express');
var router = require('./router/router.js')；
var app = express();
app.use(router);
```
**路由文件router.js**

```js
var fs = require('fs');
var express = require('express');
var router = express.Router();
router
	.get("/", function(req, res) {
	})
	.get("/login", function(req, res) {
	});
```
***
### 业务增删改查文件expressCrud.js
```js
var fs = require('fs');

//难点，路径设置
//当模块加载到app.js时，读写文件的路径是以数据文件相对执行js的脚本所在路径的相对位置，而不是模块位置，所以执行app.js以该路径为基准
//这样无论模块在项目什么位置，app.js加载的模块中读写文件都以读写的文件相对app.js为准，可以认为读写模块fs加载执行时的文件上
var dbPath = './data/data.json';


//当单独测试子模块时，执行模块文件，则路径变更到数据文件相对模块文件的路径
// var dbPath = '../data/data.json';

//四个url路径，4个get请求，2个post请求
// var arr = ["/login/new", "login/delete", "login/edit", "/login"];


//负责编辑url状态，由于切换表单的action的url请求以及切换请求方式method，get或post
var editurl = function(num, method, callback) {
	fs.readFile(dbPath, 'utf-8', function(err, data) {
		if (err) {
			callback(err);
		} else {
			var json = JSON.parse(data);
			json.action.url = json.action.arrurl[num];
			json.action.method = method;
			fs.writeFile(dbPath, JSON.stringify(json), 'utf-8', function() {
				callback(null, json.action); //写入成功返回新生数据
			});
		}
	});
};
// editurl(0,"post",function(err,data){
// 	console.log(data);
// });



//查找所有学生
var findAllStudent = function(callback) {
	fs.readFile(dbPath, 'utf-8', function(err, data) {
		//err是当没找到，err则指向错误对象
		if (err) {
			callback(err);
		} else {
			callback(err, JSON.parse(data));
		}
	});
};
// findAllStudent(function(err, data) {
// 	console.log(data);
// });



//添加学生
var addStudent = function(one, callback) {
	fs.readFile(dbPath, 'utf-8', function(err, data) {
		if (err) {
			callback(err);
		} else {
			var json = JSON.parse(data);
			//为新生生成新id，有问题，空数组
			var id = json.student[json.student.length - 1].id + 1;
			one.id = id;
			json.student.push(one);

			fs.writeFile(dbPath, JSON.stringify(json), 'utf-8', function() {
				callback(null, one);
			});
		}

	});
};
// addStudent({name:"peter",age:20}, function(err, data){
// 	console.log(data)
// });



//删除单个
var deleteStudent = function(id, callback) {
	fs.readFile(dbPath, 'utf-8', function(err, data) {
		if (err) {
			callback(err);
		} else {
			var json = JSON.parse(data);
			var stop = false;
			json.student.forEach(function(ele, index) {
				if (ele.id === id) {
					//由于作用域嵌套，回调函数声明和调用传参，否则回调函数内部无法读取其变量
					stop = true;
					json.student.splice(index, 1);
					fs.writeFile(dbPath, JSON.stringify(json), 'utf-8', function() {
						callback(null, true); //删除成功
					});
				}
			});
			if (!stop) {
				//没找到目标id，无法删除返回空对象
				callback(null, null);
			}
		}

	});
};
// deleteStudent(10000, function(err, data) {
// 	console.log(data);
// });



//编辑学生
var editStudent = function(one, callback) {
	fs.readFile(dbPath, 'utf-8', function(err, data) {
		if (err) {
			callback(err);
		} else {
			var json = JSON.parse(data);
			var stop = false;
			json.student.forEach(function(ele, index) {
				if (ele.id === Number(one.id)) {

					stop = true;
					json.student.splice(index, 1, one);
					fs.writeFile(dbPath, JSON.stringify(json), 'utf-8', function() {
						callback(null, json.student[index]); //写入成功返回新生数据
					});
				}
			});
			if (!stop) {
				//没找到id返回null
				callback(null, null);
			}
		}
	});
};
// editStudent(10006,{name:"peter",age:100}, function(err, data) {
// 	console.log(data);
// });



//查找单个学生id
var findStudent = function(id, callback) {
	fs.readFile(dbPath, 'utf-8', function(err, data) {
		if (err) {
			callback(err); //当数据库不存在调用
		} else {
			var json = JSON.parse(data);
			//遍历完，无匹配id，返回空对象
			var stop = false;
			json.student.forEach(function(ele, index) {

				if (ele.id === id) {
					//由于作用域嵌套，回调函数声明和调用传参，否则回调函数内部无法读取其变量
					stop = true; //找到
					return callback(null, json.student[index]);
				}
			});
			if (!stop) {
				//未找到传入空对象
				callback(null, null);
			}
		}
	});
};
//回调函数的作用域嵌套
// findStudent(10001, function(err, data) {
// 	console.log(data);
// });


module.exports.url = editurl;
module.exports.findAll = findAllStudent;
module.exports.find = findStudent;
module.exports.add = addStudent;
module.exports.delete = deleteStudent;
module.exports.edit = editStudent;
```
+ `回调函数获取异步函数内部参数`
+ `表单提交字符型数字，进行相互转换`
+ `node核心模块读写文件路径相对于执行js文件路径，模块嵌套，子模块读写文件路径全部统一为数据文件相对于执行文件路径`
+ `作用域嵌套`
+ `隐藏表单提交`
+  `CDN偶然掉线`
+  `id生成`
+  `回调函数return`否则可能容易报错
+  
***
### 路由文件router.js
```js
// //express框架路由模块管理
var express = require('express');
var router = express.Router();

// //学生数据增删改查封装方法
var student = require('./expressCrud');
// student.findAll(function(err, data) {
// 	//能取到data数据
// 	console.log(data);
// });

//url状态列表
// var arr = ["/login/new", "login/delete", "login/edit", "/login"];

router
	//渲染主页面
	.get("/", function(req, res) {
		student.findAll(function(err, data) {
			console.log(err);
			//这里获取不到data，根本原因在于readFile读取文件的路径基准以执行的文件为基准
			res.render("index.html", data);
		});
	})
	//跳转添加学生页面，渲染页面
	.get("/login", function(req, res) {
		//替换login表单的action以及method
		student.url(0, "post", function(err, data) {
			console.log(data);
			//发送的表单action为/login/new，且method为post
			res.render("login.html", data);
		});
	})
	//添加学生不渲染页面仅仅传数据重定向
	.post("/login/new", function(req, res) {

		//这里将获得新生数据字符串化，不做页面渲染
		req.body.gender = Number(req.body.gender);
		student.add(req.body, function(err, data) {
			if (data) {
				res.redirect("/");
			}
		});
	})
	//跳转编辑页面，渲染页面
	.get("/login/edit", function(req, res) {

		student.url(2, "post", function(err, data) {
			var obj = data;
			student.find(Number(req.query.id), function(err, data) {
				if (data) {
					data.url = obj.url;
					data.method = obj.method;
					//隐藏表单，记录id
					res.render("login.html", data);
				}
			});
		});
	})
	//编辑页面不渲染页面仅仅传数据重定向
	.post("/login/edit", function(req, res) {
		//这里中间件获得字符型数字
		req.body.gender = Number(req.body.gender);
		req.body.id = Number(req.body.id);
		student.edit(req.body, function(err, data) {
			if (data) {
				res.redirect("/");
			}
		});
	})
	//删除
	.get("/delete", function(req, res) {
		student.delete(Number(req.query.id), function(err, data) {
			if (data) {
				res.redirect("/");
			}
		});
	});
module.exports = router;
```
***
### 入口文件app.js
```js
//引入express模块
var express = require('express');
//服务器对象
var server = express();
//引入模板引擎
var template = require('art-template');
//核心模块，post解析query,原生解析post数据
var querystring = require('querystring');


//express中间件辅助插件，帮助express完成post请求
var bodyParser = require('body-parser');
//配置中间件，给req添加body属性，post键值对对象
server.use(bodyParser.urlencoded({
	extended: false
}));
server.use(bodyParser.json());


//配置express模板引擎
//解析后缀名为html的模板
server.engine('html', require('express-art-template'));

//修改默认views文件夹，默认html存放在views文件夹下
//server.set('views', './admin/')


//开放public静态资源

//写法一
//url路径不能包含public
// server.use(express.static('./public/'));

//url请求路径包含/public
//方法二
server.use("/public/", express.static('./public/'));

//原生路由管理
var router = require("./router/student.js");
//router(server);

//express框架路由管理
server.use(router);

//等价于
// server.use(require('./router/student.js'));

//express路由处理，get请求
//原理server监听请求，请求匹配
server
	.listen(80, function() {
		console.log("开启80端口服务器");
	});

//不是正确url地址返回404
```