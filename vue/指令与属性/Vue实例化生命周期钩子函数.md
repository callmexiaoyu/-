# Vue实例化生命周期钩子函数

[TOC]

***

| ![](C:\Users\Administrator\Desktop\前端资料\优秀参考资料html及md文件\vue\指令与属性\20180704165722301.png) |                                    |
| ------------------------------------------------------------ | ---------------------------------- |
| beforeCreate                                                 | 组件实例刚被创建，组件属性未初始化 |
| created                                                      | 组件实例属性初始化完毕，$el不存在  |
| beforeMount                                                  | 模板编译/挂载前，                  |
| mounted                                                      | 模板编译/挂载后                    |
| beforeUpdate                                                 | 组件更新之前                       |
| updated                                                      | 组件更新之后                       |
| activated                                                    |                                    |
| deactivated                                                  |                                    |
| beforeDeatory                                                | .$destory()组件销毁前              |
| destoryed                                                    | 组件销毁后                         |



```html
    <div id=app>{{a}}</div>
    <script>
    var myVue = new Vue({

        el: "#app",

        data: {

            a: "Vue.js"

        },

        beforeCreate: function() {

            console.log("beforeCreate创建前")
            console.log(this.a)//undefined
            console.log(this.$el)//undefined
            

        },

        created: function() {

            console.log("created创建之后");
            console.log(this.a)//Vue.js
            console.log(this.$el)//undefined

        },

        beforeMount: function() {

            console.log("beforeMountmount之前")
            console.log(this.a)//Vue.js
            console.log(this.$el)//<div id="app">{{a}}</div>
        },

        mounted: function() {

            console.log("mountedmount之后")
            console.log(this.a)//Vue.js
            console.log(this.$el)//<div id="app">Vue.js</div>

        },

        beforeUpdate: function() {

            //控制台输入myVue.a = "update myVue"
            console.log("mounted更新前");
            console.log(this.a)//update myVue
            console.log(this.$el)//<div id="app">update myVue</div>

        },

        updated: function() {

            console.log("updated更新完成");
            console.log(this.a);//update myVue
            console.log(this.$el)//<div id="app">update myVue</div>

        },

        beforeDestroy: function() {

            console.log("beforeDestroy销毁前");
            console.log(this.a)
            console.log(this.$el)
            console.log(this.$el)

        },

        destroyed: function() {

            console.log("destroyed已销毁");
            console.log(this.a)
            console.log(this.$el)

        }

    });
```

