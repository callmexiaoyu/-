# express-session
[TOC]
***
## session原理

+ **cookie** `http无状态协议，无法识别用户登录状态等信息，而cookie作为浏览器自带功能，当访问的url与cookie记录的domain及path一致时，会在该请求中添加该cookie信息，浏览器将将识别用户登录状态的信息添加到cookie中，下次每次访问domain时，就会携带该cookie，登录状态的cookie有生命周期，到期浏览器自动删去过期cookie`

+ **session** `由于cookie用户可以进行修改读取，保存用户信息存在风险，故将登录信息等进行加密，客户端看到的是cookie的密文，同时通过httpOnly属性限制用户修改，这样保证了登录状态的安全性，加密以及解读cookie的工具`

  ![1554144125472](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1554144125472.png)

+ **express-session使用** `将留存在客户端的登录状态添加到请求req.session.属性=xxx进行赋值操作，通过req.session.属性;进行读取读取，req.headers.cookie仅仅获取的时密文cookie`

```js
var express = require('express');
var parseurl = require('parseurl');

var app = express();


//session中间件
var session = require('express-session');
app.use(session({
  secret: 'keyboard cat',//加密字符串，可自行设置，混淆
  resave: false,
  saveUninitialized: true
}));

app.get('/', function(req, res, next) {

  //请求投中的cookie值，包含了session部分的密文
  console.log(req.headers.cookie)

  //给请求头设置session对象，添加属性
  //session原理是在cookie上存放的用户登录信息，进行加密成密文cookie，每次发起请求时，session对密文cookie进行解密


  req.session.isLogin = true;
  req.session.name = "jack";

  //一个对象
  console.log(req.session);

  //session是一种加密的cookie，req.session.cookie记录cookie属性
  console.log(req.session.cookie);


  //获取session用于存放在客户端的cookie值
  //console.log(req.session.isLogin);


  //对加密的cookie属性修改
  //声明周期
  var timegap = 10000;
  // req.session.cookie.expires = new Date(Date.now() + timegap);
  req.session.cookie.maxAge = timegap; //单位毫秒

  //该cookie路径，当前请求不能读取，无效
  //req.session.cookie.path = "/login/";

  console.log(req.session.id);

  res.setHeader("Content-Type", "text/html;charset=utf-8");
  res.end();

});


app.get("/login", function(req, res) {
  //获取到请求/login/new设置的cookie
  console.log(req.session);

  res.setHeader("Content-Type", "text/html;charset=utf-8");
  res.end();
});

app.get("/login/new", function(req, res) {
  console.log(req.session);

  req.session.isLogin = true;
  req.session.name = "peter";

  //path设置的原则是当前请求的url能获取到该cookie

  req.session.cookie.path = "/login";

  res.setHeader("Content-Type", "text/html;charset=utf-8");
  res.end();
});


app.listen(80, function(args) {
  console.log("服务器已监听")
})
```

### express-session的cookie属性修改

1. expires/maxAge生命周期

  ```js
    //声明周期
    var timegap = 10000;
    // req.session.cookie.expires = new Date(Date.now() + timegap);
    req.session.cookie.maxAge = timegap; //单位毫秒
  ```
2. path

   `浏览器当访问子域名或者子路径，也会将cookie放在请求中，cookie作用域`

   > www.xxx.com/abc/，该cookie记录的url路径，当访问www.xxx.com/abc/def路径添加该cookie
   >
   > 而www.xxx.com不能获取该cookie

   `设置路径时不能再当前请求url路径父级以上设置`

   > www.xxx.com/abc/，设置session的path，只能在已有的路径上设置，www.xxx.com/abc/def不能设置，www.xxx.com能设置，保证设置的cookie自己能获取到的原则

3. session默认存在内存中