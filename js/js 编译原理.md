# js 编译原理
***
[TOC]
***
### js预编译作用域链（作用域嵌套）
```js
//作用域嵌套
function f1() {
    var a = 1;
    f2();//该函数内部变量的作用域为声明时所在的作用域，嵌套不能读取
}

function f2() {
    console.log(a); //报错a is not defined
}

function f3() {
    var a = 1;
    //作用域未发生嵌套，因为声明在f3函数内部
    function f4() {
        console.log(a);//输出1
    }
    f4();
}
//f2();
f3();
```
`函数作用域为函数声明或函数表达式变量声明时所在的作用域，但函数或函数表达式在另一个函数外声明时，但在另一个函数内部调用，则导致作用域嵌套，此时该函数内部变量无法读取另一个函数内部变量，只能读取该函数或函数表达式变量声明时所在作用域的变量`
`变量会顺着作用域链上一级一级向上查找`
2. 回调函数作用域
```
//回调函数作用域链
function h1(callback){
    var a = 0;
    callback();//a is not defined
}
h1(function(){
    console.log(a)
});
```
`函数作为参数对传参，该函数则为回调函数，注意该函数的作用域为声明时所在作用域，匿名函数作为参数传参数时，导致作用域嵌套，匿名函数内部变量不受调用它的函数内部变量影响，互不干扰，匿名函数内部变量安全性得到保障`
3. 异步函数获取内部参数
```
function h1(callback){
    var a = 0;
    callback(a);//a is not defined
}
h1(function(a){
    console.log(a)
});
```
`以上回调函数虽然作用域嵌套，但通过调用时传参，拿到了函数内部需要的参数，其余依旧不能获取，调用时注入需要传递的参数即可，同时也解决了获取异步函数内部变量的问题，由于回调函数本身就是异步执行，获取异步函数方法只能在其执行完毕，执行后续代码，故该代码块也必须是异步，所以只能使用回调函数获取异步函数内参，同时需要传参，形参和实参都必须有，否则作用域嵌套无法读取`
***
### 预编译变量处理
```
1. **变量声明**
var num1 = 0;
var num1;
console.log(num1);//0

var num2;
var num2 = 1;
console.log(num2);//1
//var x = 10;相当于var x;x = 10;
```
当第二次遇到var num1，作用域链是否已经存在同名变量num1，有则忽略第二次变量声明，故输出第一次变量值0
没有则创建新的变量var num1;
当第二次遇到var num2，由于作用域链已经存在同名变量忽略该该变量声明，但是对第一个变量num2进行赋值操作
2. **函数声明处理**
```
function dmeo(){
	console.log(1);
}
function demo(){
	console.log(2);
}
demo();//输出2
```
相当于第二次函数声明无效，但对第一次函数声明重新赋值
3. **函数变量声明与var声明冲突处理**
```
//变量声明不赋值
var g1;
function g1() {
    console.log("g1");
}
console.log(g1);//函数


function g2() {
    console.log("g2");
}
var g2;
console.log(g2);//函数

//变量声明赋值
var g3 = 10;
function g3() {
    console.log("g3");
}
console.log(g3);//10


function g4() {
    console.log("g4");
}
var g4 = 10;
console.log(g4);//10

//函数表达式，作为普通变量声明，而非函数声明
var g5 = 10;
var g5 = function() {
    console.log("g5");
};
console.log(g5);//函数
//两者都是变量声明

var g6 = function() {
    console.log("g6");
};
var g6 = 10;
console.log(g6);//10
//两者都是变量声明
```
变量声明优先级高于函数声明，在作用域链上，当变量声明被赋值，无论先后顺序，则函数声明被取代，可能是出于函数声明堆内存比堆内存占用更多空间，出于优化角度，设计者角度


### 变量声明
1. 所有的全局变量是window的属性，但又不可删除，未声明的变量可删除
2. 函数声明和变量声明会提升到当前作用域顶端
3. 函数形参具有隐式声明
4. 作用域嵌套