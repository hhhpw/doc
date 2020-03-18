### let、const 


- const声明一个只读的常量，一旦声明，不可改变。let声明一个变量，可读可写。


- 不存在变量提升。必须先声明，后使用。


- 暂时性死区。先使用后声明变量，就会报错。


```js

var tmp = 123;

if (true) {
  tmp = 'abc'; // ReferenceError
  let tmp;
}
```

- 在同一作用域内，不允许重复声明

```js

 function f1() {
    let n = 5;
    if (true) {
      let n = 10;
      console.log("A", n);  // 10 
    }
    console.log("B", n); // 5
  }
```
- 块级作用域

- 块级作用域与函数声明

```js
1.允许在块级作用域内声明函数。
2.函数声明类似于var，即会提升到全局作用域或函数作用域的头部。
3.同时，函数声明还会提升到所在的块级作用域的头部。
```
这三条规则只对实现ES6的浏览器实现有效。


- let、const、class声明的全局变量，不属于顶层对象的属性。

```js
var a = "name";
window.a  // name
let b = "leo";
window.b // undefined
```
### var 

- 变量提升

- 不存在块级作用域的概念

- 可以重复命名变量




 