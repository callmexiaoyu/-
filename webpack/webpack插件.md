# webpack

[TOC]

***

## 静态资源

+ js

  .js、.jsx、.coffee、.ts、

+ css

  .css、.less、.sass、.scss、

+ img

  .jpg、.png、.svg、.gif、

+ fonts

  .svg、.ttf、.eot、.woff、

+ 模板文件

  .art、.ejs、.jade、.vue、

***

## 插件

### webpack-dev-server 热更新模块文件

#### 原理

+ websocket协议：类似客户端ajax轮询，但ajax轮询浪费资源，实现服务端主动发起
+ `webpack-dev-server：添加基于websocket协议的小型静态资源服务器，当修改模块文件时，重新编译打包模块，放到内存中，检测到main.js的hash值变化，通过自带的websocket协议，通知客户端重载刷新`


保存动态刷新页面

```
npm i webpack-dev-server --save-dev
```

package.json配置命令

```json
"scripts": {
    "dev": "webpack --mode development",
    "build": "webpack --mode production",
    "start": "webpack-dev-server"
}
```

+ 模块化，存在index.js作为模块化的入口，添加对其他模块的依赖关系

+ webpack命令遵从webpack.config.js，生成一个合成的main.js脚本，所有依赖模块压缩打包
+ 工程化静态页面制作实时渲染，webpack-dev-server，当修改

+ webpack-dev-server，`开启小型服务器，采用websocket协议，服务端修改模块文件，触发页面重载，生成的main.js保存在内存中，不替换到dist/main.js`
+ `html页面内联到根目录下的main.js而不是../dist/main.js`，修改完毕后再通过webpack命令覆盖的磁盘中，这样做快，而根目录基于webpack.config.js为基准
+ 修改模块保存，自动刷新

webpack.config.js相关配置，通过webpack-dev-server实现websocket通信协议，重载页面

```js
  devServer: {
    host: 'localhost', //服务器的ip地址
    port: 3000, //端口
    open: true, //自动打开页面
    //网站的首页，默认项目根目录列表
    //默认dev-server为webpack.config.js所在位置为服务器根目录，修改为index.html所在路径
    contentBase: path.join(__dirname, "views"),
    //当编辑模块时，默认重新完整合成，开启部分更新内存模块更新
    hot: true

  }
```

***

### html-webpack-plugin

webpack-dev-server实现js的热更新，当服务端html修改，则无法触发html页面重载，需要手动触发浏览器重载，触发请求，获取最新的html页面

+ 页面无需添加对模块入口文件的引用
+ 添加html到内存中

```
npm install --save-dev html-webpack-plugin
```

```js
const path = require('path'); // 使用node.js中的path模块

//webpack插件

//模块文件热更新
var webpack = require('webpack');

//html页面热更新
var HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  // 
  mode: "development", // 设置模式为development,("production" | "development" | "none")
  entry: './src/index.js', // 根目录./为webpack.config.js所在位置
  output: {
    filename: 'main.js', // 打包输出文件名
    path: path.resolve(__dirname, 'dist') // 打包输出目录
  },


  //webpack-dev-server配置，小型服务器
  devServer: {
    host: 'localhost', //服务器的ip地址
    port: 3000, //端口
    open: true, //自动打开页面
    //网站的首页，默认项目根目录列表
    contentBase: path.join(__dirname, "views"),
    //保存部分更新内存生成的main.js，而不是重新合成，提高速度，否则模块文件庞大，编译事件很长
    hot: true
  },

  //插件
  plugins: [
    //似乎没必要，hot直接代替
    new webpack.HotModuleReplacementPlugin(),

    // 实现html页面热更新，html也保存在内存中
    new HtmlWebpackPlugin({
      template: path.join(__dirname, "./views/index.html"), //指定模板页面来源。内存
      filename: "index.html"
    }),
  ]
}
```

`html无需添加对模块的引用，自动添加了srcipt标签`

***

###  css打包

```
npm install css-loader style-loader --save-dev
```

webpack.config.js配置

```js
module: {
    //css解析规则
    rules: [{
      test: /\.css$/, // 查找.css文件
      // 解析style-loader，将css插入html中，css-loader将css插入js中
      loaders: ['style-loader', 'css-loader'] // 从右向左解析
    }]
  }
```

编译时动态的添加创建style标签，index.html未编译无需添加对css以及js的引用，通过npm run dev合成目标html

在index.js中通过import引入，基于index.js的位置

```js
// require("./assets/less/index.less");
// 两者混用
import "./assets/less/index.less";
```

```less
@import "./demo.less";
```

***

## less预处理器

```
npm install --save-dev less
npm install --save-dev less-loader
```

webpack.config.js

```js
rules: [{
      //解析less
      test: /\.less$/,
      use: [{
        loader: "style-loader" // creates style nodes from JS strings
      }, {
        loader: "css-loader" // translates CSS into CommonJS
      }, {
        loader: "less-loader" // compiles Less to CSS
      }]
    }]
```

index.js引用

```js
//导入less
// require("./assets/less/index.less");
// 两者混用
import "./assets/less/index.less";
```

```css
@import "./demo.css";
```

