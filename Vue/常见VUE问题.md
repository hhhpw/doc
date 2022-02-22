#### 为何只有一个根元素？

1. **new APP 传入一个 el 选项**

Vue 其实并不知道哪一个才是我们的入口，因为对于一个入口来讲，这个入口就是一个‘Vue 类’，Vue 需要把这个入口里面的所有东西拿来渲染，处理，最后再重新插入到 dom 中。
如果同时设置了多个入口，那么 vue 就不知道哪一个才是这个‘类’。

2. **template 里层都需要一个 div 包裹**

首先看一看 template 这个标签，这个标签是 html5 的新标签，有三个特性：

隐藏性：不会显示在页面中
任意性：可以写在页面的任意地方
无效性： 没有一个根元素包裹，任何 HTML 内容都是无效的。

每一个.Vue 的单文件组件本质就是一个 vue 实例，既然是一个 vue 实例，它就要有一个入口，如果有多个 div，就不无法指定这个 vue 实例的根入口。

就像一个 HTML 文档只能有一个根元素一样，多个根元素必将导致无法构成一颗树，所以解释了\<template>\</template>只有一个\<div>根元素。

#### vue 生命周期？

beforeCreate（创建前） 在数据观测和初始化事件还未开始
created（创建后） 完成数据观测，属性和方法的运算，初始化事件，$el属性还没有显示出来
  beforeMount（载入前） 在挂载开始之前被调用，相关的render函数首次被调用。实例已完成以下的配置：编译模板，把data里面的数据和模板生成html。注意此时还没有挂载html到页面上。
  mounted（载入后） 在el被新创建的vm.$el 替换，并挂载到实例上去之后调用。实例已完成以下的配置：用上面编译好的 html 内容替换 el 属性指向的 DOM 对象。完成模板中的 html 渲染到 html 页面中。此过程中进行 ajax 交互。**在这过程中 DOM 渲染完成**
beforeUpdate（更新前） 在数据更新之前调用，发生在虚拟 DOM 重新渲染和打补丁之前。可以在该钩子中进一步地更改状态，不会触发附加的重渲染过程。
updated（更新后） 在由于数据更改导致的虚拟 DOM 重新渲染和打补丁之后调用。调用时，组件 DOM 已经更新，所以可以执行依赖于 DOM 的操作。然而在大多数情况下，应该避免在此期间更改状态，因为这可能会导致更新无限循环。该钩子在服务器端渲染期间不被调用。
beforeDestroy（销毁前） 在实例销毁之前调用。实例仍然完全可用。
destroyed（销毁后） 在实例销毁之后调用。调用后，所有的事件监听器会被移除，所有的子实例也会被销毁。该钩子在服务器端渲染期间不被调用。

#### 第一次页面渲染会触发哪些生命周期？

beforeCreate, created, beforeMount, mounted

#### data 为什么是函数？

因为组件是可以复用的,JS 里对象是引用关系,如果组件 data 是一个对象,那么子组件中的 data 属性值会互相污染,产生副作用。

所以一个组件的 data 选项必须是一个函数,因此每个实例可以维护一份被返回对象的独立的拷贝。new Vue 的实例是不会被复用的,因此不存在以上问题。

#### vue 常用修饰符

.prevent: 提交事件不再重载页面；
.stop: 阻止单击事件冒泡；
.self: 当事件发生在该元素本身而不是子元素的时候会触发；
.capture: 事件侦听，事件发生的时候会调用

#### 计算属性

在模板中放入太多的逻辑会让模板过重且难以维护，在需要对数据进行复杂处理，且可能多次使用的情况下，尽量采取计算属性的方式。 1.使得数据处理结构清晰； 2.依赖于数据，数据更新，处理结果自动更新； 3.计算属性内部 this 指向 vm 实例； 4.在 template 调用时，直接写计算属性名即可； 5.常用的是 getter 方法，获取数据，也可以使用 set 方法改变数据； 6.相较于 methods，不管依赖的数据变不变，methods 都会重新计算，但是依赖数据不变的时候 computed 从缓存中获取，不会重新计算。

#### comupted 和 watcher 的区别

