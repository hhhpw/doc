# mixin

## 合并策略

在 Vue 中，一个混入对象可以包含任意组件选项，但是对于不同的组件选项，会有不同的合并策略。

1.data 对于 data,在混入时会进行递归合并，如果两个属性发生冲突，则以组件自身为主。

2.对于生命周期钩子函数，混入时会将同名钩子函数加入到一个数组中，然后在调用时依次执行。混入对象里面的钩子函数会优先于组件的钩子函数执行。如果一个组件混入了多个对象，对于混入对象里面的同名钩子函数，将按照数组顺序依次执行

3.其他选项对于值为对象的选项，如 methods,components,filter,directives,props 等等，将被合并为同一个对象。两个对象键名冲突时，取组件对象的键值对。

```js
const mixin1 = {
  data() {
    return {
      name: "dd"
    };
  },
  created() {
    console.log("我是第一个输出的");
  }
};

const mixin2 = {
  data() {
    return {
      age: 10
    };
  },
  created() {
    console.log("我是第二个输出的");
  }
};
export default {
  mixins: [mixin1, mixin2],
  data() {
    return {
      name: "leo"
    };
  },
  created() {
    console.log("this.$data", this._data); // {age:10, name: "leo"}
    console.log("我是第三个输出的");
  }
};
```

## 自定义合并策略

```js
const optionMergeStrategies = Vue.config.optionMergeStrategies;
// myOpt的合并策略与created相同
optionMergeStrategies.myOpt = optionMergeStrategies.created;
```
