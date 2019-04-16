# npm常用模块
***
[TOC]
***
## nodemon
`npm install nodemon -g`
终端输入
`nodemon 文件名`
监听文件变化，文件保存，则触发`node 文件名`，express搭建服务器时，自动重启服务器，配合express框架使用
***
##  art-template
`npm install art-template -save`
服务端渲染模板，模块中lib/template-web.js，单独提取出来，作为客户端渲染脚本嵌入html页面中

***

## express

`npm install express -save`
基于node的服务端开发框架，需要中间件

### express辅助中间件

+ express-art-template

  链接art-template和express，服务端渲染模板

+ body-parser

  辅助express解析post请求

+ express -cookie

+ express-session

------

