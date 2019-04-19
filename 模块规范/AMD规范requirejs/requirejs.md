## requirejs

[TOC]

***

```js
//jquery报错，提供不提供es6支持的导出出口，jquery不是es6规范
// import {jquery} from "../lib/jquery/dist/jquery.min";

//requirejs采用来加载jquery方法

//方法一

//配置
require.config({
  //baseUrl的相对路径指的是基于服务器app.js运行时所在目录下寻找
  //./相对路径使用
  baseUrl: './assets',
  paths: {
    jquery: 'lib/jquery/dist/jquery', //不能添加.js后缀名
    bootstrap: "lib/bootstrap/dist/js/bootstrap.min",
    popper: "js/popper"
  },
  shim: {
    jquery: {
        exports: 'jquery'
    },
    popper: {
        exports: 'popper'
    },
    bootstrap: {
        deps: ['jquery',"popper"]
    },
  }
});

require(["jquery"], function ($) {

  console.log(arguments);
  console.log($);
  $("li:odd").css({
    "backgroundColor": "pink"
  });
  console.log(jQuery);

  require(['bootstrap'], function (bs) {

  console.log(bs);
    
  });

});
//外部使用报错
// console.log($("li").eq(0));


//方法二

//基于本文件main.js所在路径下寻找
// requirejs(["../lib/jquery/dist/jquery.min"], function () {
//   //$或jquery直接作为全局对象使用
//   console.log($("li").eq(0));
//   console.log(jQuery("li").eq(0));
// });
// console.log($("li").eq(0));

//方法三
// define(["../lib/jquery/dist/jquery.min"], function () {
//   //这里作为jQuery使用
//   console.log(jQuery("li").eq(0));
// });


//存在请求到模块文件，但找不到引用
```



```js

(function() {
     require.config({
          baseUrl : './',
          paths : {
               jquery : 'assets/js/jquery/jquery.min',
               bootstrap : 'assets/bootstrap/js/bootstrap.min'
          },
          shim : {
               bootstrap : {
                    deps : [ 'jquery' ],
                    exports : 'bootstrap'
               }
          }
         
     });
     require(['bootstrap' ], function() {
          console.log(all loaded);
     });
```

