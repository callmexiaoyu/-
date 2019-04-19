# webpack使用

[TOC]

***

## 安装

+ 全局安装最新版本webpack，4.x以上

  ```
  npm install webpack -g
  ```

+ 全局安装webpack-cli

  ```
  npm install webpack -g
  ```

+ 本地安装

  webpack

  ```
  npm install webpack -save-dev
  或简写
  npm install webpack -D
  ```

  webpack-cli

  ```
  npm install webpack-cli -save-dev
  或简写
  npm install webpack-cli -D
  ```

***

## 流程

1. `模式配置`

    ```
   webpack --mode development/production
   ```

2. `项目根目录创建src/index.js`

3. 打包命令

   1. ```
      webpack --mode development
      ```

   2. package.json文件scripts添加

      ```json
          "dev": "webpack --mode development",
          "build": "webpack --mode production"
      ```

      项目根目录下运行

      ```
      npm run dev/build
      ```

      自动打包在dist文件下生成main.js

***

## 配置文件webpack.config.js

类似gulp的gulpfile.js的打包配置

项目根目录创建配置文件webpack.config.js，对打包进行说明

```js
const path = require('path'); // 使用node.js中的path模块

module.exports = {
  mode: "development",        // 设置模式为development,("production" | "development" | "none")
  entry: './src/index.js',    // 入口文件
  output: {
    filename: 'main.js',    // 打包输出文件名
    path: path.resolve(__dirname, 'dist')    // 打包输出目录
  }
};
```

根目录下执行

```
webpack-cli --config webpack.config.js
```

***

## webpack的模块化设计

+ webpack将模块化问价压缩合成生成一个main.js来进行模块，而 requirejs模块化，是通过请求相对应的模块化js文件，服务器开放js脚本，客户端require根据main.js，向服务端分别发起对应路径请求js，合成一个js文件，而原生的es6模块化处理方式存在兼容性问题

+ node模块依赖commonjs规范