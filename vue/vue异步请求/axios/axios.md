# axios

[TOC]

***

axios相当于jquery的$ajax

```html
    <button id="btn1">axios发起get请求</button>
    <button id="btn2">axios发起post请求</button>

    <script type="text/javascript">
    var btn1 = document.querySelector("#btn1");
    var btn1 = document.querySelector("#btn1");

    btn1.addEventListener("click", function() {
        console.log("已经发起get请求")
        axios.get('/get', {
	        	//url请求get的查询字符串
                params: {
                    name:123
                }
            })
            .then(function(response) {
                console.log(arguments)
                console.log(response)
                console.log(response.data);
            })
            .catch(function(error) {
                console.log(error);
            });

    });

    btn2.addEventListener("click", function() {
        console.log("已经发起post请求")
        axios.post('/post', {
	        	//post请求的FormData
                name:123
            })
            .then(function(response) {
                console.log(arguments)
                console.log(response)
                console.log(response.data);
            })
            .catch(function(error) {
                console.log(error);
            });

    });
    </script>
```

|          |                  |                                     |
| -------- | ---------------- | ----------------------------------- |
|          | axios默认        | 默认表单post                        |
| 数据格式 | application/json | application/x-www-form-urlencoded   |
|          | Request Payload  | FormData                            |
|          | json字符串       | xxx=xxx&xxx=xxx（字符串形式键值对） |
|          |                  |                                     |

+ `设置数据传输类型时，传送的数据必须符合数据类型定义的形式，否则设置的类型变为实际传输的类型`


## 异步请求数据类型和对应形式
|  |                  |      |
| ----------------------------------- | ---------------- | ---- |
| Content-Type                        | 数据类型         |      |
| text/plain;charset=UTF-8            | 字符串或二进制流 |      |
| application/x-www-form-urlencoded   | 键值对字符串     |      |
| application/json                    | json字符串       |      |
| multipart/form-data                 | 二进制流         |      |

设置application/x-www-form-urlencoded，但不满足其键值对形式

express中间件connect-multiparty处理post的multipart/form-data