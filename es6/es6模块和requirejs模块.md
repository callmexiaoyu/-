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


  