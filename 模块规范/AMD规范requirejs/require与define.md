# require和define

[TOC]

***

## 模块管理思路

+ 将js脚本模块化，形成多个js模块文件，每个模块存放地址不同，且模块之间相互引用，main.js作为前端模块入口文件

+ 模块之间的依赖关系处理
  + 模块内部处理依赖关系，main.js直接引用
  + 模块内部不处理依赖关系，需要main.js处理模块依赖关系，引用时加载依赖模块

***

## require

```html
    <!-- 未配置data-main属性 -->
    <!-- requirejs的模块请求路径基于引入requirejs的html所在目录的根目录 -->
    <script type="text/javascript">
        // a模块依赖b模块，且a模块内部未添加依赖关系
        // require(["public/js/a.js"], function (a) {
        //     console.log(a);
        // });


        // 处理模块依赖关系，通过嵌套
        // require(["public/js/b/b.js"], function (b) {
        //     console.log(b);
        //     require(["public/js/a.js"], function (a) {
        //         console.log(a);
        //     });
        // });



        // 由于config统一配置了所有路径前缀，所有模块相互依赖关系不写.js后缀
        // 写法一
        // require.config({
        //     // 无路径前缀
        //     baseUrl: "./",
        //     paths: {
        //         a: "public/js/a",
        //         b: "public/js/b/b"
        //     }
        // });
        // 写法二
        // require.config({
        //     // 存在路径前缀
        //     baseUrl: "public/js",
        //     paths: {
        //         a: "a",
        //         b: "b/b"
        //     }
        // });
        // // a模块设置了对b模块的依赖，直接引用，不能添加.js后缀
        // require(["a"], function (a) {
        //     console.log(a);
        // });


        //a模块内部未对b模块依赖关系处理
        // require(["b"], function (b) {
        //     console.log(b);
        //     require(["a"], function (a) {
        //         console.log(a);
        //     });
        // });


        //模块内部define模块添加后缀名.js，但不设置模块的具体路径，此时baseUrl失效，直接在主目录下寻找b.js文件
        //模块文件的前缀无法插进模块引用路径中
    </script>
```



***

## define

+ 可以引入模块，return模块的出口对象，每个模块内部都存在该方法，将模块导出

+ 模块本身也会依赖其他模块，其依赖模块设置路径或

  a.js

  ```
  
  console.log("已加载a.js模块文件")
  
  //a.js依赖b.js
  
  //该方法执行必须先引用b.js模块
  function fa(arg){
  
  	console.log("调用方法fa");
  
  	var str = fb(arg);	
  	
  	str = "fa已经处理" + str;
  	return str;
  };
  
  //define不设置依赖模块路径，由引用模块时处理
  // define(function(){
  // 	return {
  // 		fa
  // 	}
  // });
  
  
  // define设置依赖模块，需要main.js统一配置路径前缀，依赖模块b
  // define(["b"],function(){
  // 	return {
  // 		fa
  // 	}
  // });
  
  
  // define设置依赖模块文件的具体路径，直接调用a模块即可
  // define(["public/js/b/b.js"],function(){
  // 	return {
  // 		fa
  // 	}
  // });
  
  ```

  b.js

  ```js
  
  console.log("已加载b.js模块文件")
  
  function fb(arg){
  	
  	console.log("调用方法fb");
  	
  	var str = "fb已经处理" + arg;
  	return str;
  };
  
  //导出fb
  define(function(){
  	return fb;
  });
  
  ```

  