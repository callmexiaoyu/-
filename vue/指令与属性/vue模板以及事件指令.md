# vue指令
[TOC]
***
## 模板替v-换指令

  ```html
  <head> 
  	<style type="text/css">
      [v-cloak] {
          /*在vue.js加载之前，隐藏模板元素，加载完毕vue自动显示*/
          display: none !important;
      }
      </style>
  </head>
  <body>
      <div class="app">
  
          <p>哈哈哈{{msg1}}{{msg2}}哈哈哈</p>
          <!-- 当网速过慢，页面在vue.js没有加载完毕情况下，会将模板直接加载出来，而不是渲染后的结果 -->
  
          <p v-cloak>{{msg2}}</p>
          <!-- v-text，将控制元素的内容全部替换成普通文本，不识别模板数据的标签元素 -->
          <p v-text="msg1"><span>哈哈哈</span></p>
          <p v-text="msg3"><span>哈哈哈</span></p>
  
          <!-- v-html，将控制元素的内容全部替换成html文本，转义模板数据的标签元素 -->
          <p v-html="msg3">哈哈哈</p>
      </div>
      <script type="text/javascript">
      //控制的元素
      var vm = new Vue({
          el: '.app',
          data: {
              //将控制的元素的目标模板替均换成对应文本
              msg1: 'vue1',
              msg2: "vue2",
              msg3: "<span>vue</span>"
          }
      });
      console.log(vm)
      </script>
  </body>
  ```

+ `v-cloak`：防止vue.js网速问题导致vue.js未加载完毕，直接将{{}}模板渲染到页面中

+ `v-text=xxx`：将元素内容全部替换成目标文本，覆盖

+ `v-html=xxx`：将元素内容全部以html解析后替换，覆盖

+ `v-bind`：绑定元素属性，缩写`：属性`
***
## 绑定事件指令

+ `v-on`：绑定事件，缩写`@事件`
#### vue事件修饰符

  + .stop 阻止冒泡

    ***

    **下面三个属性是addEventListener的监听第三个参数对象的三个属性**

  + capture 事件为捕获，默认是冒泡

  + .once 只触发一次，注意事件嵌套会多次绑定，虽然只执行一次

  + passive 监听函数不会调用事件的`preventDefault`方法

    ***

  + .prevent 阻止事件默认行为

  + self 当且仅当事件target指向自身触发，也就是绑定元素的事件不受冒泡和捕获影响

***

#### vue事件修饰符连用

   > 1. 事件冒泡或捕获传递`.capture`
   > 2. 事件默认行为`.stop`
   > 3. 事件触发的函数方法
```html
<a href="http://www.baidu.com" target="_blank" class="demo1" @click.self.prevent="btn1">
```
**self.prevent和prevent.self对比**

|              |              | 是否冒泡或捕获 | 是否阻止默认行为跳转 |   是否触发绑定事件方法   |
| ------------ | -------------- | -------------------- | ---- | ------------ |
| self |  | × | × | √ |
| prevent |  | √ | √ | √ |
| self.prevent | 冒泡触发 | √ | × | × |
|  | target触发 |  | √ | √ |
| prevent.self | 冒泡触发 | √ | √ | × |
|  | target触发 | | √ | √ |

1. 当target=自身触发，都阻止默认行为，且绑定事件都执行
   
2. 连用都不能阻止冒泡，冒泡触发，都无法阻止事件句柄函数，但默认行为有差异
   
   **连用事件修饰符需要具体对待**

***

## v-model表单数据双向绑定

**input、select、textarea表单元素**

`v-model，当js数据发生变化，视图层数据也变化，且元素绑定属性的值发生变化，js的数据也变化`

```html
	<form action="" id="form">
		<p>{{msg}}</p>
		<!-- v-bind只能实现m到v -->
		<input type="text" v-bind:value="msg">

		<!-- v-model实现数据双向绑定，v到m -->
		<input type="text" v-model:value="msg">
	</form>
	<script type="text/javascript">
		// 单项绑定数据v-bind只能讲数据绑定到元素属性上，当msg变化，vue监听data.msg变化实时重新渲染
		var vm = new Vue({
			el:"#form",
			data:{
				msg:"表单",
			}
		})
	</script>
```

`可省略value，v-model="xxx"即可`

#### v-model双向绑定修饰符

+ `.lazy`

  oninput事件是表单值发生变化触发，onchange是表单值变化，光标脱离才触发，相比input需要手动点击

  将表单的input事件改为change

+ `.number`

  值转为数字

+ `trim`

  表单值去除首尾空字符