# computed和methods以及watch

[TOC]

***
## computed

+ 方法不能与data内属性同名
+ 计算属性，仅仅计算函数内相关的属性变化时，调用
+ 必须添加return返回值

```html
    <div id="demo">{{ fullName }}</div>
    <script type="text/javascript">
    var vm = new Vue({
        el: "#demo",
        data: {
            firstName: 'Foo',
            lastName: 'Bar',
        },
        computed: {
            fullName: {
                // getter
                // 默认取值，当监听的属性变化，调用
                get: function() {
                    console.log("取值")
                    return this.firstName + ' ' + this.lastName
                },
                // setter
                // 当vm.fullname='xxx'，调用
                set: function(newValue) {
                    console.log(newvalue)
                    console.log("赋值")
                    var names = newValue.split(' ')
                    //这里修改了get中监听的相关属性，又调用get
                    this.firstName = names[0]
                    this.lastName = names[names.length - 1]
                }
            }
        }
    });
    </script>
```

## watch

+ 监听同名属性

+ 方法能与data内属性同名，模板内同名属性指向data的属性
+ 方法的实参指向同名属性，过去值和现在值

```html
    <div id="demo">{{ fullName }}</div>
	<script type="text/javascript">
    var vm = new Vue({
        el: "#demo",
        data: {
            firstName: 'Foo',
            lastName: 'Bar',
            fullName: 'xxx'
        },
        watch: {
            firstName: function(val) {
                console.log(arguments)
                this.fullName = val + ' ' + this.lastName
            },
            lastName: function(val) {
                console.log(arguments)
                this.fullName = this.firstName + ' ' + val
            }
        },
    });
    </script>
```



***
```html
    <div id="demo">

        <div>{{num1,num2}}</div>
        <div>{{med()}}</div>
        <div>{{comfn}}</div>
    
    </div>
    <script type="text/javascript">
    
    var vm = new Vue({
        el: "#demo",
        data: {
            num1: 1,
            num2: 2
        },
        methods: {
            med:function(){
                //data数据任意变化都调用
                console.log(arguments);
                return Date.now();
            }
        },
        computed:{
            comfn:function(){
                console.log(arguments);
                //当num1属性变化才执行，修改num2不调用
                console.log(this.num1);
                return Date.now();
            }
        }
    });
    
    //控制台给vm.$data.num3=1或delete vm.$data.num1均不调用methods和computed
    </script>
```

+ methods

  + 所属组件的data对象任意属性发生变化，都调用

+ computed
  + 函数arguments中，添加了所属组件的Vue实例化对象
  + 仅函数内监听的属性发生变化，才调用，未监听属性变化不调用

+ watch

  仅监听的同名属性变化，调用同名方法

+ 差异

  + computed相比methods效率更高，依赖缓存
  + 监听的访问不一样，一个比一个少
