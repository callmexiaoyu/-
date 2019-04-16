# axios路由拦截器

[TOC]

***

## 请求拦截与响应拦截

+ 请求响应拦截
+ 拦截期间处理一些过渡html

```js
createAjax: function () {
    var req = this.$http.create({
        //axios的post请求默认是Content-Type=application/json

        //设置了数据类型，且具体内容必须满足类型规定的形式，否则解析为multipart/form-data，二进制流文件
        headers: {
            'Content-Type': 'application/x-www-form-urlencoded'
        },
        transformRequest: [function (data) {
            //data为一个对象
            console.log(data, typeof data);
            //这里需要将对象转为字符串键值对形式
            //node中有querystring.stringify处理，这里需要引入相对应的模块
            function qs(data) {
                var str = ""
                for (var attr in data) {
                    str += attr + "=" + data[attr] + "&";
                };
                var newstr = str.replace(/&{1}$/, function (match) {
                    // console.log(match)
                    return '';
                })
                console.log(newstr);
                return newstr;
            };

            //发送的是对象
            return qs(data);
        }],
    });
    //axios请求拦截
    req.interceptors.request.use(config => {
        console.log(config);
        console.log("请求拦截");
        config.data.info = "axios路由拦截";
        return config;
    });
    //axios响应拦截
    req.interceptors.response.use(config=>{
        console.log(config);
        console.log("响应拦截");
        return config;
    })
    var _this = this;
    req.post("/post", {

            info: "axios实例发起的post请求，数据键值对"

        }).then(function (response) {
            console.log(arguments)
            console.log(response)
            console.log(response.data);
            _this.create = response.data;
        })
        .catch(function (error) {
            console.log(error);
        });
}
}
```

