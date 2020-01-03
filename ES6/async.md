  

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