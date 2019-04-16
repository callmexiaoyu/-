# history对象

[TOC]

***

窗口的url发生变化，添加一条历史记录，而url地址添加#，包含#产生的hash值，url发生变化，但浏览器不发送请求，本身该功能用于定位当前页面，跳转到当前页面的某一坐标

+ 改变hash值，url变化，添加一条历史记录，回退

```js
window.history.pushState(state, title, url)
```

+ 历史记录只能增加，替换，不能删除

```js
window.history.pushState(null, "","login")//最后一个路径分隔符后面替换成login
window.history.pushState(null, "","/login")//url根路径替换成/login
window.history.pushState(null, "","#/login")//url最后添加#/login，替换掉相应hash值
```

