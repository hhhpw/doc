[参考资料](https://es6.ruanyifeng.com/#docs/set-map)

# Set

**接受具有 iterable 接口的其他数据结构作为参数**，因此对象将不能作为 new Set()参数。

# WeakSet

**不重复的值的集合**
**WeakSet 可以接受一个数组或类似数组的对象作为参数。（实际上，任何具有 Iterable 接口的对象，都可以作为 WeakSet 的参数。Symbol 等不可以**
**弱引用（成员对象会随时消失）**
**不可遍历**

```js
let a = [1, 2];
let ws = new WeakSet(a); // Invalid value used in weak set

let b = { name: "leo" };
let ws1 = new WeakSet(b); // object is not iterable (cannot read property Symbol(Symbol.iterator))

let c = [
  [1, 2],
  [3, 4]
];
let ws2 = new WeakSet(c); //{Array[2], Array[2]}
```

# Map

**一种值-》值的数据结构**

# WeakMap

**健值对中 key 只能是对象**
**key 所指的对象不计入垃圾回收机制**
