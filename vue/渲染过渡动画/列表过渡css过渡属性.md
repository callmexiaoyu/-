# 列表过渡

[TOC]

***

## transition过渡组件

+ css过渡属性
+ css动画
+ js动画

单个元素过渡，css过渡的相关class类动画结束删除属性

+ transition组件

  过渡组件根元素有且仅有一个，过渡组件本身不渲染，给过渡组件添加相关属性会自动添加到根元素，过渡效果结束删除属性

+ transition-group

## transition-group过渡组组件

+ 过渡组件属性
  + appear属性 列表入场动画，所有元素调用过渡进入钩子函数
  + tag属性，指定过渡组件实际渲染标签

  ```html
  <transition-group appear name="staggered-fade" tag="ul" v-bind:css="false" v-on:before-enter="beforeEnter" v-on:enter="enter" v-on:after-enter="afterEnter" v-on:enter-cancelled="enterCancelled" v-on:before-leave="beforeLeave" v-on:leave="leave" v-on:after-leave="afterLeave" v-on:leave-cancelled="leaveCancelled">
  ```

  

+ css过渡属性

  + `给过渡组组件添加name属性，css六个相关的class类名，列表增删元素，仅增删元素本身自动添加相关的类名，结束删除，导致当插入元素时，后面元素瞬间后移，空出元素位置，然后开始效果，而删除元素时，效果结束，DOM销毁或display:none时，后面元素瞬间前移，占据删除元素位置`

  + css过渡属性本身display:none时，后续元素瞬移上来，不支持，需要等增删元素结束后，计算后面元素的相关属性添加动画

  + v-move

    在六个clas过渡组件类名之上，添加v-move第七个类名，给增删的元素后续所有元素在增删时，通过transform属性过渡动画，过渡组组件自动计算后续元素的新旧位置，相当于后续元素的v-enter/v-leave以及v-enter-to/v-leave-to添加了列表初始和结束坐标，v-move则添加相关过渡属性即可，相当于v-enter-active/v-leave-active

    + 缺点当删除时不启动

    + 因为当删除时，v-move立刻计算位置，而删除元素的过渡动画未结束，v-move计算为位移为0，故要在动画执行期间，当删除元素脱标，此时v-move计算得知相应的tranform

      ```html
      v-leave-active {
          position: absolute;
      }
      ```

    + v-leave期间脱标，v-move会计算提前，不合适

  + 由于v-move使用css过渡属性，只能对块级元素起作用

### 示例

```html
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.14.1/lodash.min.js"></script>
    <style type="text/css">
    span {
        display: inline-block;
        margin-right: 10px;
        /*过渡属性对块级元素有效*/
    }

    /*单个元素的过渡属性，后续元素瞬移突兀*/
    .list-enter-active,
    .list-leave-active {
        transition: opacity, transform, 5s;
    }


    .list-enter,
    .list-leave-to {
        transform: translateX(30px);
    }

    /*增加元素的后面所有元素，通过transform属性，产生位移过渡动画，当删除元素不启动*/
    .list-move {
        transition-duration: 10s;
        transition-property: transform;
    }

    /*删除元素脱标，v-move计算位移，立马贴上去，transform后移，过渡归零*/
    .list-leave-active {
        position: absolute;
    }
    /*过早计算*/
    /*.list-leave {
        position: absolute;
    }*/
    </style>

<body>
    <div id="list-demo" class="demo">
        <button v-on:click="add">增加</button>
        <button v-on:click="remove">删除</button>
        <button v-on:click="shuffle">混乱</button>
        <transition-group name="list" tag="ul">
            <li v-for="item in items" v-bind:key="item">
                {{ item }}
            </li>
        </transition-group>
        <span>观察瞬移</span>
    </div>
    <script type="text/javascript">
    new Vue({
        el: '#list-demo',
        data: {
            items: [1, 2, 3, 4, 5, 6, 7, 8, 9],
            nextNum: 10
        },
        methods: {
            randomIndex: function() {
                return Math.floor(Math.random() * this.items.length)
            },
            add: function() {
                this.items.splice(this.randomIndex(), 0, this.nextNum++)
            },
            remove: function() {
                this.items.splice(this.randomIndex(), 1)
            },
            shuffle: function() {
                this.items = _.shuffle(this.items)
            }
        }
    })
    </script>
```

