# node 核心模块
***
[TOC]
***
## fs读写文件路径基准
**若使用相对路径./或../**
`当模块文件嵌套时，子模块加载node的核心模块fs时，其读写路径都必须以数据文件相对执行文件的相对路径为基准，而不是读写模块文件所在路径为基准`
```js
├─app.js
├─data
│  └─data.json
├─public
│  ├─css
│  ├─img
│  ├─js
│  └─lib
├─router
│  ├─routerStudent.js
│  └─studentCrud.js
└─views
   ├─index.html
   └─login.html
```
`其中router的文件以及app.js均加载了fs模块，则当执行哪个文件时，它引入的其余模块文件的fs的读写路径都以它相对数据文件data.json为基准，由于一般app.js当入口文件，故它加载的所有子模块的读写路径都以app.js相对data.json为基准，也就是执行的文件不同，路径不同`
**可以认为读写模块fs加载执行时的文件上**
## 动态绝对路径
```js
var path = require("path");
var dbPath = path.join(__dirname,"../data/data.json");
//统一用这种方式处理
```
`绝对路径`
./表示node执行命令式所在路径，相对路径
子模块使用./或../等相对路径，app.js执行时统一变为app.js为基准的路径
使用绝对路径
`require(./)，模块中相对路径则是相对模块文件自身的路径，不受执行node命令路径影响，不统一化`