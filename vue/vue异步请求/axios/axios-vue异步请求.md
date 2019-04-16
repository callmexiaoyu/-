# axios-vue异步请求

[TOC]

***

```html
    <div id="demo">
        <button @click="getData">axios发起get请求</button>{{get}}
        <button @click="postData">axios发起post请求</button>{{post}}
        <button @click="createAjax">axios实例发起post请求</button>{{create}}
        <button @click="mixData">axios发起post请求</button>{{mix}}
    </div>
    <script type="text/javascript">
    //把axios方法加到Vue实例化的对象上
    Vue.prototype.$http = axios;

    var vm = new Vue({
        el: "#demo",
        data: {
            get: null,
            post: null,
            create: null,
            mix: null
        },

        methods: {
            getData: function() {
                console.log("vue已经通过axios发起get请求");

                var _this = this;
                this.$http.get("/get", {
                        params: {
                            method: "get请求"
                        }
                    }).then(function(response) {
                        console.log(arguments)
                        console.log(response)
                        console.log(response.data);
                        _this.get = response.data;
                    })
                    .catch(function(error) {
                        console.log(error);
                    });
            },
            postData: function() {
                console.log("vue已经通过axios发起post请求")

                var _this = this;
                this.$http.post('/post', {
                        //post请求的数据格式Request Payload
                        method: "post请求",

                    })
                    .then(function(response) {
                        console.log(arguments)
                        console.log(response)
                        console.log(response.data);
                        _this.post = response.data;
                    })
                    .catch(function(error) {
                        console.log(error);
                    });
            },
            mixData: function() {
                console.log("vue已经通过axios请求")

                var _this = this;
                this.$http({
                        method: "post",
                        url: "/post",
                        data: {
                            name: 123
                        }
                    }).then(function(response) {
                        console.log(arguments)
                        console.log(response)
                        console.log(response.data);
                        _this.mix = response.data;
                    })
                    .catch(function(error) {
                        console.log(error);
                    });
            },
            createAjax: function() {
                var req = this.$http.create({
                    //axios的post请求默认是Content-Type=application/json

                    //设置了数据类型，且具体内容必须满足类型规定的形式，否则解析为multipart/form-data，二进制流文件
                    headers: { 'Content-Type': 'application/x-www-form-urlencoded' },

                    transformRequest: [function(data) {
                        //data为一个对象

                        console.log(data, typeof data);

                        //这里需要将对象转为字符串键值对形式

                        //node中有querystring.stringify处理，这里需要引入相对应的模块
                        function qs(data) {

                            var str = ""
                            for (var attr in data) {
                                str += attr + "=" + data[attr] + "&";
                            };

                            var newstr = str.replace(/&{1}$/, function(match) {
                                // console.log(match)
                                return '';
                            })
                            console.log(newstr);

                            return  newstr;
                        };

                        //发送的是对象
                        return qs(data);
                    }],
                });

                var _this = this;
                req.post("/post", {

                        method: "axios实例发起的post请求，数据键值对"

                    }).then(function(response) {
                        console.log(arguments)
                        console.log(response)
                        console.log(response.data);
                        _this.create = response.data;
                    })
                    .catch(function(error) {
                        console.log(error);
                    });

            },
            jsonp: function() {
                function jsonp(str) {
                    console.log(str);
                };


                console.log("vue已经通过axios发起get请求");

                var _this = this;
                this.$http.get("/get", {
                        params: {
                            method: "get请求"
                        }
                    }).then(function(response) {
                        console.log(arguments)
                        console.log(response)
                        console.log(response.data);
                        _this.get = response.data;
                    })
                    .catch(function(error) {
                        console.log(error);
                    });


            }
        }
    })
    </script>
```

