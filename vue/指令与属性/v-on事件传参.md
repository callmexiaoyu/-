# v-on绑定事件传参

[TOC]

***

原生事件传参通过闭包，返回函数

```html
    <div id="demo">
        <!-- 不传参，参数为事件Event对象 -->
		<button @click="change">不传参</button>

        <!-- 传参，参数为设置的参数 -->
        <button @click="change('参数字符串')">传参</button>

        <!-- 传递事件event和自己设置的参数 -->
        <button @click="change('参数字符串',$event)">传参</button>    
    </div>


    <script type="text/javascript">
    var vm = new Vue({
        el: '#demo',
        data: {
        },
        methods:{
        	change:function(){
        		console.log(arguments);

        	}
        }
    });
    </script>
```

$event为事件发生时的Event实例化对象