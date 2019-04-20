# es语法降级babel

[TOC]

***

+ 2007年发布es5，2015年发布es6，也称作es2015，伺候每年新增版本es2016，且新版本兼容旧版本。

## babel降级工具

+ 语法转椅器

  js最新语法转译器

  `babel-preset-env、babel-preset-es2015、babel-preset-es2016、babel-preset-es2017、babel-preset-latest`

+ 补丁转译器

  js的API和全局对象

  `babel-profill`

+ jsx与flow插件

  jsx语法

  `babel-preset-react`

***

#### 转译规则

+ `babel-preset-env`

  根据配置将语法降级为es5，预先降级

+ `babel-preset-es2015`

  转译es6到es5

+ `babel-preset-latest`

  已经废除，使用babel-prset-env

+ `babel-prset-react`

## webpack使用babel

#### 安装插件

第一套,babel转译工具额外插件工具

```
npm intall babel-loader@7 babel-core install babel-plugin-transform-runtime -D
```

+ babel-loader@7

  webpack处理babel的相关配置文件

  最新8.x报错，对应版本babel-core的6.x版本

+ babel-core

  babel核心包

+ babel-plugin-transform-runtime
  + 多个文件重复引用相同helpers（帮助函数）> 提取运行时
  + 新API方法全局污染 > 局部引入

第二套，babel转译规则，.babelrc中presets规则

```
npm babel-preset-env babel-preset-stage-0 -D
```

+ babel-preset-env

  转译规则，将es5+的语法转译到es5，需要配置

+ babel-preset-stage-0

  es7不同阶段语法转译规则

#### 配置

+ 根目录创建.babelrc文件json，不能添加注释，配置转译预设

  ```json
  {
      //转译规则babel-preset-x，两个babel-preset-env、babel-preset-stage-0
      "presets": ["env", "stage-0"],
      //转译过程使用的插件babel-plugin-transfrom-runtime
      "plugins": ["transform-runtime"]
  }
  ```

+ webpack.config.js相关配置

  ```js
  //babel降级
  {
      test: /\.js$/, //匹配文件夹中后缀名是 .js的文件（注意这里不能加 引号）
      loader: "babel-loader", //对匹配的 js文件用 babel来编译
      exclude: /node_modules/ //排除 node_modules 中的js文件（注意这里不能加引号）
  }
  ```

  

`告诉webpack打包时，一旦匹配到.js文件就使用babel-loader进行处理，然后babel-loader调用babel-core的api使用bable-preset-env与babel-preset-stage-0的规则进行转码。`