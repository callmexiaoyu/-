# requirejs模块路径

[TOC]

***

## 依赖关系配置

+ 模块相互依赖，且各个模块存放路径位置不同，一种是模块内部对依赖的模块添加依赖关系，这样main.js中直接引用，缺点时，一旦移动某一个模块，依赖他的模块的路径也需要重新配置
+ 模块内部对模块的依赖关系不进行配置，而在main.js中引用模块时，设置模块依赖关系，该方法在当模块文件移动时，在main.js中统一处理

## require引入模块路径

+ 主目录

  + 当配置了引入main.js的data-main路径时，主目录，为data-main的所在根目录

    ```html
    <script type="text/javascript" src='https://cdn.bootcss.com/require.js/2.3.6/require.js' data-main="public/js/main.js"></script>
    ```

    此时baseUrl设置为

    ```js
    require.config({
    	baseUrl:"./public/js",
    	paths:{
    		a:"a",
    		b:"b/b"
    	}
    });
    ```
    主目录为public文件夹所在目录

  + 不设置data-main

    主目录基于引用requirejs的html所在的html的根目录

    ```html
        <script type="text/javascript" src='https://cdn.bootcss.com/require.js/2.3.6/require.js'></script>
    
        <!-- 未配置data-main属性 -->
        <!-- requirejs的模块请求路径基于引入requirejs的html所在目录的根目录 -->
        <script type="text/javascript">
            require.config({
                baseUrl: "public/js",
                paths: {
                    a: "a",
                    b: "b/b"
                }
            });
            require(["b"], function (b) {
                console.log(b);
            });
        </script>
    ```

  + 以上引入的模块不添加.js后缀名，url请求路径采用相对路径

+ 引入模块不添加.js后缀名

  + 根据baseUrl设置路径寻找，基于主目录

    ```js
    require.config({
        baseUrl: "public/js",
        paths: {
            a: "a",
            b: "b/b"
        }
    });
    require(["b"], function (b) {
        console.log(b);
    });
    ```

+ 添加.js后缀名

  + 此时模块加载baseUrl设置无效

    ```js
    require.config({
          baseUrl: "public/js",
          paths: {
              a: "a",
              b: "b/b"
          }
      });
      require(["b.js"], function (b) {
          console.log(b);
      });  
    ```

  + 在根目录下寻找b.js

+ 互相转换

***

## define引入依赖模块路径

+ 方法一，模块依赖关系可以写入模块内部，直接引用

+ 方法二，依赖关系写在引用的main.js，配置依赖关系

+ 模块内部difine路径设置，也受require.config配置影响

  ```js
  //不添加.js后缀名
  //不添加依赖模块的路径
  // define(["b"],function(){
  // 	return {
  // 		fa
  // 	}
  // });
  
  //不添加.js后缀名
  //添加依赖模块的路径
  // define(["public/js/b/b"],function(){
  // 	return {
  // 		fa
  // 	}
  // });
  
  //添加后缀名
  define(["public/js/b/b.js"],function(){
  	return {
  		fa
  	}
  });
  ```

  
