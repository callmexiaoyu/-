# AMD规范requirejs

[TOC]

***

同步加载模块由于网速问题导致，某个模块无法加载导致假死，异步回调

## requirejs

require.js采用AMD模块规范，模块文件必须同样采用AMD规范

+ index.html

  ```html
  <script type="text/javascript">
  
  //require模块加载配置
  // require.config({
  //     //文件不能添加js后缀名
  //     //用变量名main指代相对应的js模块
  //     paths: {
  //         "main": "public/js/main",
  //     }
  // });
  
  
  // require.config({
  //     //在相对index.html目录下加载对应的js文件
  //     baseUrl:"public/js"
  // });
  
  // //加载main.js模块文件，不能添加js后缀名
  // require(['main'], function() {
  // 	//加载模块文件的回调函数
  
  //     console.log(arguments)
  // });
  
  </script>
  
  <script type="text/javascript" src='https://cdn.bootcss.com/require.js/2.3.6/require.js' data-main="public/js/main.js"></script>
  ```

  加载主文件

+ main.js

  ```js
  function fmain(){
  	console.log("来自main.js模块的方法fmain");
  };
  
  //引入模块相对路径的模块，不能加.js后缀名
  //回调函数的参数拿到加载模块的对象
  define(["a"],function(a){
      
      /*~~~~*/
      
      //模块导出接口
  	return {
  		a,
  		fmain
  	};
  });
  
  ```

+ a.js

  ```js
  
  console.log("加载a.js模块文件")
  
  function fa(){
  	console.log("来自a.js模块的方法fa")
  };
  
  //异步加载模块
  define(["b/b"],function(b){
      //加载完毕，回调函数获取加载导出的对象
  	return {
  		b,
  		fa
  	}
  });
  
  ```

+ b.js

  ```js
  
  console.log("加载b.js模块文件")
  
  function fb(){
  	console.log("来自b.js的方法fb")
  }
  
  //没加载其他模块
  define(function(){
  	return fb;
  });
  
  ```

***

## 

