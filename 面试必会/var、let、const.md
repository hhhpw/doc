### var

在 es5 中，var 声明的变量既是全局变量，也是顶层变量

**var 存在变量提升**

```js
var a;
console.log(a); // undefined
var a = 10;
```

**var 可以对一个变量进行多次声明,后面声明的变量会覆盖前面的变量声明**

**在函数中使用 var，则该变量是局部的。在函数中不使用 var，则该变量是全局的**

```js
var a = 20;
function change() {
  var a = 30;
}
change();
console.log(a); // 20

//////

var a = 20;
function change() {
  a = 30;
}
change();
console.log(a); // 30
```

### let

- 不存在变量提升
- 存在暂时性死缓区，必须先声明后使用
- 不允许在相同作用域下重复声明
- 不再挂载在 window 上

### const

- 声明一个常量，不能改变。一旦声明，必须立马赋值。
- 不能重复声明
- 不存在变量提升
- 存在暂时性死缓区，必须先声明后使用
- 不再挂载在 window 上

**const 实际上保证的并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动
对于简单类型的数据，值就保存在变量指向的那个内存地址，因此等同于常量
对于复杂类型的数据，变量指向的内存地址，保存的只是一个指向实际数据的指针，const 只能保证这个指针是固定的，并不能确保改变量的结构不变**

```js
const obj = {};
obj.a = "name"; // ok

obj = {} // TypeError: "obj" is read-only
```
