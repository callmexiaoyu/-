# 插槽v-slot

[TOC]

***

## v-slot

替代卡标签slot具名插槽属性和作用域插槽属性

+ 旧版本slot具名插槽

  `slot='name'`

+ 旧版本slot-scope作用域插槽

  数组方程都行类似props存放

  `slot-scope='{属性1，属性2}'`

  `slot-scope="[classname,fn]"`

+ v-slot联合提到用法

  `v-slot:default='{属性1，属性2}'`

  `v-slot:name='{属性1，属性2}'`

  简写

  `#name='{属性1，属性2}'`

  ```html
      <div id="demo">
  
          <slot-component :post='post'>
              <!-- 默认卡标签的绑定指令作用域来自，当前所在组件的$parent，也就vm -->
  
              <!-- slot-scope="{classname,fn}"，卡标签添加该属性，绑定指令作用域变更为所在组件，其中记录着从组件中拿到的变量和函数变量，当方法slot-scope没有，则从父组件中拿 -->
            <!--   <div slot-scope="[classname,fn]" @click="fn" :class="classname" style="width: 100px;height: 100px;background-color: pink"></div> -->
  
  
              <!-- v-slot替换具名插槽和作用域插槽 -->
              <template v-slot:default='{classname,fn}'>
                  <div @click="fn" :class="classname" style="width: 100px;height: 100px;background-color: pink" v-text='classname'>{{classname}}</div>
              </template>
              <
          </slot-component>
      </div>
  
      <script type="text/javascript">
          Vue.component('slot-component',{
              props:['post'],
  
              data:function(){
                  return {
                      classname:this.post.classname,
                  }
              },
  
              methods:{
                  fn:function(e){
                      console.log("来自子组件的方法")
                      console.log(this)
                      console.log(e.target)
                  }
              },
              //模板slot标签
              //绑定属性获取来自子组件的数据和方法
              template:
              `
              <div><slot :classname='classname' :fn='fn'></slot></div>
              `
          });
  
          var vm = new Vue({
              el: "#demo",
              data:{
                  classname:"父组件",
                  post:{
                      classname:"子组件",
                  }
              },
              methods:{
                  fn:function(e){
                      console.log("来自父组件的方法")
                      console.log(this)
                      console.log(e.target)
                  }
              }
          });
      </script>
  ```

  