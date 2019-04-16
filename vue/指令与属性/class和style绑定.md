# class和style绑定

[TOC]

***

## class绑定

```html
    <style type="text/css">
    .action {
        width: 100px;
        height: 100px;
        background-color: pink;
    }
    </style>
<body>
    <!-- 原生类名绑定 -->
    <!-- <div class="action" id="demo"></div> -->


    <!-- 方法一，绑定属性值 -->
    <!-- <div :class="msg" id="demo"></div> -->


    <!-- 方法二调用methods返回字符串 -->
    <!-- 
	<div :class="active()" id="demo"></div> -->



    <!-- 数组 -->
    <!-- 方法三，以数组的方式，值指向data -->
    <!-- <div :class=['action','ok'] id="demo"></div> -->
    <!-- <div :class="'action ok'" id="demo"></div> -->
    <!-- <div :class="classarr" id="demo"></div> -->

    <!-- 混合 -->
    <!-- <div :class=[msgstr,{show:show}] id="demo"></div> -->

    <!-- 以下两者等价 -->
    <!-- <div :class="['action']" id="demo"></div> -->
    <!-- <div :class=["action"] id="demo"></div> -->

   
   	<!-- 对象 -->

    <!-- 方法四，绑定对象赋值是对象时 -->
    <!-- 通过对象属性的布尔值是否添加属性 -->
    <!-- <div :class={action:show,hidden:show} id="demo"></div> -->
    <!-- <div :class='{action:show,hidden:show}' id="demo"></div> -->

    <!-- <div :class="classobj" id="demo"></div> -->

     <!-- 计算属性 -->
    <!-- <div :class={action:comAction} id="demo"></div> -->





    <!-- style绑定内联样式 -->

    <!-- 原生内联样式 -->
     <!-- <div style="background-color: red;width: 100px;height: 100px" id="demo"></div> -->

     <!-- 绑定样式 -->
    <!-- <div :style={backgroundColor:style.backgroundColor,width:style.width,height:style.height} id="demo"></div> -->

    <!-- 绑定数组对象形式 -->
    <div :style=[{backgroundColor:style.backgroundColor},{width:style.width},{height:style.height}] id="demo"></div>
    
    
    <!-- 直接绑定对象 -->
    <div :style=[style] id="demo"></div>

    <script type="text/javascript">
    var vm = new Vue({
        el: "#demo",
        data: {
            show: true,
            msg: "action",
            msgstr: "action ok",
            classarr: ["action","ok"],
            classobj: {
                action: true,
                'hidden': false,
            },
            style:{
            	backgroundColor:"red",
            	width:"100px",
            	height:"100px"
            }
        },
        methods: {
            // 返回一个字符串
            active: function() {
                return 'action';
            }
        },
        //计算属性
        computed: {
            comAction: function() {
                return this.show;
            }
        }
    });
    console.log(vm);
    </script>
```

+ `原生class值是字符串，而v:bind绑定后值看做表达式`
+ `默认值可以需要两边添加"，当值数组或对象时引号''可以省略`
+ `数组或对象内部不添加"，表示一个变量`

+ **class和style绑定属性，赋值成一个json对象，键值对形式，class值布尔值，style的css属性值字符串或数字**