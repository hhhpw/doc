## 什么是事件循环

**JS 语言是单线程的语言**，同一个时间只能做一件事。
原因在于 JS 是运行在浏览器端上的，用于用户交互，
如果是多线程，同一 DOM 的增删没法评辩优先级。

**单线程意味着所有任务都需要排队，前一个任务结束，后一个任务才能执行，
这样会十分浪费资源**

由此，任务分为两种。
**进入主线程同步任务（synchronous），
进入任务队列（task queue）的异步任务（asynchronous）。
只有任务队列通知主线程，某个异步任务可以执行了，该任务才会进入主线程执行。**

同步、异步任务运行机制：

（1）所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）。

（2）主线程之外，还存在一个"任务队列"（task queue）。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件。

（3）一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。

（4）主线程不断重复上面的第三步。

**主线程从任务队列中读取事件，这个过程是循环不断的，
所以整个的运行机制又称为 Event Loop(事件循环)**

**当主线程中的任务执行完成，将会去查找任务队列中是否有事件，如果有事件，
主线程会从中取出排在第一位的事件，并将执行这个事件的回调函数，然后执行其中的同步代码,
如此反复...**

## 异步任务（宏任务：macro task，微任务：micro task）

以上的事件循环过程是一个宏观的表述，实际上因为异步任务之间并不相同，因此他们的执行优先级也有区别。不同的异步任务被分为两类：微任务（micro task）和宏任务（macro task）。

**以下事件属于宏任务**：
setInterval()
setTimeout()
I/O
script 代码块

**以下事件属于微任务**：
new Promise()
new MutaionObserver()
setImmediate
nextTick
callback
process.nextTick
Object.observe

前面我们介绍过，在一个事件循环中，异步事件返回结果后会被放到一个任务队列中。**然而，根据这个异步事件的类型，这个事件实际上会被对应的宏任务队列或者微任务队列中去。并且在当前执行栈为空的时候，主线程会 查看微任务队列是否有事件存在。如果不存在，那么再去宏任务队列中取出一个事件并把对应的回到加入当前执行栈；如果存在，则会依次执行队列中事件对应的回调，直到微任务队列为空，然后去宏任务队列中取出最前面的一个事件，把对应的回调加入当前执行栈...如此反复，进入循环。**

我们只需记住当当前执行栈执行完毕时会立刻先处理所有微任务队列中的事件，然后再去宏任务队列中取出一个事件。同一次事件循环中，微任务永远在宏任务之前执行。

---
```js
async await 遇到await时，会先将await后的函数执行一遍，然后将await后面的代码加入micro队列中。

async function async1() {
    console.log('async1 start');// 同步。立即执行
    await async2(); // 同步。会执行
    console.log('async1 end'); // 进入微任务队列
}
async function async2() {
    console.log('async2');
}
async1()


console.log("script start")


setTimeout(function(){
    console.log('setTimeout')
});

new Promise(function(resolve){
    console.log('Promise start'); // promise实例建立后立即执行
    resolve();
}).then(function(){
    console.log('promise end')
});

console.log('script end');


// async1 start
// async2
// script start
//  Promise start
// script end
// async1 end
// promise end
// setTimeout

```

```js
new Promise((reslove, reject) => {
  console.log("1");
  reslove();
})
  .then(() => {
    console.log("2");
    new Promise((reslove, reject) => {
      console.log("3");
      reslove();
    })
      .then(() => {
        console.log("4");
      })
      .then(() => {
        console.log("5");
      })
      .then(() => {
        console.log("9");
      });
  })
  .then(() => {
    console.log("6");
  })
  .then(() => {
    console.log("7");
  })
  .then(() => {
    console.log("8");
  });

  promise实例会立即执行。then方法返回的是promise。
定义promise，记做p1，最开始输出1，然后reslove()，执行then方法。then方法后面又挂载then记住then1。
执行里面输出2。定义promise,记做p2。输出3，reslove()。输出4。 这是时候整个p2
reslove的过程就结束了。then方法后面又挂载then记住then2。 微
任务的执行顺序： then1 then2
1 2 3 4 6 5 7 9 8

```

[参考资料](https://segmentfault.com/a/1190000018675871)
