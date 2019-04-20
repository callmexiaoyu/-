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

  .svg、.ttf、.eot、.woff、woff2

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

### less预处理器

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

***

### scss预处理器

***

### url

处理静态资源路径，开启虚拟路径时，而css中的url指向实际硬盘中路径，需要置换为虚拟内存中的路径，打包时通过

```
npm install url-loader --save-dev
npm install file-loader --save-dev
```

```js
{
        //less中图片处理处理
        test: /\.(png|jpg|gif|jpeg)$/i,
        use: [{
          loader: 'url-loader',
          options: {
            limit: 8192, //限制大小，超过图片小于base64编码
            outputPath: "img/",  //指定打包输出图片存放在dist中的路径，需要file-loader模块
        name: '[hash:8]-[name].[ext]' //默认图片名字hash值，修改图片名为原名原格式
          }
        }]
      }
```

以上处理图片打包后dist的路径

***

### 字体图标url

webpack.config.js配置

```js
{
        //字体图标，bootstrap字体图标
        test: /\.(ttf|svg|eot|woff|woff2)$/i,
        use: [{
          loader: 'url-loader',
        }]
      }
```

样式表导入

```js
//导入bootstrap样式表
// import "../node_modules/bootstrap/dist/css/bootstrap.css";
import "bootstrap/dist/css/bootstrap.css";//可简写
```

### 丑化js

自带压缩混淆js以及去除注释js

```
npm install uglifyjs-webpack-plugin --save-dev
```

```js
const UglifyJsPlugin = require('uglifyjs-webpack-plugin');

optimization: {
    minimizer: [
        new UglifyJsPlugin({
            test: /\.js(\?.*)?$/i,
        }),
    ],
},
```

***

### es6语法降级babel工具库

#### 安装插件

第一套,babel转译工具额外插件工具

```
npm intall babel-loader@7 babel-core install babel-plugin-transform-runtime -D
```

- babel-loader@7

  webpack处理babel的相关配置文件

  最新8.x报错，对应版本babel-core的6.x版本

- babel-core

  babel核心包

- babel-plugin-transform-runtime

  - 多个文件重复引用相同helpers（帮助函数）> 提取运行时
  - 新API方法全局污染 > 局部引入

第二套，babel转译规则，.babelrc中presets规则

```
npm babel-preset-env babel-preset-stage-0 -D
```

- babel-preset-env

  转译规则，将es5+的语法转译到es5，需要配置

- babel-preset-stage-0

  es7不同阶段语法转译规则

#### 文件配置

- 根目录创建.babelrc文件json，不能添加注释，配置转译预设

  ```json
  {
      //转译规则babel-preset-x，两个babel-preset-env、babel-preset-stage-0
      "presets": ["env", "stage-0"],
      //转译过程使用的插件babel-plugin-transfrom-runtime
      "plugins": ["transform-runtime"]
  }
  ```

- webpack.config.js相关配置

  ```js
  //babel降级
  {
      test: /\.js$/, //匹配文件夹中后缀名是 .js的文件（注意这里不能加 引号）
      loader: "babel-loader", //对匹配的 js文件用 babel来编译
      exclude: /node_modules/ //排除 node_modules 中的js文件（注意这里不能加引号）
  }
  ```


`告诉webpack打包时，一旦匹配到.js文件就使用babel-loader进行处理，然后babel-loader调用babel-core的api使用bable-preset-env与babel-preset-stage-0的规则进行转码，额外通过babel-plugin-transform-runtime辅助插件。`

***

### css分离插件

webpack 3.x使用extract-text-webpack-plugin

webpack 4.x使用mini-css-extract-plugin

```
npm install mini-css-extract-plugin -D
```

```
npm install mini-css-extract-plugin -D
```

```js
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
plugins: [
        // 创建全局变量，区分开发和生产模式
        new webpack.DefinePlugin({
            'process.env.NODE_ENV': JSON.stringify('production')
        }),

        //剥离css
        new MiniCssExtractPlugin({
            filename: '[name].css',
            chunkFilename: '[id].css',
        }),
    ],
    
module: {
        rules: [{
                //css解析
                test: /\.css$/, // 查找.css文件
                // 解析
                use: [{
                    loader: MiniCssExtractPlugin.loader,
                }, {
                    loader: 'css-loader'
                }], // 从右向左解析
                // options: {
                //     // you can specify a publicPath here
                //     // by default it uses publicPath in webpackOptions.output
                //     publicPath: 'css/',
                //     hmr: process.env.NODE_ENV === 'development',
                // },
            },
            {
                //解析less
                test: /\.less$/,
                use: [{
                        loader: MiniCssExtractPlugin.loader, // 剥离
                    },
                    {
                        loader: "css-loader" // translates CSS into CommonJS
                    },
                    {
                        loader: "less-loader" // compiles Less to CSS
                    }
                ]
            },
        ]
    }
```

