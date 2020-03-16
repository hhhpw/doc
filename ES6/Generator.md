# yield








### yield* 表达式

- yield* 等同于for...of
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