[参考资料](https://juejin.im/post/593021272f301e0058273468)

[参考资料](https://juejin.im/entry/585b5a198d6d810065d1a29e)

## MVVM(Model-View-ViewModel)

最早由微软提出

MVVM 由 Model,View,ViewModel 三部分构成，Model 层代表数据模型，也可以在 Model 中定义数据 View 代表 UI 组件，它负责将数据模型转化成 UI 展现出来，ViewModel 是一个同步 View 和 Model 的对象，定义修改和操作逻辑。

在 MVVM 架构下，View 和 Model 之间并没有直接的联系，而是通过 ViewModel 进行交互，Model 和 ViewModel 之间的交互是双向的， 因此 View 数据的变化会同步到 Model 中，而 Model 数据的变化也会立即反应到 View 上。

ViewModel 通过双向数据绑定把 View 层和 Model 层连接了起来，而 View 和 Model 之间的同步工作完全是自动的，无需人为干涉，因此开发者只需关注业务逻辑，不需要手动操作 DOM, 不需要关注数据状态的同步问题，复杂的数据状态维护完全由 MVVM 来统一管理。

- Model

把 Model 称为数据层,因为仅仅关心数据本身，不关心任何行为

```js
var data = {
  name: "leo",
};
```

- View

MVVM 中的 View 通过使用模板语法来声明式的将数据渲染进 DOM，当 ViewModel 对 Model 进行更新的时候，会通过数据绑定更新到 View

```js
<div id="myapp">
  <div>
    <span>{{ val }}rmb</span>
  </div>
  <div>
    <button v-on:click="sub(1)">-</button>
    <button v-on:click="add(1)">+</button>
  </div>
</div>
```

- ViewModel

```js
new Vue({
  el: "#myapp",
  data: data,
  methods: {
    add(v) {
      if (this.val < 100) {
        this.val += v;
      }
    },
    sub(v) {
      if (this.val > 0) {
        this.val -= v;
      }
    },
  },
});
```

当 Model 发生变化，ViewModel 就会自动更新；ViewModel 变化，Model 也会更新。

```js
// 依赖收集
const Dep = {
  events: [],
  addListener(key, fn) {
    if (!this.events[key]) {
      this.events[key] = [];
    }
    this.events[key].push(fn);
  },
  trigger(key) {
    const fns = this.events[key];

    if (!fns || fns.length === 0) {
      return;
    }

    for (let i = 0, fn; (fn = fns[i++]); ) {
      fn.apply(this, arguments);
    }
  },
};
```
