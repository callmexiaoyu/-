# Promise多层回调嵌套
***
[TOC]
***
### 多层回调嵌套
```js
//传统通过异步函数，注入实参回调函数，回调函数等到异步任务执行结果，调用回调
//假设现有五个方程f1,f2,f3,f4,f5,每个函数等待必须按顺序执行，且每个函数均是异步，

function f1(f1形参, f2, f3, f4, f5) {

	//f1异步代码，获取f1实参后得到ret1，作为f2的实参

	var f2实参 = ret1;
	f2(f2实参, f3, f4, f5)
};

f1(f1实参, function(f2形参, f3, f4, f5) {

	//f2异步代码，获取f2实参后得到ret2，作为f3的实参

	var f3实参 = ret2;
	f3(f3实参, f4, f5);

}, function(f3形参, f4, f5) {

	//f3异步代码，获取f3实参后得到ret3，作为f4的实参

	var f4实参 = ret3;
	f3(f4实参, f4);

}, function(f4形参, f5) {

	//f4异步代码，获取f4实参后得到ret4，作为f5的实参

	var f5实参 = ret4;
	f5(f4实参);
	
}, function(f5形参) {

	//f5异步代码，获取f5实参后得到ret5，回调链结束
});
```
`以上是在f1调用时，注入了f2-f5的具体方法作为实参，函数实参，每一个函数代码最后添加，调用注入的函数参数，并注入实参，实现了一个f1实参，经过多层异步函数运算，得到结果`

`相比计算斐波拉切数列，回调调用自身，将函数作为实参输入，异步执行完调用实参函数`

+ 匿名函数添加函数名
+ 模块化
+ promise
***
***
### Promise构造函数
`一般获取异步函数内部执行结果采用，实参添加回调函数，异步函数内部将获得的结果作为回调函数参数执行，也就是要获取异步函数结果的代码，统统打包成回调函数`
```js
    var promise = new Promise(function(resolve, reject) {
        
        var num = 1;

        console.log(num+"promise")
        if(num===0){
        	resolve(num);
        }else{
        	reject(num);
        }
    });

    promise.then(function(num){
    	console.log(num+"resolve")
    },function(num){
    	console.log(num+"reject")
    });
```
`Promise本身是一个构造函数，实例化注入一个匿名回调函数，匿名函数有两个回调函数resolve和reject方法，由于构造函数实例化会执行内部代码，输出"promise"，而两个回调函数resolve和reject为异步，不立刻执行，需要对这两个回调函数注入具体函数，通过实例化then(f1,f2)，对两个回调函数赋值，f1=resolve，f2=reject`

```js
//相似
function pro(f1, f2) {
	var num = 0;
	if (num === 0) {
		f1(num);
	} else {
		f2(num);
	}
};
pro(function(num) {
	console.log(num + " f1");
}, function(num) {
	console.log(num + " f2")
})
```
### Promise处理回调嵌套
```js
//promise处理多层回调嵌套

var p1 = new Promise(function(resolve, reject) {

	var num = "p1";

	console.log(num + " p1 promise1")
	if (typeof num === "1string") {

		resolve(num);

	} else {
		reject(num);
	}
});

var p2 = new Promise(function(resolve, reject) {

	var num = "p2";

	console.log(num + " p2 promise2")
	if (typeof num === "1number") {

		resolve(num);

	} else {
		reject(num);
	}
});

var p3 = new Promise(function(resolve, reject) {

	var num = "p3";

	console.log(num + " p3 promise3")
	if (typeof num === "number") {

		resolve(num);

	} else {
		reject(num);
	}
});

p1
	.then(function(num) {

		console.log(num + " resolve p1")
		return p2;

	}, function(num) {

		console.log(num + " reject p1");
		throw new Error();

	})
	.catch(function(data) {

		console.log(data)

	})
	.then(function(num) {

		//未收到返回的promise实例对象，num为undefined
		console.log(num + " resolve p2")
		return p3;

	}, function(num) {

		console.log(num + " reject p2");
		throw new Error();

	})
	.catch(function(data) {
		//未处理的promise，此时data为未处理promise对象名称的字符串
		//抛出错误对象捕获
		console.log(typeof data, data);

	})
```
`回调函数本身用于使异步函数按顺序执行，但多层的回调函数嵌套导致结构不清晰`
`promise使异步函数按顺序加载，且逻辑清晰，且对每一个异步的执行结果进行分类`
`链式，对每一个异步都有执行成功和失败两个分支，这样产生一个完整成功的链，其余链产生的所有错误分支均由对应的promise对象的reject的处理`
```flow
st=>start: 每一个promise构造函数有
resolve和reject两个回调函数，用于获
取异步函数内部的执行结果，并处理，
根据执行结果调用相应的回调函数，
每一个promise实例对象的then（）
存在两个实参函数，对异步函数的结果分别处理
e=>end: 多层回调函数嵌套最后结果
op1=>operation: promise1的reject回调函数，异步失败处理
op2=>operation: promise2的reject回调函数，异步失败处理
op3=>operation: promise3的reject回调函数，异步失败处理
cond1=>condition: promise1的resolve回调函数，
最后写入return promise2实例
cond2=>condition: promise2的resolve回调函数，
最后写入return promise2实例
cond3=>condition:  promise2的resolve回调函数，
最后写入return promise2实例

st->cond1->cond2->cond3->e
cond1(no)->op1
cond1(yes)->cond2(yes)->cond3(yes)->e
cond1(yes)->cond2(no)->op2
cond1(yes)->cond2(yes)->cond3(no)->op3
```
原生多层回调函数嵌套的另一种写法
`闭包嵌套`
```js
function h1(x1) {
	console.log(x1);

	var num1 = x1 * 2;

	//判断
	return h2(num1);
}

function h2(x2) {
	console.log(x2);

	var num2 = x2 * 2;

	//判断
	return h3(num2);
}

function h3(x3) {
	console.log(x3);

	var num3 = x3 * 2;

	//判断
	return h4(num3);
}

function h4(x4) {
	console.log(x4);

	var num4 = x4 * 2;

	//判断
	return h5(num4);
}

function h5(x5) {
	console.log(x5);

}

h1(1);
```
定时器异步回调
```js
setTimeout(function(num) {
	num++;

	setTimeout(function(num) {
		num++;

		setTimeout(function(num) {
			num++;

			console.log(num);
		}, 1000, num);

	}, 1000, num);

}, 1000, 10)；
```
无作用域嵌套，变量传递污染
```js
!function s1(num){
	num++;

	return function(num){
		num++;

		return function(num){
			num++;

			return function(num){

				console.log(num)
			}(num)
		}(num)
	}(num)
}(10)
```
### Promise处理ajax
```js
    function getPromise(url) {
        return new Promise(function(resolve, reject) {
            var xhr = new XMLHttpRequest();
            xhr.open('GET', url, true);
            xhr.onreadystatechange = function() {
                // 通信成功时，状态值为4
                if (xhr.readyState === 4) {
                    if (xhr.status === 200) {
                        resolve(JSON.parse(xhr.responseText));
                    } else {
                        console.error(xhr.statusText);
                    }
                }
            };
            xhr.onerror = function(e) {
                console.error(xhr.statusText);
            };
            xhr.send(null);

        })
    }
    //根据获取的数据再进一步查询
    getPromise("/form?name=jack")
        .then(function(data) {
            console.log(data);
            两次获取的data不一样
            return getPromise("/form?job=" + data.job)
        })
        .then(function(data) {
            console.log(data)
        })
```
