# yield

### 定义

Generator函数是ES6提供的一种异步编程解决方案。**返回的是一个Iterator对象。**

形式上有两个特征：1.使用星号。function*(){}。2.函数体内使用yield（译为产出）关键字。


```js
function* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
  return 'ending';
}

var hw = helloWorldGenerator();

hw.next()
// { value: 'hello', done: false }
hw.next()
// { value: 'world', done: false }
hw.next()
// { value: 'ending', done: true }
hw.next()
// { value: undefined, done: true }
//  已经遍历结束，多次调用也只会返回这个
```

### yield 表达式
由于 Generator 函数返回的遍历器对象，只有调用next方法才会遍历下一个内部状态，所以其实提供了一种可以暂停执行的函数。
yield表达式就是暂停标志。
**yield表达式总是返回undefined**

遍历器对象的next方法的运行逻辑如下:

（1）**遇到yield表达式，就暂停执行后面的操作,并将紧跟在yield后面的那个表达式的值，作为返回的对象的value属性值**。

（2）下一次调用next方法时，再继续往下执行，直到遇到下一个yield表达式。

（3）如果没有再遇到新的yield表达式，就一直运行到函数结束，直到return语句为止，并将return语句后面的表达式的值，作为返回的对象的value属性值。

（4）如果该函数没有return语句，则返回的对象的value属性值为undefined。

```js
function* f() {
  let v = yield "hello";
  console.log("v", v) //  undefined
}
g = f();
g.next() // {value: "hello", done: false }

```


### yield* 表达式

- **yield* 等同于for...of**
- 如果想在一个generator函数里调用另一个generator，则必须使用yield* g2()，
如果g2里有return语句，let v2 = yield* g2()获取return值
- yield* 后面如果紧跟一个具有iterator接口，则会被yield* 遍历

```js
let read = (function* () {
  yield 'hello'; // 没带星号 返回Hello
  yield* 'hello'; // 带了星号，遍历string
})();

read.next().value // "hello"
read.next().value // "h"
read.next().value // e
```


### next方法的参数

**next(param) param将会作为上一个yield表达式的返回值，因此在第一次使用next传递参数是无效的**


```js
  // 嵌套一层使得next首次传参可以作为yield的值
  function wrapper(genFunc) {
    return function (...args) {
      let generatorObject = genFunc(...args);
      // 先执行一次next，再讲*函数返回
      generatorObject.next();
      return generatorObject;
    };
  }

  function* gen(a, v) {
    let t = yield;
    console.log("t", t); // A
    return 'DONE';
  }

  const wrapped = wrapper(gen);

  let g = wrapped();
  g.next("A")

```

#### for...of循环

for...of循环可以自动遍历Generator函数运行时生成的Iterator对象，并且不再需要调用next方法。
**一旦next方法返回对象的done属性为true，for...of循环就会终止，且不包含该返回对象，因此return返回的值，不会在for...of循环里。**


```js

// 斐波那契数列
function* fibonacci() {
  let [prev, curr] = [0, 1];
  for (;;) {
    yield curr;
    [prev, curr] = [curr, prev + curr];
  }
}

for (let n of fibonacci()) {
  if (n > 1000) break;
  console.log(n);
}
```