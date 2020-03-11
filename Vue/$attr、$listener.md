

[参考文档](https://blog.csdn.net/songxiugongwang/article/details/84001967?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)

组件结构如下：
A=>B=>C

A=>C 组件如何通信，属性传递，方法调用


## 名词解释

- $attrs–继承所有的父组件属性（除了prop传递的属性、class 和 style ）

- inheritAttrs：默认值true,继承所有的父组件属性（除props的特定绑定）作为普通的HTML特性应用在子组件的根元素上，如果你不希望组件的根元素继承特性设置inheritAttrs: false,但是class属性会继承
默认情况下父作用域的不被认作 props 的特性绑定 (attribute bindings) 将会“回退”且作为普通的 HTML 特性应用在子组件的根元素上。当撰写包裹一个目标元素或另一个组件的组件时，这可能不会总是符合预期行为。通过设置 inheritAttrs 到 false，这些默认行为将会被去掉。而通过 (同样是 2.4 新增的) 实例属性 $attrs 可以让这些特性生效，且可以通过 v-bind 显性的绑定到非根元素上。
注意：这个选项不影响 class 和 style 绑定。

- $listeners，它是一个对象，里面包含了作用在这个组件上的所有监听器，你就可以配合 v-on="$listeners" 将所有的事件监听器指向这个组件的某个特定的子元素。

主要用途
用在父组件传递数据给子组件或者孙组件

### 父组件

```vue
<template>
  <div>
    这是父
    <son :name="name"
         :age=age
         @m1="m1"></son>
  </div>
</template>
<script>
import son from "./son.vue";
export default {
  inheritAttrs: true,
  components: {
    son: son
  },
  data () {
    return {
      name: "leo",
      age: 12
    }
  },
  methods: {
    m1 (value) {
      console.log(value)
    },
    m2 () {
      console.log("M2")
    }
  }
}
</script>

```
### 子组件
```
<template>
  <div>
    这是son
    <p>name:{{$attrs.name}}</p>
    <p>attrs:{{$attrs}}</p>
    <grandson v-bind="$attrs"
              v-on="$listeners"></grandson>
  </div>
</template>
<script>
import grandson from "./grandson.vue";

export default {
  components: {
    grandson: grandson
  },
}
</script>

```
### 孙组件
```
<template>
  <div>这是grandSon
    <p>attrs:{{$attrs}}</p>
    <p>listeners:{{$listeners}}</p>
    <button @click="test">点我</button>
  </div>
</template>
<script>

export default {
  methods: {
    test () {
      this.$emit("m1", "grandson");
    }
  }
}
</script>

```

