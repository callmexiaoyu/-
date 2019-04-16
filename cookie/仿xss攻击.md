# xss攻击
[TOC]
***
## xss攻击原理

+ cookie不添加httpOnly

`服务器发送一段字符串，浏览器会根据发送的文本类型text/html进行html渲染，而浏览器出现用户提交的文本发布的功能时，当用户提交的文本是一段浏览器执行的js脚本时，该脚本会拼接到html页面中，且当该脚本存到数据库，当其他用户浏览就会同样执行，扩散，这是可以在该脚本中嵌入一段ajax脚本，将用户的信息如cookie，由于cookie在客户端浏览器通过js脚本进行增删改查操作，则就能配合ajax将用户的cookie信息发送到其他服务端，实现了xss攻击`

```js
//引入express模块
var express = require('express');
//服务器对象
var server = express();
//引入模板引擎
var template = require('art-template');

var fs = require("fs");

var path = require("path");

//express中间件辅助插件，帮助express完成post请求
var bodyParser = require('body-parser');

server.use(bodyParser.urlencoded({
	extended: false
}))

// parse application/json
server.use(bodyParser.json());

server.engine('html', require('express-art-template'));

server.set('views', './');

server.get("/", function(req, res) {

	res.render('模仿xss攻击.html')

});

server.post("/submit", function(req, res) {

	//console.log(req.body);
	//通过中间件获得用户提交的评论文本

	//并且将用户发布的评论，渲染到用户的页面上
	// res.render("模仿xss攻击.html", req.body);
	//art-template模板语言想要攻击成功需要{{#submit}}，#是当文本出现标签时连同标签一起渲染



	//模拟xss攻击
	var str = req.body.submit;

	//网页存在用户用户提交一段文本，且该文本要渲染到页面中，渲染页面需要拼接html的字符串

	//当用户提交页面包含html可执行的脚本语言时，相当于将该脚本拼接到html中，且告诉浏览器发送的字符串以text/html解析

	//此时脚本语言就会执行，不过谷歌浏览器拦截异常代码

	fs.readFile(path.join(__dirname, "模仿xss攻击.html"), 'utf-8', function(err, data) {
		var htmlstr = data.replace(/(<div>)([^]*)(<\/div>)/, function(match, $1, $2, $3) {

			return $1 + str + $3;
		});

		//console.log(htmlstr)
		res.setHeader("Content-Type", "text/html;charset=utf-8");
		res.end(htmlstr);
	})
});
server.listen(80, function() {
	console.log("服务器已经启动");
})
```

嵌入的js脚本也需要ajax跨域通信