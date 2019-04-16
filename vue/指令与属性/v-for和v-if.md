# v-for 遍历 v-if判断

[TOC]

***
## v-for

```html
    <style type="text/css">
        .demo1 {
            width: 100px;
            height: 100px;
            background-color: pink;
        }
        .demo2 {
            width: 200px;
            height: 100px;
            background-color: gray;
        }
        .demo3 {
            width: 300px;
            height: 100px;
            background-color: red;
        }
        .demo4 {
            width: 400px;
            height: 100px;
            background-color: blue;
        }
        .demo5 {
            width: 500px;
            height: 100px;
            background-color: yellow;
        }
    </style>
<body>
    <div id="demo">
        <div v-for="(item,index) in arr" :class="item.msg">{{item.msg}}</div>
    </div>

    <script type="text/javascript">
        var vm = new Vue({
            el:"#demo",
            data:{
                arr:[{
                    msg:"demo1"
                },{
                    msg:"demo2"
                },{
                    msg:"demo3"
                },{
                    msg:"demo4"
                },{
                    msg:"demo5"
                }]
            },
            methods:{

            }
        });
    </script>

    <!-- 控制台数组操作方法 -->
    <!-- vm.arr.pop()
    vm.arr.push({ msg: 'demo1' });
    vm.arr.shift();
    vm.arr.unshift({msg:"demo2"});
    vm.arr.reverse();
 -->
</body>
```

#### 数组修改vue响应式渲染实时刷新

`以下修改数组`

+ push

+ pop

+ shift

+ unshift

+ reverse

+ sort

+ splice

#### 不修改数组，返回新数组vue实时刷新
+ `以下数组方法不修改原数组，返回新数组，重新赋值`**缺点重新赋值修改了原数组**

  + concact
  + filter
  + slice

  ```js
  vm.arr = vm.arr.slice(startindex,endindex)
  ```

+ `通过计算属性或methods返回新数组`

  **不修改原数组，产生新数组，通过computed和methods的方法创建了一个新数组**


#### 数组成功修改vue不刷新

**一般当数组改变，v-for会响应式实时刷新，但存在以下，原数组被修改，但vue不会渲染**

+ `通过对象添加属性的方式给数组添加新元素`

  ```js
  //用对象方式添加新元素
  var arr2 = [1, 2, 3];
  arr2[3] = 100;
  arr2[10] = 1000;
  console.log(arr2, arr2.length);//[ 1, 2, 3, 100, , , , , , , 1000 ] 11
  //该方式虽然成功给数组添加新元素，但vue不渲染
  ```

+ `变更数组对象内部元素的索引指针指向`

  ```js
  vm.arr[0] = null;
  //该方法同样数组元素被替换，指针指向改变，但vue不渲染
  ```

  + Vue.set(vm.arr,0,null);

+ `修改数组length属性`

  ```js
  var arr1 = [1, 2, 3, 4, 5, 6, 7, 8, 9];
  
  console.log(arr1);
  //设置数组长度，截断数组元素
  arr1.length = 3;
  console.log(arr1);//[ 1, 2, 3 ]
  
  //增加数组长度，产生空元素
  arr1.length = 9;
  console.log(arr1);//[ 1, 2, 3, , , , , ,  ]
  //该方式同样数组被修改，数组length变化，但vue不渲染
  ```

  + vm.arr.splice(length) ，该方法修改数组长度且响应式渲染
#### v-for渲染组件复用

默认组件复用，通过`:key赋予唯一的值，告诉vue渲染时复用组件唯一性`

***
## v-if

通过判断语句，选择性的渲染一片html

`v-else必须紧跟v-if后面`

```html
	<!-- vue控制的元素 -->
    <div id="demo">
		<!-- v-if所属的控制标签一并渲染 -->
        <div v-if="awesome">
            <h1 id="title">真</h1>
        </div>
        
        <div v-else>
            <h1 id="title">假</h1>
        </div>
    </div>


    <script type="text/javascript">
    // v-if的表达式值的判断语句为真值时，会将绑定元素以及内容元素不渲染出来
    var vm = new Vue({
        el: '#demo',
        data: {
            awesome: true,
        }
    })
    </script>
```

`缺点是会将v-if所在的元素标签一并渲染`

