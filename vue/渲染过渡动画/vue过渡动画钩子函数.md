# vue过渡动画钩子函数

[TOC]

***

## 过渡组件的钩子函数

`通过钩子函数，过渡组件name属性，css过渡类名，以及js动画工具velocity完成vue过渡时的css动画和js动画的混合`

+ 离开

  + beforeLeave

    实参一，过渡组件transition的子元素，根元素

    `过渡离开自定义动画的根元素属性初始化`

  + leave

    实参一，过渡组件transition的子元素，根元素

    `vue过渡组件给根元素添加过渡离开的相关class类名`

    `配合velocity完成过渡离开自定义动画`

    `实参二，done()，放在velocity动画完成之后的回调函数中，v-if的话vue销毁DOM对象，v-show的话vue对根元素display:none，以及删除vue过渡组件给根元素添加过渡离开的相关class类名`

  + afterLeave

    实参一，过渡组件transition的子元素，根元素

    `v-if销毁后后续处理`

  + leaveCancelled

    `v-show，DOM对象display:none后续操作`

+ 进入

  + beforeEnter

    实参一，过渡组件transition的子元素，根元素

    `过渡进入自定义动画根元素属性初始化`

  + enter

    实参一，过渡组件transition的子元素，根元素

    `vue过渡组件给根元素添加过渡离开的相关class类名`

    `配合velocity完成过渡离开自定义动画`

    `实参二，done()，放在velocity动画完成之后的回调函数中，以及删除vue过渡组件给根元素添加过渡进入的相关class类名`

  + afterEnter

    实参一，过渡组件transition的子元素，根元素

    `v-if渲染完毕后续处理`

  + enterCancelled

    `v-show，DOM对象display:none后续操作`

***

## velocity.js辅助完成过渡组件的过渡js动画

仅使用js动画，过渡组件添加v-bind:css="false"，跳过css检测

```html
    <style type="text/css">
    .show {
        display: block;
        width: 400px;
        height: 400px;
        background-color: pink;
    }
    </style>

<body>
    <div id="demo">
        <button v-on:click="show = !show">
            Toggle
        </button>
        <!-- 结合velocity以及vue过渡组件的钩子函数，通过js操作完成增删DOM时的自定义动画 -->
        <!-- 过渡组件的name属性用于结合css的类名完成vue过渡的js动画以及css动画混合 -->
        <transition name="vue" v-on:before-enter="beforeEnter" v-on:enter="enter" v-on:after-enter="afterEnter" v-on:enter-cancelled="enterCancelled" v-on:before-leave="beforeLeave" v-on:leave="leave" v-on:after-leave="afterLeave" v-on:leave-cancelled="leaveCancelled">
            <div v-show="show" class="show"></div>
        </transition>
    </div>
    <script type="text/javascript">
    new Vue({
        el: '#demo',
        data: {
            show: true
        },
        methods: {
            //velocity以及vue过渡组件的钩子函数，通过js操作完成增删DOM对象时的自定义动画

            //过渡进入
            beforeEnter: function(el) {

                el.style.width = 0;
                el.style.height = 0;
                console.log("过渡进入根元素属性初始化")
                console.log("beforeEnter");
                console.log(arguments)
            },
            enter: function(el, done) {
                console.log("过渡进入开始自定义动画，vue过渡组件给根元素添加过渡进入的相关class类名")
                // 过渡组件的name属性，通过添加相关的类名，完成js动画以及css动画的混合
                Velocity(el, { width: 400, height: 400 }, {
                    duration: 1000,
                    complete: function() {
                        console.log("过渡进入自定义动画结束");
                        //done()，删除vue过渡组件给根元素添加过渡进入的相关class类名
                        done();
                    }
                });
                console.log("enter");
                console.log(arguments);
            },
            afterEnter: function(el) {
                console.log("过渡进入元素动画完成后续操作")
                console.log("afterEnter")
                console.log(arguments)
            },
            // enterCancelled 只用于 v-show 中
            enterCancelled: function(el) {
                console.log("v-show，过渡进入自定义动画完成后的后续操作")
                console.log("enterCancelled")
                console.log(arguments)
            },


            //过渡离开
            beforeLeave: function(el) {
                el.style.width = 1000;
                el.style.height = 1000;
                console.log("过渡离开根元素属性初始化")
                console.log("beforeLeave");
                console.log(arguments)
            },
            leave: function(el, done) {
                console.log("过渡离开开始自定义动画，vue过渡组件给根元素添加过渡离开的相关class类名");
                // 过渡组件的name属性，通过添加相关的类名，完成js动画以及css动画的混合
                Velocity(el, { width: 0, height: 0 }, {
                    duration: 1000,
                    complete: function() {
                        console.log("过渡离开自定义动画结束，且销毁DOM对象")
                        // done()，v-if的话用来销毁DOM对象，v-show的话根元素属性display:none，删除vue过渡组件给根元素添加过渡进入的相关class类名
                        done();
                    }
                });
                console.log("leave")
                console.log(arguments);
            },
            afterLeave: function(el) {
                console.log("过渡离开销毁DOM后的后续操作")
                console.log("afterLeave")
                console.log(arguments);
            },
            // leaveCancelled 只用于 v-show 中
            leaveCancelled: function(el) {
                console.log("v-show，过渡离开自定义动画完成后的后续续，DOM对象不销毁")
                console.log("leaveCancelled")
                console.log(arguments)
            }
        }
    })
    </script>
```

`通过钩子函数，过渡组件name属性和css过渡类名，以及js动画工具velocity完成vue过渡时的css动画和js动画的混合`

`混合使用注意leave和enter的done需要等到js动画以及css动画全部执行后使用，done有清除类名的作用销毁DOM作用等`