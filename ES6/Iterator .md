[参考文档](https://es6.ruanyifeng.com/#docs/iterator)

#### 定义

目前，ES 中共有四个集合数据类型。包括 Array/Object/Map/Set。
需要一种统一的接口来访问四种数据类型。
遍历器（Iterator）就是这样一种机制。它是一种接口，为各种不同的数据结构提供统一的访问机制。
**任何数据结构只要部署 Iterator 接口，就可以完成遍历操作（即依次处理该数据结构的所有成员）。这也是 object 没有部署 iterator 接口的原因！！！**
**凡是部署了 Symbol.iterator 接口，都可以使用遍历器**

```js
let obj = { name: "ddd" };

for (let key of obj) {
  console.log(key); // obj is not iterable
}
```

#### 作用

1. 为各种数据接口提供访问的接口
2. 使得数据结构的成员能按某种次序排列
3. ES6 创造了 for...of 循环，iterator 接口主要供 for...of 消费

#### 遍历过程

1.创建一个指针对象，指向当前数据结构的起始位置。**也就是说，遍历器对象本质上，就是一个指针对象**。

2.第一次调用指针对象的 next 方法，可以将指针指向数据结构的第一个成员。

3.第二次调用指针对象的 next 方法，指针就指向数据结构的第二个成员。

4.不断调用指针对象的 next 方法，直到它指向数据结构的结束位置。

- 一个模拟迭代器例子

```js
//  done: false和value: undefined属性都是可以省略的
var it = makeIterator(["a", "b"]);
it.next(); // { value: "a", done: false }
it.next(); // { value: "b", done: false }
it.next(); // { value: undefined, done: true }
function makeIterator(array) {
  var nextIndex = 0;
  return {
    next: function () {
      return nextIndex < array.length
        ? { value: array[nextIndex++], done: false }
        : { value: undefined, done: true };
    }
  };
}
```

- 为对象添加 Symbol.iterator,使得对象可以遍历

```js
let obj = {
  name: "leo",
  age: 12,
  sex: "boy"
};

obj[Symbol.iterator] = function () {
  let keyArr = Object.keys(obj);
  let nextIndex = 0;
  return {
    next: function () {
      return nextIndex < keyArr.length
        ? {
            value: obj[keyArr[nextIndex++]],
            done: false
          }
        : { value: undefined, done: true };
    }
  };
};
for (let value of obj) {
  console.log("value", value);
  // leo 12 boy
}

let i = obj[Symbol.iterator]();
console.log("i", i, i.next(), i.next());
// i {next: ƒ} {value: "leo", done: false} {value: 12, done: false}
```
