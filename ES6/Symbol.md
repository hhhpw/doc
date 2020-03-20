

# Symbol

JS第七种原始数据类型。

Symbol值通过Symbol函数生成。Symbol()返回的是一个独一无二的值。

```js
let s1 = Symbol();
let s2 = Symbol();

typeof s1 // "symbol"
s1 === s2  // false

let s3 = Symbol("f");
let s4 = Symbol("f");
s3 === s4 // false

// 提供description实例属性，直接返回symbol
s3.description // "f"

```

[参考资料](https://es6.ruanyifeng.com/#docs/symbol)

## 属性名的使用

**作为属性名时，不能使用点运算符**
```js

let mySymbol = Symbol();

// 第一种写法
let a = {};
a[mySymbol] = 'Hello!';

// 第二种写法
let a = {
  [mySymbol]: 'Hello!'
};

// 第三种写法
let a = {};
Object.defineProperty(a, mySymbol, { value: 'Hello!' });

// 以上写法都得到同样结果
a[mySymbol] // "Hello!"

```


## 属性名的遍历

symbol 作为属性名，遍历对象的时候，该属性不会出现在for...in、for...of循环中，
也不会被Object.keys()、Object.getOwnPropertyNames()、JSON.stringify()返回。


但是，它也不是私有属性，有一个Object.getOwnPropertySymbols()方法，可以获取指定对象的所有 Symbol 属性名。
该方法返回一个数组，成员是当前对象的所有用作属性名的 Symbol 值。


**另一个新的 API，Reflect.ownKeys()方法可以返回所有类型的键名，包括常规键名和 Symbol 键名。**

```js
const obj = {};
let a = Symbol('a');
let b = Symbol('b');

obj[a] = 'Hello';
obj[b] = 'World';

const objectSymbols = Object.getOwnPropertySymbols(obj);

objectSymbols
// [Symbol(a), Symbol(b)]

```

## Symbol.for()，Symbol.keyFor()

Symbol.for()，它接受一个字符串作为参数，然后搜索有没有以该参数作为名称的 Symbol 值。
如果有，就返回这个 Symbol 值，否则就新建一个以该字符串为名称的 Symbol 值
并将其注册到全局。
```js
let s1 = Symbol.for('foo');
let s2 = Symbol.for('foo');

s1 === s2 // true
```
Symbol.keyFor()方法返回一个已登记的 Symbol 类型值的key。
**注意，Symbol.for()为 Symbol 值登记的名字，是全局环境的，不管有没有在全局环境运行。**
```js

let s1 = Symbol.for("foo");
Symbol.keyFor(s1) // "foo"

let s2 = Symbol("foo"); // 没有登记
Symbol.keyFor(s2) // undefined
```