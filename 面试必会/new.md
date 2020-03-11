 ## new原理
  - 创建一个新对象
  - 将构造函数内部指针指向新对象，添加属性和方法
  - 如果构造函数内没有返回其他对象，则返回这个新对象
 
1.创建了一个全新的对象。
2.这个对象会被执行[[Prototype]]（也就是__proto__）链接。
3.生成的新对象会绑定到函数调用的this。
4.通过new创建的每个对象将最终被[[Prototype]]链接到这个函数的prototype对象上。
5.如果函数没有返回对象类型Object(包含Functoin, Array, Date, RegExg, Error)，那么new表达式中的函数调用会自动返回这个新的对象。
 ## 实现new操作符
 ---

 ```js
function myNew(fn, ...args) {
    if (typeof fn !== 'function') {
        throw new Error('the firest parameter expectation is a function')
    }
    // 原型继承
    let newObj = Object.create(fn.prototype);
    // 改变this指向
    // 获取fn返回的返回的结果，如果在fn里没有返回Object类型（包括Date.Funciton
    // .Array.Error.RegExp）则会直接返回这些值。否则返回新的对象，即newObj
    let res = fn.apply(newObj, args);
    if ((typeof res === 'object' || typeof res === 'function') && res !== null) {
        return res
    }
    return newObj;
}
 ```