`将v-if所在的标签改为template，此时v-if所在标签不渲染`

```html
    <!-- 结合template摆脱容器 -->
    <div id="demo">
		<!-- v-if所属的控制标签不被渲染 -->
        <template v-if="name===arr[0].name">
            <h1 id="title">{{arr[0].name}}</h1>
        </template>
        <template v-else-if="name===arr[1].name">
            <h1 id="title">{{arr[1].name}}</h1>
        </template>
        <template v-else-if="name===arr[2].name">
            <h1 id="title">{{arr[1].name}}</h1>
        </template>
        <template v-else>
            <h1 id="title">未找到</h1>
        </template>
    </div>

    <script type="text/javascript">
    // v-if的表达式值的判断语句为真值时，会将绑定元素以及内容元素不渲染出来，相当于display:none
    var vm = new Vue({
        el: '#demo',
        data: {
            awesome: true,
            name: "jack",
            arr: [{ name: "jack" }, { name: "peter" }, { name: "rose" }]
        }
    })
    </script>
```



###  \<template>标签

```html
    <template id="tem">
        <div id="app">
            <h1 id="title">hello world!</h1>
        </div>
    </template>
    <script type="text/javascript">
    var tem = document.getElementById("tem"); //获取template标签
    console.log(tem);
    console.log(tem.innerHTML); //
    var title = tem.content.getElementById("title"); //在template标签内部内容，必须要用.content属性才可以访问到
    console.log(title);
    </script>
```

+ template语义化模板标签

+ 标签默认css样式display:none，不可见，标签内容隐藏，html5新标签

### v-if和v-show对比

v-if表达式切换时，适当的（复用组件，切换模板时，相同的组件默认复用，`添加key告诉vue不复用` ）销毁DOM对象和创建DOM对象，`真正的渲染`

```html
    <!-- 组件复用 -->
    <!-- 表单值存在 -->
    <div id="demo">
		<template v-if="loginType">
            <label>Username</label>
            <input placeholder="Enter your username">
		</template>
		<template v-else>
            <label>Email</label>
            <input placeholder="Enter your email address">
		</template>
		<button @click="change">切换</button>  	
    </div>



    <!-- 组件不复用，表单值清空 -->
    <!-- key属性告诉vue渲染时不复用标签元素，每次切换都销毁DOM且创建新的DOM对象 -->
    <div id="demo">
        <!-- label标签复用 -->
		<template v-if="loginType">
            <label>Username</label>
            <input placeholder="Enter your username" key="username-input">
		</template>
		<template v-else>
            <label>Email</label>
            <input placeholder="Enter your email address" key="email-input">
		</template>
		<button @click="change">切换</button>  	
    </div>


    <script type="text/javascript">
    // v-if的表达式值的判断语句为真值时，会将绑定元素以及内容元素不渲染出来，相当于display:none
    var vm = new Vue({
        el: '#demo',
        data: {
        	loginType:true,
            awesome: true,
            name: "jack",
            arr: [{ name: "jack" }, { name: "peter" }, { name: "rose" }]
        },
        methods:{
        	change:function(){
        		this.loginType = !this.loginType;
        	}
        }
    });
    </script>
```

v-show是渲染DOM对象但css隐藏，切换成本低，DOM存在于DOM树种
***

## v-if和v-for不能混用

### 混用

```html
    <!-- <ul id="demo">
        <li v-for="(item,index) in arr" v-if="item.age > 18">{{item.age}}</li>
    </ul> -->



    <ul id="demo">
        <li v-for="(item,index) in arrfun">{{item.age}}</li>
    </ul>


    <script type="text/javascript">
        var vm = new Vue({
            el:"#demo",
            data:{
                arr:[{name:"jack",age:18},{name:"peter",age:20},{name:"rose",age:30},{name:"ben",age:25},{name:"li",age:17},{name:"Daiv",age:21},]
            },
            computed:{
                arrfun:function(){
                    return this.arr.filter(function(ele,index) {
                        return ele.age > 18;
                    });
                }
            }
        });
    </script>
```
v-for级别高于v-if
```

this.users.map(function (user) {
  if (user.isActive) {
    return user.name
  }
})
```

`采用map，map方法是对数组每个元素进行处理返回新数组`