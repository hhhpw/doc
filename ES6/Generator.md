# yield

### 定义

Generator 函数是 ES6 提供的一种异步编程解决方案。**返回的是一个 Iterator 对象。**

形式上有两个特征：1.使用星号。function\*(){}。2.函数体内使用 yield（译为产出）关键字。

```js
function* helloWorldGenerator() {
  yield "hello";
  yield "world";
  return "ending";
}

var hw = helloWorldGenerator();

hw.next();
// { value: 'hello', done: false }
hw.next();
// { value: 'world', done: false }
hw.next();
// { value: 'ending', done: true }
hw.next();
// { value: undefined, done: true }
//  已经遍历结束，多次调用也只会返回这个
```

### yield 表达式

由于 Generator 函数返回的遍历器对象，只有调用 next 方法才会遍历下一个内部状态，所以其实提供了一种可以暂停执行的函数。
yield 表达式就是暂停标志。
**yield 表达式总是返回 undefined**

遍历器对象的 next 方法的运行逻辑如下:

（1）**遇到 yield 表达式，就暂停执行后面的操作,并将紧跟在 yield 后面的那个表达式的值，作为返回的对象的 value 属性值**。

（2）下一次调用 next 方法时，再继续往下执行，直到遇到下一个 yield 表达式。

（3）如果没有再遇到新的 yield 表达式，就一直运行到函数结束，直到 return 语句为止，并将 return 语句后面的表达式的值，作为返回的对象的 value 属性值。

（4）如果该函数没有 return 语句，则返回的对象的 value 属性值为 undefined。

```js
function* f() {
  let v = yield "hello";
  console.log("v", v); //  undefined
}
g = f();
g.next(); // {value: "hello", done: false }
```

yield 表达式和 return。相似之处，在于都紧跟返回语句后面的那个表达式值。区别在于，yield 可以暂停函数的执行，下一次从该位置继续向后执行，而 return 语句不具备位置记忆的功能。一个函数里，只能执行一次（或者是一个）return,但可以执行多次（多个）yield。

---

### yield\* 表达式 (用来在一个 Generator 函数里执行另一个 Generator)

- **yield\* 等同于 for...of**
- 如果想在一个 generator 函数里调用另一个 generator，则必须使用 yield* g2()，
  如果 g2 里有 return 语句，let v2 = yield* g2()获取 return 值
- yield* 后面不是 generator 函数，而是紧跟一个具有 iterator 接口，则会被 yield* 遍历

```js
function* bar() {
  yield "A";
}
let read = (function* () {
  yield "hello"; // 没带星号 返回Hello
  yield* "hello"; // 带了星号，遍历string
  yield* bar();
})();

read.next().value; // "hello"
read.next().value; // "h"
read.next().value; // e
```

### next 方法的参数

**next(param) param 将会作为上一个 yield 表达式的返回值，因此在第一次使用 next 传递参数是无效的**

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
  return "DONE";
}

const wrapped = wrapper(gen);

let g = wrapped();
g.next("A");
```

#### for...of 循环

for...of 循环可以自动遍历 Generator 函数运行时生成的 Iterator 对象，并且不再需要调用 next 方法。
**一旦 next 方法返回对象的 done 属性为 true，for...of 循环就会终止，且不包含该返回对象，因此 return 返回的值，不会在 for...of 循环里。**

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
