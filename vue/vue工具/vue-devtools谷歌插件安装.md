# vue-devtoold安装

[TOC]
***
## 手动安装
1. 下载插件包并解压<https://github.com/vuejs/vue-devtools>
2. 在该路径下执行`npm install`安装插件依赖的包

  >C:\Users\Administrator\Desktop\前端资料\chrome插件\vue-devtools-dev

3. 同时在该路径下`npm run build`
    **注意** node版本过低安装出错，安装最新node

  `nvm install 10.0.0`

  `nvm use 10.0.0`
4. >C:\Users\Administrator\Desktop\前端资料\chrome插件\vue-devtools-			dev\shells\chrome\manifest.json

将值由fasle改为false
```js
"persistent": true
```

5. 打开谷歌浏览器

   设置 > 扩展程序 > 加载已解压的扩展程序

   选择chrome文件夹

   > C:\Users\Administrator\Desktop\前端资料\chrome插件\vue-devtools-dev\shells\chrome

6. 浏览器提示

   + `Vue.js is detected on this page. Open DevTools and look for the Vue panel.`

     重新打开控制台出现，安装成功

   + `Vue.js is detected on this page. Devtools inspection is not available because it's in production mode or explicitly disabled by the author.`

     使用的vue是vue.min.js线上版本更换为vue.js生产版本即可