computed 特性 1.是计算值， 2.应用：就是简化 tempalte 里面{{}}计算和处理 props 或\$emit 的传值 3.具有缓存性，页面重新渲染值不变化,计算属性会立即返回之前的计算结果，而不必再次执行函数

**遍历 computed 属性，创建 wathcer 实例，将 computed 与 data 混入 vm 实例，因此这也是 computed 属性 key 不能与 data key 重名的原因。**

computed 本质是一个惰性求值的观察者。
computed 内部实现了一个惰性的 watcher,也就是 computed watcher,computed watcher 不会立刻求值,同时持有一个 dep 实例。
其内部通过 this.dirty 属性标记计算属性是否需要重新求值。
当 computed 的依赖状态发生改变时,就会通知这个惰性的 watcher,
computed watcher 通过 this.dep.subs.length 判断有没有订阅者,

依赖收集、动态计算的流程：

1. data 属性初始化 getter setter
2. computed 计算属性初始化，提供的函数将用作属性 vm.reversedMessage 的 getter
3. 当首次获取 reversedMessage 计算属性的值时，Dep 开始依赖收集
4. 在执行 message getter 方法时，如果 Dep 处于依赖收集状态，则判定 message 为 reversedMessage 的依赖，并建立依赖关系
5. 当 message 发生变化时，根据依赖关系，触发 reverseMessage 的重新计算

watch 特性 1.是观察的动作， 2.应用：监听 props，\$emit 或本组件的值执行异步操作 3.无缓存性，页面重新渲染时值不变化也会执行

#### Vue 中组件生命周期调用顺序说一下

组件的调用顺序都是先父后子,渲染完成的顺序是先子后父。

组件的销毁操作是先父后子，销毁完成的顺序是先子后父。

#### 如何检测 data 中数组的变化

重写数组原型链方法，指向自定义的数组原型方法，当调用数组 api 时，就可以通知依赖更新。如果数组中有引用类型，会对数组中的应用类型再次检测。（遍历数组中的每一项，对元素调用 observe 方法，进行深度观测）

#### .vue 如何转化为.js 模块

主要是由于 vue-loader。它会将 template、js、css 分成不同的模块。如

```js
var MODULE_0__ = __webpack_require__(
  "./test.vue?vue&type=template&id=13429420&scoped=true&"
);
var MODULE_1__ = __webpack_require__("./test.vue?vue&type=script&lang=js&");
var MODULE_2__ = __webpack_require__(
  "./test.vue?vue&type=style&index=0&id=13429420&scoped=true&lang=scss&"
);
var MODULE_3__ = __webpack_require__(
  "./lib/vue-loader/runtime/componentNormalizer.js"
);
```

在得到上述的 request 之后，webpack 会先使用 vue-loader 处理，然后再使用 template-loader 来处理，然后得到最后模块。
首先，通过 compile 编译器把 template 编译成 AST 语法树（abstract syntax tree 即 源代码的抽象语法结构的树状表现形式），compile 是 createCompiler 的返回值，createCompiler 是用以创建编译器的。另外 compile 还负责合并 option。
然后，AST 会经过 generate（将 AST 语法树转化成 render funtion 字符串的过程）得到 render 函数，render 的返回值是 VNode，VNode 是 Vue 的虚拟 DOM 节点，里面有（标签名、子节点、文本等等）

#### modal 组件的封装

[参考资料](https://segmentfault.com/a/1190000038928664)
主要使用 h 函数渲染。

#### 错误捕获

- 后端接口类型的（axios,interceptor 进行网络层面的接口请求）

```js
apiClient.interceptors.response.use(
  (response) => {
    return response;
  },
  (error) => {
    if (error.response.status == 401) {
      router.push({ name: "Login" });
    } else {
      message.error("出错了");
      return Promise.reject(error);
    }
  }
);
```

- 代码逻辑问题

```js
Vue.config.errorHandler = function (err, vm, info) {
  // handle error
  // `info` 是 Vue 特定的错误信息，比如错误所在的生命周期钩子
  // 只在 2.2.0+ 可用
};
// 3
app.config.errorHandler = function (err, vm, info) {};
```
