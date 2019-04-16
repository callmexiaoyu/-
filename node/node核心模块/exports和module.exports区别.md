# exports和module.exports区别
***
实际通过moudule.exports将指向的对象或原型类型的数值，作为引用文件的require()的返回值，node简化，增添了exports变量，且exports = module.exports，为exports对象添加属性，exports.属性=xxx，等于module.exports.属性=xxx，而exports={}，则exports指向新的对象堆地址，此时exports ！== module.exports，而module.exports指向的对象栈地址未发生变化，建议使用module.exports赋值