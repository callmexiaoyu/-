# velocity-js动画库

[TOC]

***

## 配合jquery使用

```js
$element.velocity({
    width: "500px",        // 动画属性 宽度到 "500px" 的动画
    property2: value2      // 属性示例
}, {
    /* Velocity 动画配置项的默认值 */
    duration: 400,         // 动画执行时间
    easing: "swing",       // 缓动效果
    queue: "",             // 队列
    begin: undefined,      // 动画开始时的回调函数
    progress: undefined,   // 动画执行中的回调函数（该函数会随着动画执行被不断触发）
    complete: undefined,   // 动画结束时的回调函数
    display: undefined,    // 动画结束时设置元素的 css display 属性
    visibility: undefined, // 动画结束时设置元素的 css visibility 属性
    loop: false,           // 循环
    delay: false,          // 延迟
    mobileHA: true         // 移动端硬件加速（默认开启）

```



***

## 单独使用

```js
Velocity(that,{translateX:[200,400]},{duration:1000});
//[终点，起点]，默认起点为0
```

