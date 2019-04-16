# Cookie 服务端node写入

[TOC]
***
## 原理

http协议无状态协议，无法识别统一url等请求是否来自同一用户，故当登录时，服务端给响应头设置cookie，而cookie保存在浏览器中，当浏览器下次访问url路径时，自动将该cookie，放在请求头中，然后客户端根据相应头的cookie，判断是否是同一用户，cookie是一个不超过4kb的字符串。

`存储cookie是浏览器提供的功能，浏览器专门的一个文件夹存放所有cookie`

同一个域名存放cookie不超过20个，不同浏览器可能不一样，cookie不属于http协议

## 服务端cookie设置

```js
res.writeHead(200, {
	'Set-Cookie': 'myCookie=test',
	'Content-Type': 'text/plain;charset=utf-8'
});
```

```js
"key=name; expires=Thu, 25 Feb 2016 04:18:00 GMT; domain=ppsc.sankuai.com; path=/; secure; HttpOnly"
```

## cookie属性

`属性之间必须由一个;分号和一个空格 隔开`

+ expires

  expries为国际标准时间，记录cookie到期时间，到期浏览器自动检查到期cookie并删去该cookie

  ```js
  var http = require('http');
  
  //简写方式，链式
  http
  	.createServer(function(req, res) {
  
  		// //读取相应头，包含大量信息
  		// console.log(req);
  
  		// //请求头信息，也就是浏览器里的请求头包含的全部信息
  		// console.log(req.headers)
  
  		//获取响应头里的cookie
  		//console.log(req.headers.cookie);
  
  		if (req.url === "/") {
  
  
  			var cookie = req.headers.cookie;
  			
  
  			//如果cookie不存在，设置cookie
  			if (!cookie) {
  				console.log(cookie);
  				//请求时间
  				var datetime1 = new Date().getTime()
  				//时间间隔
  				var timegap = 10000;
  				//到期时间
  				var datetime2 = datetime1 + timegap;
  
  				//cookie必须使用国际标准时间，当地时间在这个基础上加上时区
  				//可以通过new Date().toGMTString()或者 new Date().toUTCString() 来获得。两者等价
  
  				var UTCdate2 = new Date(datetime2).toUTCString();
  				var localtime = new Date(datetime2).toLocaleString();
  
  				//设置cookie
  				//复数响应头
  				res.writeHead(200, {
  
  					//设置到期时间，浏览器会根据expires记录都国际标准时间，自动清除过期cookie
  					//设置了cookie声明周期10秒
  					'Set-Cookie': 'cookieEndTime='+localtime+';path="/"; Expires=' + UTCdate2,
  					'Content-Type': 'text/plain;charset=utf-8'
  				});
  				res.end("cookie不存在已添加");
  			} else {
  				console.log(cookie)
  				res.setHeader("Content-Type", "text/plain;charset=utf-8");
  				res.end("cookie已经存在");
  			}
  
  
  		} else {
  			res.setHeader("Content-Type", "text/plain;charset=utf-8");
  			res.end("请求失败");
  		}
  
  	})
  	.listen(80, function(args) {
  		console.log("服务器启动成功");
  	});
  ```

+ Max-Age

  cookie的声明周期，和expires一样，不同在于，单位为秒表示倒计时，10秒后清除该cookie

  ```js
  res.writeHead(200, {
  	'Set-Cookie': 'cookieEndTime=""; Max-Age='+10+'; httpOnly',
  	'Content-Type': 'text/plain;charset=utf-8'
  });
  ```

+ httpOnly

  ```js
  res.writeHead(200, {
  	'Set-Cookie': 'cookieEndTime='+localtime+';path="/"; Expires=' + UTCdate2 + '; httpOnly',
  	'Content-Type': 'text/plain;charset=utf-8'
  });
  ```

  `浏览器可以通过document.cookie拿到cookie值，而在cookie添加httpOnly禁止客户端通过js脚本拿到cookie，此时返回为空`

  `限制客户端不能修改删除cookie，只有服务端有权限`

+ domain和path

  + 表示当访问的url是在cookie设置的路径和域名或子域名之下，则该请求中会附上cookie，cookie告诉浏览器下次访问什么样的url带上相应的cookie
  + 不设置，自动变成url的domain=host，

### cookie修改
一个name/domain/path三者一致，则覆盖，否则添加新cookie
### cookie删除
给cookie设置一个生命周期，到期自动清除

  