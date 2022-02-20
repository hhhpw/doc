### watchEffect\watch 区别

1. watchEffect 不需要指定监听的熟悉，会自动收集依赖，一旦依赖属性发生改变就会触发 watchEffect 回调
2. watch 可以获取到旧值和新值
3. watchEffect 如果存在的话，在组件初始化的时候就会执行一次用以收集依赖（与 computed 同理），而后收集到的依赖发生变化，这个回调才会再次执行，而 watch 不需要，因为他一开始就指定了依赖

```js
watchEffect(
  () => {
    /* ... */
  },
  {
    flush: "post",
    // 默认为pre
  }
);

// pre：在组件更新前去执行副作用
// post: 在组件更新后重新运行侦听器副作用
// sync: 将强制效果始终同步触发
```
