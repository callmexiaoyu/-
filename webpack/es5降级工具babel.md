# babel

[TOC]

***

## .babelrc

json文件

```json
{
    "presets": ["env", "stage-0"],
    "plugins": ["transform-runtime"]
}
```

***

webpack.config.js与webpack.common.js

```js
module: {

        rules: [
            
            {
                //babel降级
                test: /\.js$/, //匹配文件夹中后缀名是 .js的文件（注意这里不能加 引号）
                loader: "babel-loader", //对匹配的 js文件用 babel来编译
                exclude: /node_modules/ //排除 node_modules 中的js文件（注意这里不能加引号）
            }
        ]
    }
```

