 ## 实现new操作符
 ---

 ```js
function myNew(fn, ...args) {
    if (typeof fn !== 'function') {
        throw new Error('the firest parameter expectation is a function')
    }
    // 原型继承
    let newObj = Obj.create(fn.prototype);
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