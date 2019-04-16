# 冒泡和捕获以及target和currentTarget

[TOC]
***
## 冒泡和捕获执行顺序
**事件只能向上传递，而传递时执行顺序是冒泡和捕获的区别**

捕获和冒泡是指当元素嵌套，且嵌套元素都绑定同样时间，`当子代盒子事件触发，父辈事件也同样触发，但是当父辈事件触发，子代事件则不触发，因为子代多个`，也就是事件会沿着父辈一路传递，而冒泡和捕获区别在于事件触发的顺序不一样，冒泡是子代事件触发，触发顺序是子代到父辈，向上传递，而捕获是指当子代事件触发，事件执行顺序是父辈传递给后代

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style type="text/css">
    .demo1 {
        width: 600px;
        height: 600px;
        background-color: pink;
    }

    .demo2 {
        width: 500px;
        height: 500px;
        background-color: yellow;
    }

    .demo3 {
        width: 400px;
        height: 400px;
        background-color: red;
    }

    .demo4 {
        width: 300px;
        height: 300px;
        background-color: green;
    }

    .demo5 {
        width: 200px;
        height: 200px;
        background-color: grey;
    }

    .demo6 {
        width: 100px;
        height: 100px;
        background-color: #24FACD;
    }
    </style>
</head>

<body>
    <div class="demo1">
        <div class="demo2">
            <div class="demo3">
                <div class="demo4">
                    <div class="demo5">
                        <div class="demo6"></div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <script type="text/javascript">
    var divs = document.querySelectorAll("div");

    //纯冒泡，从点击的元素开始，父辈中所有的同样点击事件的元素形成的事件触发的链

    //事件顺序自己到父辈，向上传递

    divs[0].addEventListener("click",function(e){
    	console.log("粉盒子被点击");

    	console.log(e.target,e.currentTarget)
    })
    divs[1].addEventListener("click",function(e){
    	console.log("黄盒子被点击");

    	console.log(e.target,e.currentTarget)
    });
    divs[2].addEventListener("click",function(e){
    	console.log("红盒子被点击");

    	console.log(e.target,e.currentTarget)
    });
    divs[3].addEventListener("click",function(e){
    	console.log("绿盒子被点击");

    	console.log(e.target,e.currentTarget)
    });
    divs[4].addEventListener("click",function(e){
    	console.log("灰盒子被点击");

    	console.log(e.target,e.currentTarget)
    });
    divs[5].addEventListener("click",function(e){
    	console.log("蓝盒子被点击");

    	console.log(e.target,e.currentTarget)
    });



    //纯捕获，执行顺序是父辈到自己，自己的后代不触发，因为后代不确定

    // divs[0].addEventListener("click", function() {
    //     console.log("粉红盒子被点击");
    // }, true)
    // divs[1].addEventListener("click", function() {
    //     console.log("黄盒子被点击");
    // }, true);
    // divs[2].addEventListener("click", function() {
    //     console.log("红盒子被点击");
    // }, true);
    // divs[3].addEventListener("click", function() {
    //     console.log("绿盒子被点击");
    // }, true);
    // divs[4].addEventListener("click", function() {
    //     console.log("灰盒子被点击");
    // }, true);
    // divs[5].addEventListener("click", function() {
    //     console.log("蓝盒子被点击");
    // }, true);



    //捕获和冒泡混合

    //当混合时，事件触发顺序是，将事件元素分为捕获组合和冒泡组，先从父辈捕获组一路向下执行，在冒泡组一路向上执行
        
    // divs[0].addEventListener("click", function() {
    //     console.log("粉盒子被点击");
    // }, false)
    // divs[1].addEventListener("click", function() {
    //     console.log("黄盒子被点击");
    // }, true);
    // divs[2].addEventListener("click", function() {
    //     console.log("红盒子被点击");
    // }, false);
    // divs[3].addEventListener("click", function() {
    //     console.log("绿盒子被点击");
    // }, true);
    // divs[4].addEventListener("click", function() {
    //     console.log("灰盒子被点击");
    // }, true);
    // divs[5].addEventListener("click", function() {
    //     console.log("蓝盒子被点击");
    // }, false);
    </script>
</body>

</html>
```

### 事件冒泡和捕获混合

`将同个事件的元素形成的事件链，分成捕获组和冒泡组，事件触发顺序是`

`先触发捕获组，从父辈一路向下，直到触发事件的元素，再触发冒泡组，一路向上，而最伸层的元素，也就是事件的触发者，它事件是否是冒泡或是捕获不影响执行顺序`

***

### 阻止事件冒泡和捕获

+ event.stopPropagation()
+ event.stopImmediatePropagation()

## event.target和event.currentTarget
`由于事件可以由于捕获和冒泡触发，target指向事件的真正触发者，currentTarget指向当前时间的触发者也就是自身`