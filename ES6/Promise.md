[文档连接](https://es6.ruanyifeng.com/#docs/promise#Promise-%E7%9A%84%E5%90%AB%E4%B9%89)

#### Promise.all

Promise.all()方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。
**p 的状态由 p1、p2、p3 决定，分成两种情况。**
（1）只有 p1、p2、p3 的状态都变成 fulfilled，p 的状态才会变成 fulfilled，此时 p1、p2、p3 的返回值组成一个数组，传递给 p 的回调函数。

（2）只要 p1、p2、p3 之中有一个被 rejected，p 的状态就变成 rejected，此时第一个被 reject 的实例的返回值，会传递给 p 的回调函数。

#### Promise.race

```js
const p = Promise.race([p1, p2, p3]);
```

上面代码中，只要 p1、p2、p3 之中有一个实例率先改变状态，p 的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给 p 的回调函数。

#### Prmise.allSettled

Promise.allSettled()方法接受一组 Promise 实例作为参数，包装成一个新的 Promise 实例。只有等到所有这些参数实例都返回结果，不管是 fulfilled 还是 rejected，包装实例才会结束。

**该方法返回的新的 Promise 实例，一旦结束，状态总是 fulfilled，不会变成 rejected**。
状态变成 fulfilled 后，Promise 的监听函数接收到的参数是一个数组，每个成员对应一个传入 Promise.allSettled()的 Promise 实例。

```js
const resolved = Promise.resolve(42);
const rejected = Promise.reject(-1);

const allSettledPromise = Promise.allSettled([resolved, rejected]);

allSettledPromise.then(function (results) {
  console.log(results);
});
// [
//    { status: 'fulfilled', value: 42 },
//    { status: 'rejected', reason: -1 }
// ]
```

#### Promise.any

Promise.any()方法接受一组 Promise 实例作为参数，包装成一个新的 Promise 实例。
**只要参数实例有一个变成 fulfilled 状态，包装实例就会变成 fulfilled 状态；**
**如果所有参数实例都变成 rejected 状态，包装实例就会变成 rejected 状态。该方法目前是一个第三阶段的提案 。**

Promise.any()跟 Promise.race()方法很像，只有一点不同，**就是不会因为某个 Promise 变成 rejected 状态而结束。**

### Promise.resolve()

  （1）参数是一个 Promise 实例
  如果参数是 Promise 实例，那么 Promise.resolve 将不做任何修改、原封不动地返回这个实例。
  （2）参数是一个 thenable 对象
  thenable 对象指的是具有 then 方法的对象，比如下面这个对象。

  ```js
  let thenable = {
    then: function (resolve, reject) {
      resolve(42);
    }
  };
  ```

  Promise.resolve 方法会将这个对象转为 Promise 对象，然后就立即执行 thenable 对象的 then 方法。
  （3）参数不是具有 then 方法的对象，或根本就不是对象

  如果参数是一个原始值，或者是一个不具有 then 方法的对象，则 Promise.resolve 方法返回一个新的 Promise 对象，状态为 resolved。
  （4）不带有任何参数

  Promise.resolve()方法允许调用时不带参数，直接返回一个 resolved 状态的 Promise 对象。

* Promise.reject()

  Promise.reject(reason)方法也会返回一个新的 Promise 实例，该实例的状态为 rejected。

  **注意，Promise.reject()方法的参数，会原封不动地作为 reject 的理由，变成后续方法的参数。这一点与 Promise.resolve 方法不一致。**

* Promise.try

  实际开发中，经常遇到一种情况：不知道或者不想区分，函数 f 是同步函数还是异步操作，但是想用 Promise 来处理它。因为这样就可以不管 f 是否包含异步操作，都用 then 方法指定下一步流程，用 catch 方法处理 f 抛出的错误。

---

[Promise 限制并发数量](https://www.jianshu.com/p/cc706239c7ef)

[Promise 限制并发数量](https://blog.csdn.net/u012515877/article/details/104870757)

```js
function fetchImageWithLimit(imageUrls, limit) {
  // copy一份，作为剩余url的记录
  let urls = [...imageUrls];

  // 用来记录url - response 的映射
  // 保证输出列表与输入顺序一致
  let rs = new Map();

  // 递归的去取url进行请求
  function run() {
    if (urls.length > 0) {
      // 取一个，便少一个
      const url = urls.shift();
      // console.log(url, ' [start at] ', ( new Date()).getTime() % 10000)
      return fetchImage(url).then((res) => {
        // console.log(url, ' [end at] ', ( new Date()).getTime() % 10000)
        rs.set(url, res);
        return run();
      });
    }
  }

  // 当imageUrls.length < limit的时候，我们也没有必要去创建多余的Promise
  const promiseList = Array(Math.min(limit, imageUrls.length))
    // 这里用Array.protetype.fill做了简写，但不能进一步简写成.fill(run())
    .fill(Promise.resolve())
    //  继发，自己去执行
    .map((promise) => promise.then(run));

  return Promise.all(promiseList).then(() =>
    imageUrls.map((item) => rs.get(item))
  );
}
```

[参考资料](https://www.cnblogs.com/fuGuy/p/13112876.html)

```js
class PromisePool {
  constructor(max, fn) {
    this.max = max; //最大并发量
    this.fn = fn; //自定义的请求函数
    this.pool = []; //并发池
    this.urls = []; //剩余的请求地址
  }
  start(urls) {
    this.urls = urls; //先循环把并发池塞满
    while (this.pool.length < this.max) {
      let url = this.urls.shift();
      this.setTask(url);
    }
    //利用Promise.race方法来获得并发池中某任务完成的信号
    let race = Promise.race(this.pool);
    return this.run(race);
  }
  run(race) {
    race.then((res) => {
      //每当并发池跑完一个任务，就再塞入一个任务
      let url = this.urls.shift();
      this.setTask(url);
      return this.run(Promise.race(this.pool));
    });
  }
  setTask(url) {
    if (!url) return;
    let task = this.fn(url);
    this.pool.push(task); //将该任务推入pool并发池中
    console.log(`\x1B[43m ${url} 开始，当前并发数：${this.pool.length}`);
    task.then((res) => {
      //请求结束后将该Promise任务从并发池中移除
      this.pool.splice(this.pool.indexOf(task), 1);
      console.log(`\x1B[43m ${url} 结束，当前并发数：${this.pool.length}`);
    });
  }
}
//test
const URLS = [
  "bytedance.com",
  "tencent.com",
  "alibaba.com",
  "microsoft.com",
  "apple.com",
  "hulu.com",
  "amazon.com"
];
//自定义请求函数
var requestFn = (url) => {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(`任务${url}完成`);
    }, 1000);
  }).then((res) => {
    console.log("外部逻辑", res);
  });
};
const pool = new PromisePool(5, requestFn); //并发数为5
pool.start(URLS);
```
