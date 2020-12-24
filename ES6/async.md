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

// 写法：
let foo = await getFoo();
let bar = await getBar();
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
// 并发:
// 写法一
let [foo, bar] = await Promise.all([getFoo(), getBar()]);

// 写法二
let fooPromise = getFoo();
let barPromise = getBar();
let foo = await fooPromise;
let bar = await barPromise;
```
## 为什么赋变量后就可以并发执行？？
[参考资料](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/async_function)


### 继发请求次序输出

```js

async function logInOrder(urls) {
  for (const url of urls) {
    const response = await fetch(url);
    console.log(await response.text());
  }
}
```


### 并发请求次序输出

```js

function logInOrder(urls) {
  // 远程读取所有URL
  const textPromises = urls.map(url => {
    return fetch(url).then(response => response.text());
  });

  // 按次序输出
  textPromises.reduce((chain, textPromise) => {
    return chain.then(() => textPromise)
      .then(text => console.log(text));
  }, Promise.resolve());
}

//////////////////////////////////////////////////////////////
async function logInOrder(urls) {
  // 并发读取远程URL
  const textPromises = urls.map(async url => {
    // async方法内部返回一个promise，这里作用在map参数方法里，相当于每次立刻返回一个promise函数，
    const response = await fetch(url);
    return response.text();
  });

  // 按次序输出
  for (const textPromise of textPromises) {
    console.log(await textPromise);
  }
}


// 一个例子
  let arr = [5, 1, 3, 4, 2];

function c(d, i) {
  console.log("B", i + 1);
  return new Promise((reslove) => {
    setTimeout(() => {
      console.log("item:", d, "i:", i + 1);
      reslove(d * 10);
    }, d * 1000);
  })
}

async function g(arr) {
  const arr1 = arr.map(async (d, i) => {
    console.log("A", i + 1);
    let res = await c(d, i);
    console.log("res", res)
    return res;
  });
  console.log("C")
  console.log("arr1", arr1);
  for (let gc of arr1) {
    console.log("gc", gc, await gc);
  }
}

g(arr);

// A 1
// 47 B 1
// 58 A 2
// 47 B 2
// 58 A 3
// 47 B 3
// 58 A 4
// 47 B 4
// 58 A 5
// 47 B 5
// 63 C
// 64 arr1 (5) [Promise, Promise, Promise, Promise, Promise]
// 50 item: 1 i: 2
// 60 res 10
// 50 item: 2 i: 5
// 60 res 20
// 50 item: 3 i: 3
// 60 res 30
// 50 item: 4 i: 4
// 60 res 40
// 50 item: 5 i: 1
// 60 res 50
// 66 gc Promise {<fulfilled>: 50} 50
// 66 gc Promise {<fulfilled>: 10} 10
// 66 gc Promise {<fulfilled>: 30} 30
// 66 gc Promise {<fulfilled>: 40} 40
66 gc Promise {<fulfilled>: 20} 20
```