# 列表过渡js动画

[TOC]

***

列表多个元素过渡，js的六个钩子函数，当添加或删除多个元素时，钩子函数分别调用多次，同步

transition-group

- 离开

  - beforeLeave

    实参一，过渡组件transition的子元素，根元素

    `过渡离开自定义动画的根元素属性初始化`

  - leave

    实参一，过渡组件transition的子元素，根元素

    `vue过渡组件给根元素添加过渡离开的相关class类名`

    `配合velocity完成过渡离开自定义动画`

    `实参二，done()，放在velocity动画完成之后的回调函数中，v-if的话vue销毁DOM对象，v-show的话vue对根元素display:none，以及删除vue过渡组件给根元素添加过渡离开的相关class类名`

  - afterLeave

    实参一，过渡组件transition的子元素，根元素

    `v-if销毁后后续处理`

  - leaveCancelled

    `v-show，DOM对象display:none后续操作`

- 进入

  - beforeEnter

    实参一，过渡组件transition的子元素，根元素

    `过渡进入自定义动画根元素属性初始化`

  - enter

    实参一，过渡组件transition的子元素，根元素

    `vue过渡组件给根元素添加过渡离开的相关class类名`

    `配合velocity完成过渡离开自定义动画`

    `实参二，done()，放在velocity动画完成之后的回调函数中，以及删除vue过渡组件给根元素添加过渡进入的相关class类名`

  - afterEnter

    实参一，过渡组件transition的子元素，根元素

    `v-if渲染完毕后续处理`

  - enterCancelled

    `v-show，DOM对象display:none后续操作

## 示例

```html
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.3/velocity.min.js"></script>
    <style type="text/css">
	    li {
	    	height: 20px;
	    }
    </style>

<body>
    <div id="staggered-list-demo">
        <input v-model="query">
        <transition-group name="staggered-fade" tag="ul" v-bind:css="false" v-on:before-enter="beforeEnter" v-on:enter="enter" v-on:after-enter="afterEnter" v-on:enter-cancelled="enterCancelled" v-on:before-leave="beforeLeave" v-on:leave="leave" v-on:after-leave="afterLeave" v-on:leave-cancelled="leaveCancelled">
            <li v-for="(item, index) in computedList" v-bind:key="item.msg" v-bind:data-index="index">{{ item.msg }}</li>
        </transition-group>
    </div>
    <script type="text/javascript">
    new Vue({
        el: '#staggered-list-demo',
        data: {
            query: '',
            list: [
                { msg: 'Bruce Lee' },
                { msg: 'Jackie Chan' },
                { msg: 'Chuck Norris' },
                { msg: 'Jet Li' },
                { msg: 'Kung Fury' }
            ]
        },
        computed: {
            computedList: function() {
                var vm = this
                return this.list.filter(function(item) {
                    return item.msg.toLowerCase().indexOf(vm.query.toLowerCase()) !== -1
                })
            }
        },
        methods: {

            // 列表多个元素过渡js操作

            // 过渡进入

            beforeEnter: function(el) {
                el.style.opacity = 0;
                el.style.height = 0;
                console.log("beforeEnter")
                console.log(arguments);
            },
            enter: function(el, done) {

                console.log("enter")
                console.log(arguments);
                // veloacity补间动画
                Velocity(el, { translateX: [0,100],opacity: 1, height: "1em" }, { duration: 1000, complete: done });
            },
            afterEnter: function(el) {
                console.log("afterEnter")
                console.log(arguments);
            },
            enterCancelled: function(el) {
                console.log("enterCancelled")
                console.log(arguments);
            },

            // 过渡离开

            // 删除多个元素
            // 初始化删除元素的属性
            beforeLeave: function(el) {

                console.log("beforeLeave");
                console.log(arguments);

            },
            leave: function(el, done) {
                console.log("leave");
                console.log(arguments);
                Velocity(el, { translateX: 100, opacity: 0 ,height:0}, { duration: 1000, complete: done });
            },
            afterLeave: function(el) {
                console.log("afterLeave")
                console.log(arguments);
                // ...
            },
            leaveCancelled: function(el) {
                console.log("leaveCancelled")
                console.log(arguments);
                // ...
            }
        }
    })
    </script>
```

