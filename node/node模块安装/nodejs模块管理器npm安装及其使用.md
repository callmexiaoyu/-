# nodejs模块管理器npm安装及其使用
***
[TOC]
***
## npm模块命令
1. `npm install 模块名@版本号`
    本地安装，在当前目录路径下，生成node_moudles文件下内含下载的模块

2. `npm install 模块名@版本号 -g`
    全局安装，一些特殊的包需要cmd控制台输入命令执行如lessc、gulp等

3. `npm install 模块名@版本号 -save`
    本地安装，同时在目录package.json写入`线上环境`项目模块依赖
  ```json
  "Dependencies": {
   "art-template": "^4.13.2",
   "bootstrap": "^4.3.1"
   }
  ```

4. `npm install 模块名@版本号 -save-dev`
    本地安装，同时在目录package.json写入`生产环境`项目模块依赖
  ```json
    "devDependencies": {
   "browser-sync": "^2.26.3",
   "gulp": "^3.9.0",
   "gulp-autoprefixer": "^6.0.0",
   "gulp-less": "^4.0.1",
   "gulp-uglify": "^3.0.2",
   "gulp.spritesmith": "^6.9.0"
    }
  ```

5. `npm uninstall 模块名@版本号 -g/-save/-save-dev`

6. `npm init`
    在当前目录创建一个package.json（包描述说明文件）

7. `npm

8. `npm init -y`
    快速创建package.json

9. `npm install`
    下载当前目录的package.json记录的所有包依赖项

10. `npm help`
    查看命令帮助

11. `npm install --global npm`
    升级npm

12. `npm update 模块名`
    更新模块

13. `npm root -g`
    全局安装模块所在路径，如gulp

14. `npm list`
    当前路径下安装的模块，以结构形式列出安装的模块以及依赖的模块

15. `npm view 模块名 engines`

    当前模块依赖的node最低版本
***
## npm基础配置
1. `npm config get cache`
    模块缓存路径
2. `npm config set cache “C:\developer\nvm\node_npm_cach`
    设置模块缓存路径便于管理
3. `npm config get prefix`
    模块全局安装路径
4. `npm config set registry https://registry.npm.taobao.org`
    设置npm模块下载镜像路径，用于提高包下载速度

## nrm包下载源管理工具

+ `npm install nrm -g`

+ `nrm ls`

  源列表

+ `nrm use 源`

  切换源

+ `npm config get registry`

  查看当前源地址

+ `nrm test 源`

  测试目标下载源速度 

似乎没卵用，淘宝最快