# es6模块和requirejs模块

[TOC]

***

## 浏览器端实现模块化

浏览器端实现类似node的模块管理

+ 服务器开启

  `不开启报错`

+ 浏览器的script脚本引入main.js

  告诉浏览器这是一个模块文件

  `不开启报错`

  ```html
  <script type="module" src="/public/js/main.js"></script>
  ```

+ main.js中引入加载其他模块

  加载模块a.js

  ```js
  import {fna} from "./a.js";
  
  console.log("浏览器加载主文件")
  
  console.log(fna);
  
  ```

+ a.js中加载模块b.js

  ```js
  //从相对路径加载模块中的变量
  import {fnb} from "./b.js";
  
  console.log("浏览器加载a.js模块")
  
  var fna = fnb;
  
  export {fna}
  ```

+ b.js

  ```js
  function fnb(){
  	console.log("来自b.js文件的方法一");
  };
  
  console.log("浏览器加载b.js模块")
  
  //将变量导出
  export {fnb};
  ```

`类似node的require模块管理，加载其他模块使用相对路径，引入的模块相对自身路径`

+ 向服务器根据请求相对应的模块js文件
+ js模块文件的变量必须声明

匿名函数写法

```js
//文件xxx.js
export default fnb;//输出


//文件xxxx.js
import 任意名 form "xxx/xxx.js"
//使用任意名指向 fnb
```



***

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
  
  //加载相对模块
  define(["b/b"],function(b){
  
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

  