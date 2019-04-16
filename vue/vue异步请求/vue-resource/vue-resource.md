# vue-resource

[TOC]

***

默认发送的数据类型是application/json，与axios一样

vue-resource自动给实例挂载了$http方法，而axios则需要手动添加



+ get(url, [data], [options]);
+ post(url, [data], [options]);
+ jsonp(url, [data], [options]);
|              |                       |                                                              |
| ------------ | --------------------- | ------------------------------------------------------------ |
| 参数         | 类型                  | 描述                                                         |
| url          | string                | 请求的URL                                                    |
| method       | string                | 请求的HTTP方法，例如：'GET', 'POST'或其他HTTP方法            |
| body         | Object,FormDatastring | request body                                                 |
| params       | Object                | 请求的URL参数对象                                            |
| headers      | Object                | request header                                               |
| timeout      | number                | 单位为毫秒的请求超时时间 (0 表示无超时时间)                  |
| before       | function(request)     | 请求发送前的处理函数，类似于jQuery的beforeSend函数           |
| progress     | function(event)       | ProgressEvent回调处理函数                                    |
| credientials | boolean               | 表示跨域请求时是否需要使用凭证                               |
| emulateHTTP  | boolean               | 发送PUT, PATCH, DELETE请求时以HTTP                           |
| emulateJSON  | boolean               | 将request body以application/x-www-form-urlencoded content type发送 |



```html
    <div id="demo">
        <button @click="get">vue-resource的get请求</button>
        <button @click="post">vue-resource的post请求</button>
        <button @click="jsonp">vue-resource的jsonp请求</button>
    </div>
    <script type="text/javascript">
    //vue-resource给vue的实例添加$http对象
    var vm = new Vue({
        el: "#demo",
        methods: {
            get: function() {
                // console.log(this);

                this.$http.get("/get", {
                    params: {
                        info: "vue-resource的get请求"
                    }
                }).then(function(response) {

                    console.log(arguments)
                    console.log(response.body)

                }).catch(function(error) {
                    console.log(error)
                })
            },
            post: function() {
                // console.log(this);
                //全局控制表单提交为application/x-www-form-urlencoded
                // Vue.http.options.emulateJSON = true;


                this.$http.post("/post", {
                    info: "vue-resource的get请求"
                },{
                	emulateJSON: true
                }).then(function(response) {


                    console.log(arguments)
                    console.log(response.body)

                }).catch(function(error) {
                    console.log(error)
                })
            },
            jsonp: function() {
                this.$http.jsonp('/jsonp')
            }
        }
    })
    </script>
```
