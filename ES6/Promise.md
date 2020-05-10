[文档连接](https://es6.ruanyifeng.com/#docs/promise#Promise-%E7%9A%84%E5%90%AB%E4%B9%89)

- Promise.all

Promise.all()方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。
**p的状态由p1、p2、p3决定，分成两种情况。**
（1）只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。

（2）只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值，会传递给p的回调函数。


- Promise.race
```js
const p = Promise.race([p1, p2, p3]);

```
上面代码中，只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给p的回调函数。

- Prmise.allSettled

Promise.allSettled()方法接受一组 Promise 实例作为参数，包装成一个新的 Promise 实例。只有等到所有这些参数实例都返回结果，不管是fulfilled还是rejected，包装实例才会结束。

**该方法返回的新的 Promise 实例，一旦结束，状态总是fulfilled，不会变成rejected**。
状态变成fulfilled后，Promise 的监听函数接收到的参数是一个数组，每个成员对应一个传入Promise.allSettled()的 Promise 实例。
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

- Promise.any

Promise.any()方法接受一组 Promise 实例作为参数，包装成一个新的 Promise 实例。
**只要参数实例有一个变成fulfilled状态，包装实例就会变成fulfilled状态；
如果所有参数实例都变成rejected状态，包装实例就会变成rejected状态。该方法目前是一个第三阶段的提案 。**

Promise.any()跟Promise.race()方法很像，只有一点不同，**就是不会因为某个 Promise 变成rejected状态而结束。**

- Promise.resolve()

  （1）参数是一个 Promise 实例
  如果参数是 Promise 实例，那么Promise.resolve将不做任何修改、原封不动地返回这个实例。
  （2）参数是一个thenable对象
    thenable对象指的是具有then方法的对象，比如下面这个对象。
    ```js
      let thenable = {
        then: function(resolve, reject) {
          resolve(42);
        }
      };
    ```
  Promise.resolve方法会将这个对象转为 Promise 对象，然后就立即执行thenable对象的then方法。
  （3）参数不是具有then方法的对象，或根本就不是对象

  如果参数是一个原始值，或者是一个不具有then方法的对象，则Promise.resolve方法返回一个新的 Promise 对象，状态为resolved。
  （4）不带有任何参数

  Promise.resolve()方法允许调用时不带参数，直接返回一个resolved状态的 Promise 对象。


- Promise.reject()

  Promise.reject(reason)方法也会返回一个新的 Promise 实例，该实例的状态为rejected。

  **注意，Promise.reject()方法的参数，会原封不动地作为reject的理由，变成后续方法的参数。这一点与Promise.resolve方法不一致。**

- Promise.try

  实际开发中，经常遇到一种情况：不知道或者不想区分，函数f是同步函数还是异步操作，但是想用 Promise 来处理它。因为这样就可以不管f是否包含异步操作，都用then方法指定下一步流程，用catch方法处理f抛出的错误。

---

  [Promise限制并发数量](https://www.jianshu.com/p/cc706239c7ef)

  [Promise限制并发数量](https://blog.csdn.net/u012515877/article/details/104870757)