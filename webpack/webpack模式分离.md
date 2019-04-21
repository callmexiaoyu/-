# webpack模式分离

[TOC]

***

## 安装融合模块

```
npm install webpack-merge -D
```

## package.json修改npm启动命令

+ --progress

  用于在终端与浏览器控制台输出编译进度

+ --colors

  colors打包信息带有颜色显示

+ --watch

  监听

```json
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack --mode development",
    "pro": "webpack --mode production",
    "build": "webpack --config webpack.prod.js",
    "start": "webpack-dev-server --progress --config webpack.dev.js"
  },
```

## 创建webpack三个模式的配置文件根目录中

#### webpack.common.js

通用配置文件

```js

const path = require('path'); // 使用node.js中的path模块
console.log("公共配置文件");

//html添加script引用等,虚拟内存中html，去除了html的相关引用
var HtmlWebpackPlugin = require('html-webpack-plugin');

//导出公共配置对象
module.exports = {

    entry: './src/index.js', // 根目录./为webpack.config.js所在位置，入口js路径所在
    output: {
        filename: 'main.js', // 打包输出文件名
        path: path.resolve(__dirname, 'dist') // 打包输出目录，该路径基准路径
    },

    //插件
    plugins: [

        //添加html虚拟化内存插件
        new HtmlWebpackPlugin({
            template: path.join(__dirname, "./views/index.html"), //指定模板页面来源路径
            filename: "index.html"
        }),
    ],

    module: {

        rules: [
            
            {
                //图片处理url路径处理
                test: /\.(png|jpg|gif|jpeg)$/i,
                use: [{
                    loader: 'url-loader',
                    options: {
                        limit: 8192, //限制大小，超过图片小于base64编码
                        outputPath: "img/", //指定打包输出图片存放在dist中的路径，需要file-loader模块，相对于出口路径
                        // publicPath: "img/", // 指定内存中图片的虚拟路径，默认相对于webpack.config.js路径，没必要统一
                        name: '[hash:8]-[name].[ext]' //默认图片名字hash值，修改图片名为原名原格式
                    }
                }]
            },
            {
                //字体图标，bootstrap字体图标
                test: /\.(ttf|svg|eot|woff|woff2)$/i,
                use: [{
                    loader: 'url-loader',
                }]
            },
            {
                //babel降级
                test: /\.js$/, //匹配文件夹中后缀名是 .js的文件（注意这里不能加 引号）
                loader: "babel-loader", //对匹配的 js文件用 babel来编译
                exclude: /node_modules/ //排除 node_modules 中的js文件（注意这里不能加引号）
            }
        ]
    }
}
```

#### webpack.dev.js

```js
const path = require('path'); // 使用node.js中的path模块

//融合公共配置模块
const merge = require('webpack-merge');
//引入公共配置js文件
const common = require('./webpack.common.js');
console.log("开发模式");

//window无法在package.json通过以下配置获取模式区分的全局变量
//"start": "NODE_ENV=development webpack-dev-server --config webpack.dev.js"
console.log(process.env.NODE_ENV);//undefined
//需要额外npm install --save-dev cross-env
//"start": "cross-env NODE_ENV=development webpack-dev-server --config webpack.dev.js"


//提取热更新
//创建全局变量区分开发和生产模式
var webpack = require('webpack');

//导出配置对象
module.exports = merge(common, {
    //定义开发模式
    mode: "development",
    //开启小型websocket服务器
    devServer: {
        host: 'localhost', //服务器的ip地址
        port: 3000, //端口
        open: true, //自动打开页面
        //默认项目服务器打开路径，根路径展示目录列表,自动定位到实际路径index.html,
        // HtmlWebpackPlugin能替换掉该功能，使页面定位到根目录下
        contentBase: path.join(__dirname, "views"),
        //保存部分更新内存生成的main.js，而不是重新合成，提高速度，否则模块文件庞大，编译事件很长
        hot: true
    },

    plugins: [

        // 热替换插件
        new webpack.HotModuleReplacementPlugin(),

        //创建全局变量，区分开发和生产模式，用于在模块文件中判断通过process.env.NODE_ENV判断
        new webpack.DefinePlugin({
            'process.env.NODE_ENV': JSON.stringify('development')
        }),

    ],
    //非js通过loader解析打包
    module: {
        rules: [{
                //css解析
                test: /\.css$/, // 查找.css文件
                // 解析
                use: [{
                    
                    loader: 'style-loader',
                }, {
                    
                    loader: 'css-loader'
                }], 
            },
            {
                //解析less
                test: /\.less$/,
                use: [{
                        loader: "style-loader" // creates style nodes from JS strings
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
});
```

#### webpack.prod.js

```js
const path = require('path'); // 使用node.js中的path模块

//融合公共配置模块
const merge = require('webpack-merge');
//引入公共配置js文件
const common = require('./webpack.common.js');

console.log("生产模式");

//生产模式剥离css，单独文件
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

//创建全局变量区分开发和生产模式
const webpack = require('webpack');

//压缩混淆删去注释，似乎webpack4.x自带
// const UglifyJsPlugin = require('uglifyjs-webpack-plugin');

module.exports = merge(common, {
    //定义生产模式
    mode: "production", //设置模式为development,("production" | "development" | "none"),写在命令中

    plugins: [
        // 创建全局变量，区分开发和生产模式
        new webpack.DefinePlugin({
            'process.env.NODE_ENV': JSON.stringify('production')
        }),

        //剥离css插件
        new MiniCssExtractPlugin({
            filename: '[name].css',
            chunkFilename: '[id].css',
        }),
    ],

    //压缩js，似乎自带
    // optimization: {
    //     minimizer: [
    //         new UglifyJsPlugin({
    //             test: /\.js(\?.*)?$/i,
    //         }),
    //     ],
    // },
    module: {
        rules: [{
                //css解析
                test: /\.css$/, // 查找.css文件
                // 解析
                use: [{
                    //style-loader页面添加style node节点改为
                    loader: MiniCssExtractPlugin.loader,
                }, {
                    loader: 'css-loader'
                }], // 从右向左解析
                // 这里由于配置文件无法读取模式，主动分离模式配置文件
                // options: {
                //     // you can specify a publicPath here
                //     // by default it uses publicPath in webpackOptions.output
                //     publicPath: 'css/',
                //热更新模块也分到开发模式配置文件中
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
});
```