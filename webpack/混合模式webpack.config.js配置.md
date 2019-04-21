# webpack.config.js

[TOC]

***

## package.json修改

```
    "dev": "set NODE_ENV=development&webpack-dev-server",
    "pro": "set NODE_ENV=production&webpack"
```

## webpack.config.js

```js
//通过package.json修改命令得到不同命令在webpack.config.js单个文件调用不同模式
const devMode = process.env.NODE_ENV === "development";

const path = require('path'); // 使用node.js中的path模块
console.log("混合模式");

console.log(process.env.NODE_ENV);


//webpack插件

//模块文件热更新
var webpack = require('webpack');

//html添加script引用等,虚拟内存中html
var HtmlWebpackPlugin = require('html-webpack-plugin');

//分离css，默认css添加到js文件中
var MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  // webpack打包时的路径处理
  mode: process.env.NODE_ENV, // 设置模式为development,("production" | "development" | "none"),写在命令中
  entry: './src/index.js', // 根目录./为webpack.config.js所在位置，入口js路径所在
  output: {
    filename: 'main.js', // 打包输出文件名
    path: path.resolve(__dirname, 'dist') // 打包输出目录，该路径基准路径
  },


  //webpack-dev-server配置，小型服务器
  devServer: {
    host: 'localhost', //服务器的ip地址
    port: 3000, //端口
    open: true, //自动打开页面
    //默认项目服务器打开路径，根路径展示目录列表,自动定位到实际路径index.html,
    // HtmlWebpackPlugin能替换掉该功能
    contentBase: path.join(__dirname, "views"),
    //保存部分更新内存生成的main.js，而不是重新合成，提高速度，否则模块文件庞大，编译事件很长
    hot: true
  },

  //插件
  plugins: [
    //开启热更新
    new webpack.HotModuleReplacementPlugin(),

    //添加html引用等
    new HtmlWebpackPlugin({
      template: path.join(__dirname, "./views/index.html"), //指定模板页面来源路径
      filename: "index.html"
    }),

    //创建全局变量，区分开发和生产模式
    new webpack.DefinePlugin({
      // 'process.env.NODE_ENV': JSON.stringify('development');
      // 这里必须通过json.stringifty，输出是""development"",全局变量为字符串
      'process.env.NODE_ENV': JSON.stringify(process.env.NODE_ENV)
    }),

    //剥离css
    new MiniCssExtractPlugin({
      filename: '[name].css',
      chunkFilename: '[id].css',
    }),
  ],

  //忽视模块监听，修改不刷新，webpack-dev-server默认开启监听
  watchOptions: {
    ignored: /node_modules/
  },


  module: {

    rules: [
      // {
      //   //css解析
      //   test: /\.css$/, // 查找.css文件
      //   // 解析
      //   use: [{
      //       loader: devMode ? "style-loader" : MiniCssExtractPlugin.loader,
      //       options: {
      //         // you can specify a publicPath here
      //         // by default it uses publicPath in webpackOptions.output
      //         // publicPath: 'css/',
      //         hmr: process.env.NODE_ENV === 'development',
      //       },
      //     },
      //     // {
      //     //   loader: "style-loader" // creates style nodes from JS strings
      //     // }, 
      //     {
      //       loader: "css-loader" // translates CSS into CommonJS
      //     }
      //   ],
      //   // 从右向左解析
      // },
      {
        //解析less
        test: /\.(le|c)ss$/,
        use: [{
            loader: devMode ? "style-loader" : MiniCssExtractPlugin.loader,
            options: {
              // you can specify a publicPath here
              // by default it uses publicPath in webpackOptions.output
              // publicPath: 'css/',
              hmr: process.env.NODE_ENV === 'development',
            },
          },
          // {
          //   loader: "style-loader" // creates style nodes from JS strings
          // }, 
          {
            loader: "css-loader" // translates CSS into CommonJS
          }, {
            loader: "less-loader" // compiles Less to CSS
          }
        ]
      },
      {
        //图片处理处理
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

