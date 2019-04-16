# node框架express使用
***
[TOC]
***
## 安装express
`npm install express -save`

`npm install art-template -save`
**express模板辅助插件**
`npm install express-art-template -save`
**express辅助post解析插件**
`npm install body-parser -save`
***
## express案例详解
```js
//引入express模块
var express = require('express');
//服务器对象
var server = express();
//引入模板引擎
var template = require('art-template');
//核心模块，post解析query
var querystring = require('querystring');


//express中间件辅助插件，帮助express完成post请求
var bodyParser = require('body-parser');

server.use(bodyParser.urlencoded({ extended: false }))

// parse application/json
server.use(bodyParser.json());

//配置express模板引擎
//server.engine('art', require('express-art-template'));
//express为response提供render方法，默认不能使用
//使用方法，response.render('html模板名',{模板数据})
//express-art-template将art-template整合到express中，必须引入art-template模块

//art表示默认在views文件下找以xxx.art后缀名的文件，需要将html后缀名改为art
//views文件下存放html，为express渲染约定
//response.render('xxx.art',{模板数据})
//这样便于区分模板html和非模板html，后缀名art表示渲染模板

//缺点一是编辑器无法打开，重新选择编辑器打开，但无法区分模板html和普通html
//修改art为html
server.engine('html', require('express-art-template'));
//缺点二，无模板数据时，不能渲染普通html，只能渲染.art后缀名，需要用原生的读取fs模块
//response.render('xxx.html')


//总结express模板只能以特定xxx结尾的页面，有模板数据渲染，无模板数据普通html
//一旦xxx!=html时，则html文件只能通过核心模块fs读取，然后通过原生end或express的send发送
//区分了模板文件和普通html


//修改默认views文件夹
//server.set('views', './admin/')

var fs = require('fs');

//开放public静态资源

//写法一
// server.use(express.static('./public/'));

//隐藏开放文件夹public，开放public目录下所有静态资源
//url地址localhost/image/3.png成功请求
//url地址中忽略资源所在的路径

//url地址localhost/public/image/3.png请求失败


//方法二
server.use("/public/", express.static('./public/'));


//在url地址中嵌入public，或者任意字符串形式，组成url地址
//url地址localhost/public/image/3.png成功请求
//或者url地址localhost/xxx/image/3.png均能访问，并不代表文件存放在xxx文件夹中

//url地址localhost/image/3.png请求失败

//以上混淆url路径与文件在服务器的真实地址关系


//express路由处理，get请求
//原理server监听请求，请求匹配
server
	.get("/hello", function(req, res) {
		//文本信息
		res.send("hello world");
		//相比原生，无需设置响应头MIME类型
		//res.send("您好，世界");

		//原生对象正常使用
		//res.end("hellow world");

		//res.sendFile( __dirname + "/" + "index.htm" );
	})
	.get("/", function(req, res) {

		//读取json
		fs.readFile("./data/info.js", function(e, data) {
			//获取的字符串转成json格式
			var json = JSON.parse(data.toString());
			//路径./不可少，表示当前文件所在目录
			// console.log(json)

			//默认在该文件目录下的views文件夹下寻找index.art模板html
			//res.render("admin.art", json);

			res.render("index.html", json);
			res.send();
		});
	})
	.get("/login", function(req, res) {
		//三种方法
		// fs.readFile("./views/login.html", function(e, data) {
		// 	data.toString();

		//混用原生
		// res.setHeader("Content-Type", "text/html;charset=utf-8");
		// res.end(data);

		//框架原生
		// 	res.send(data);

		// });

		//框架模板渲染
		res.render("login.html");
		console.log("跳转至登录界面");

		//处理表单请求,这里思考get对处理原理
	}).get('/publish', function(req, res) {
		//虽然监听仅仅是publish请求，但如何后面跟的查询字符串自动被解析
		//原生的需要调用  核心模块url.parse(req.url转码),true).query，其中true表示解析查询字符串
		console.log(req.query);

		fs.readFile("./data/info.js", function(e, data) {

			//读取json数据库并转成json对象
			data = JSON.parse(data.toString());
			//添加发布时间
			req.query.date = new Date().toLocaleString();
			//将评论添加到json对象
			data.info.unshift(req.query);
			//json对象字符串化
			data = JSON.stringify(data);
			//写入数据库
			fs.writeFile("./data/info.js", data, function() {
				//回调函数
				//写完成功发送跳转信息
				//这里可以使用ajax请求进行交互且使用post进行提交

				//原生重定向
                 //异步请求服务端重定向无效，只针对同步请求，表单默认请求是同步请求
				// res.statusCode = 302;
				// res.setHeader("Location", "/");
				// res.end();

				//express重定向
				res.redirect("/");
			});
		});
	})
	//post请求，由于post请求不通过url查询字符串,
	//在请求header中Form Data中

	//express框架中午获取post请求体的API
	.post('/publish', function(req, res) {

		//原生处理post请求
		req.on('data', function(chunk) {
			console.log("收到post请求"); //收到post请求的Form Data数据，需要进行转码
			//chunk一个query字符串，不含?的查询字符串
			var str = decodeURIComponent(chunk);

			//仅解析=和&符号，而？符号不解析，返回类似json对象
			var query = querystring.parse(str);
			console.log(query);

			fs.readFile("./data/info.js", function(e, data) {
				try {
					//读取json数据库并转成json对象
					data = JSON.parse(data.toString());
					//添加发布时间
					query.date = new Date().toLocaleString();
					//将评论添加到json对象
					data.info.unshift(query);
					//json对象字符串化
					data = JSON.stringify(data);
					//写入数据库
					fs.writeFile("./data/info.js", data, function() {
						//回调函数
						//写完成功发送跳转信息
						//这里可以使用ajax请求进行交互且使用post进行提交

						//注意重定向
						res.statusCode = 302;
						res.setHeader("Location", "/");
						res.end();
					});

				} catch (e) {
					res.setHeader("Content-Type", "text/plain;charset=utf-8");
					res.end("404 no find");
				}
			});
		});

		//express中间件body-parser处理后
		console.log('req增添了body属性，req.body:',req.body);

		fs.readFile("./data/info.js", function(e, data) {

			//读取json数据库并转成json对象
			data = JSON.parse(data.toString());
			//添加发布时间
			req.body.date = new Date().toLocaleString();
			//将评论添加到json对象
			data.info.unshift(req.body);
			//json对象字符串化
			data = JSON.stringify(data);
			//写入数据库
			fs.writeFile("./data/info.js", data, function() {
				//回调函数
				//写完成功发送跳转信息
				//这里可以使用ajax请求进行交互且使用post进行提交

				//原生重定向
				// res.statusCode = 302;
				// res.setHeader("Location", "/");
				// res.end();

				//express重定向
				res.redirect("/");
			});
		});
	})
	.listen(80, function() {
		console.log("开启80端口服务器");
	});

//不是正确url地址返回404
```
