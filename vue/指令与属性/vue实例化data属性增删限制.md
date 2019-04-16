# vue实例化data属性增删限制

[TOC]

***

```js
var vm = new Vue({
  data: {
    a: 1
  }
})
// `vm.a` 现在是响应式的

vm.b = 2
// `vm.b` 不是响应式的
```
**默认实例化data后，当data的数据发生变化，则响应式渲染**
**一旦实例化后则通过`vm.新属性`，vue不能响应式渲染**

`实例化后，Vue 不能动态添加根级别的响应式属性`

由于vue基于属性描述性对象

```js
Object.defineProperty(对象,属性,{
get:function(){

},
set:function(){

}});
```

`这两个方法监听属性的取值和赋值，但由于js限制，没有监听属性删除和添加，故vue的属性添加和删除不是响应式的，而实例化的data的对象的所有属性都被设置了get和set`

**补救方法**

+ 未来需要的属性，即使现在不使用，也添加上去，赋值为""，提前占坑

+ Vue.set(object, key, value)，添加单个响应式属性，

+ `批量添加多个响应式新属性

  ```js
  vm.userProfile = Object.assign({}, vm.userProfile, {
    age: 27,
    favoriteColor: 'Vue Green'
  })
  ```

  

