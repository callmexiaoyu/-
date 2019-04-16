# md5

[TOC]

***

### blueimp-md5

```js
var md5 = require("../node_modules/blueimp-md5/js/md5.js");

var num = 123456789;

//加密两次
//md5只能正向加密，难以解密，相同原文md5加密后得到唯一的hash值，比对hash判断两次输入是否一致
var retnum = md5(md5(num))
console.log(retnum);


```



