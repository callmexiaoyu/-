# Vue过渡组件css过渡和动画属性

[TOC]

***
## transition过渡组件的必要性

+ v-if和v-show条线渲染和展示内容，内容显示和消失都是瞬间的，对此需要添加过渡动画，视觉上美化
+ 一般css过渡动画，DOM对象一致存在，实现样式变化，而当添加或销毁DOM对象时则需要对此针对性进一步添加过渡动画
+ DOM对象插入html添加进入过渡动画，销毁DOM对象添加离开过渡动画
+ transition组件包裹v-if或v-show命令的元素，添加过渡效果，其中过渡组件添加name属性用于区别多个过渡组件xxx

## css过渡属性

+ 进入过渡css类名

  + xxx-enter

    进入过渡效果的起始状态

    `DOM创建时添加属性，DOM插入立刻删除属性`

  + xxx-enter-active

    进入过渡效果的耗时属性以及曲线函数等

    `DOM创建时添加属性，效果完成删除属性`

  + xxx-enter-to

    进入过渡效果的结束状态

    `DOM创建时添加属性，效果完成插入删除属性`

+ 离开过渡css类名

  + xxx-leave

    离开过渡效果的起始状态

    `离开过渡触发立刻生效，下一帧立刻删除`

  + xxx-leave-active

    离开过渡效果的耗时属性以及曲线函数等

    `离开过渡触发立刻生效，效果完成删除属性，同时销货DOM`

  + xxx-leave-to

    离开过渡效果的结束状态

    `离开过渡触发立刻生效，效果完成删除属性，同时销毁DOM`

![](C:\Users\Administrator\Desktop\前端资料\优秀参考资料html及md文件\vue\过渡动画\transition.png)

### 示例

```html
    <style type="text/css">
    
    /*DON对象插入添加进入css过渡属性动画*/

    /*进入过渡效果的耗时属性以及曲线函数*/
    .vue-enter-active {
        transition-property: background-color,width,height;
        transition-duration: 5s;
        background-color: yellow;
        width: 0px;
        height: 0px;
    }
    /*进入过渡效果结束状态*/
    .vue-enter-to{
        background-color: #D7E8FD;
        width: 100px;
        height: 100px;
    }


    /*DOM对象销毁离开css过渡属性动画*/

    /*进入过渡效果的耗时属性以及曲线函数*/
    .vue-leave-active {
        transition-property: background-color,width,height;
        transition-duration: 5s;
        background-color: #D7E8FD;
        width: 100px;
        height: 100px;
    }
    /*离开过渡效果结束状态*/
    .vue-leave-to{
        background-color: yellow;
        width: 0px;
        height: 0px;
    }

    
    /*.show {
        background-color: #D7E8FD;
        width: 100px;
        height: 100px;
    }*/

    </style>

<body>
    <div id="demo">
        <button v-on:click="show = !show">
            Toggle
        </button>
        <!-- 过渡组件包裹v-if，添加过渡动画-->
        <!-- name属性同于区别过渡组件的效果，以及css类名联系 -->
        <transition name="vue">
            <!-- 添加或销货DOM对象 -->
            <div v-if="show" class="show">hello</div>
        </transition>
    </div>
    <script type="text/javascript">
    new Vue({
        el: '#demo',
        data: {
            show: true
        }
    })
    </script>
```

`缺点功能简陋，不能平滑过渡，需要过得的属性和根元素本身属性要分开`

过渡组件使用css过渡属性，v-enter和v-leave只存在一帧无意义

***

## css动画属性

- 进入原理

  创建DOM对象，过渡组件根元素添加三个class类名，v-enter、v-enter-active、v-enter-to，其中v-enter只存在一帧，该clas类名立刻删除，其余两个属性动画完成后删除

- 离开原理

  过渡组件根元素添加三个class类名，v-leave、v-leave-active、v-leave-to，其中v-leave只存在一帧，该clas类名立刻删除，其余两个属性动画完成后删除，完成后销毁DOM对象`

```flow
st=>start: 元素html展示样式
e=>end: 元素html展示样式
op1=>operation: v-leave 离开初始化样式
op2=>operation: v-leave-active 离开中间过渡效果耗时等
op3=>operation: v-leave-to 离开结束样式
op4=>operation: v-enter 进入初始化样式
op5=>operation: v-enter-active 进入中间过渡效果耗时等
op6=>operation: v-enter-to 进入结束样式

st->op1->op2->op3->op4->op5->op6->e
```
### 示例

```html
    <style type="text/css">
    .vue-enter-active {
        animation: bounce-in .5s;
    }

    .vue-leave-active {
        animation: bounce-in .5s reverse;
    }

    @keyframes bounce-in {
        0% {
            transform: scale(0);
        }

        50% {
            transform: scale(1.5);
        }

        100% {
            transform: scale(1);
        }
    }

    .show {
        background-color: #D7E8FD;
        width: 100px;
        height: 100px;
    }
    </style>

<body>
    <div id="demo">
        <button v-on:click="show = !show">
            Toggle
        </button>
        <!-- 过渡组件包裹v-if，添加过渡动画-->
        <!-- name属性同于区别过渡组件的效果，以及css类名联系 -->
        <transition name="vue">
            <!-- 添加或销货DOM对象 -->
            <div v-if="show" class="show">hello</div>
        </transition>
    </div>
    <script type="text/javascript">
    new Vue({
        el: '#demo',
        data: {
            show: true
        }
    })
    </script>
```

