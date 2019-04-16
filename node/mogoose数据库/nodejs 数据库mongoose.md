# nodejs 数据库MongoDB
***
[TOC]
***
### mongodb官方模块
全局命令安装
`npm install mongodb -g`
本地
`npm install mongodb --save`
https://github.com/mongodb/node-mongodb-native
***
### mongodb第三方模块
`npm install mongoose -save`
1. 开启数据库
2.  链接数据库
3.  开启compass可视化，查看
#### 增 
```js
//引入mongoose数据操作模块
var mongoose = require('mongoose');
//cmd开启数据库和链接数据库后，nodejs运行该脚本

//链接数据库服务的test数据库，不存在的数据库，当插入数据自动新建数据库
mongoose.connect('mongodb://localhost:27017/demo', {
	useNewUrlParser: true
});

//对一个集合，也就是数据结构进行配置
var schema = mongoose.Schema;

//配置函数实例化一个配置对象，定义各个数据类型数据类型
var blogSchema = new schema({
	//避免脏数据
	//定义实参数据写入，必须有且类型为String型，不过字符型数值也能存入
	username: {
		type: String,
		required: true //必须有
	},
	password: {
		type: Number,
		required: true
	},
	//写入数据时，没有也能成功写入，可选项，但一旦写入必须为String
	email: {
		type: String
	},
    date: {
        type:Date,
        Default:Date.now
    }
});

//创建一个以数据结构配置的XXX构造函数，无论xxx是否大小写，保存时一律小写化同时添加s

var blogCollection = mongoose.model('blog', blogSchema);

//该构造函数实例化，注入实参
var blog = new blogCollection({
	username: "jack",
	password: "123",
});

//写入数据库
//kitty.save().then(() => console.log('meow'));

blog.save(function(err, data) {
	console.log(err);//null和错误对象
	console.log(data);//undefined和写入的数据
});
```
#### 删
```js
mongoose.model('blog',{}).deleteOne({username:"li"},function(err, data) {
	if (err) {
		console.log(err);//null
	} else {
		console.log(data);//复合条件的对象的集合数组
	}
});

mongoose.model('blog',{}).deleteMany({username:"jack"},function(err, data) {
	if (err) {
		console.log(err);//null
	} else {
		console.log(data);//复合条件的对象的集合数组
	}
});
```
#### 改
```js
mongoose.model('blog', {}).findOneAndReplace({
    username: "li"
}, {
    password: "123456"
}, function(err, data) {
	console.log(arguments);
    if (err) {
        console.log(err); //null
    } else {
        console.log(data); //复合条件的对象的集合数组
    }
});
```
`版本不一样`详见https://mongoosejs.com/docs/middleware.html