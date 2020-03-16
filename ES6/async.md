  # Async

    
  ## async返回Promise对象
  ```js
  async function f() {
    return "hello world";
  }
  f().then(res => console.log(res));
  ```
  如果内部抛出错误，Promise对象变为reject状态，抛出的错误对象被catch方法回调函数接收到

  ## Promise对象状态变化
   async函数返回的Promise对象，必须等到await命令后面的Promise对象执行完，才会发生状态改变，除非遇到return语句或者抛出错误。**也就是说，只有async函数内部的异步操作执行完成，才会执行then方法里面的方法**
   

  ## await
  
  **正常情况下await后面命名是一个promise对象，返回对象的结果。如果不是Promise对象，就直接返回对应的值。**
  
  **如果该promise对象为reject状态，会被catch捕获，且整个async函数都会被中断执行**

  **如果await命名后面是一个thenable对象，即定义then方法的对象，那么await会将其等同于Promise对象,执行该对象的then方法**


  ## 继发执行
  ```js
  function ajaxA(url) {
    console.log("B");
    return fetch(url).then(res => res.json().then(data => { console.log("ajax--A"); return data; }));
  }
  function ajaxB(url) {
    console.log("C");
    return fetch(url).then(res => res.json().then(data => { console.log("ajax--B"); return data; }));
  }
  async function test() {
    console.log("A");
    let foo = await ajaxA(urls[0]);
    console.log("D");
    let bar = await ajaxB(urls[1]);
    console.log("E");
  }
  test();
// A
// B
// ajax--A
// D
// C
// ajax--B
// E
```

## 并发执行

```js
  function ajaxA(url) {
    console.log("B");
    return fetch(url).then(res => res.json().then(data => { console.log("ajax--A"); return data; }));
  }
  function ajaxB(url) {
    console.log("C");
    return fetch(url).then(res => res.json().then(data => { console.log("ajax--B"); return data; }));
  }

  async function test() {
    console.log("A");
    let foo = ajaxA(urls[0]);
    console.log("D");
    let bar = ajaxB(urls[1]);
    console.log("E");

    let a = await foo;
    console.log("F");
    let b = await bar;
    console.log("G");
  }

  test();
// A
// B
// D
// C
// E
// ajax--A
// F
// ajax--B
// G
```
## 为什么赋变量后就可以并发执行？？
[参考资料](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/async_function)