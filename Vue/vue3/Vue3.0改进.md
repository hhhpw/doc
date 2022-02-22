### ref 、toRefs、reactive 的区别

[参考资料](https://zhuanlan.zhihu.com/p/267967246)
[参考资料](https://vue3js.cn/docs/zh/guide/reactivity-fundamentals.html#%E5%93%8D%E5%BA%94%E5%BC%8F%E7%8A%B6%E6%80%81%E8%A7%A3%E6%9E%84)

- ref 的作用就是将一个原始数据类型（primitive data type）转换成一个带有响应式特性
- reactive 复制数据类型 object 赋予响应特性
- toRefs，它可以将一个响应型对象(reactive object) 转化为普通对象(plain object)，
  同时又把该对象中的每一个属性转化成对应的响应式属性(ref)。说白了就是放弃该对象(Object)本身的响应式特性(reactivity)，转而给对象里的属性赋予响应式特性(reactivity)

### setup

在执行 setup 时还没创建组件，因此 setup 没有 this。

setup 接收两个参数 props、context

**props 是响应式的，不能使用解构，因为会消除 props 的响应性。如果一定要解构，使用 toRefs**

```js
const { title } = toRefs(props);
```

context 是一个普通的 javascript 对象，暴露三个属性。也就是说不是响应式的，
可以使用解构。

```js
// Attribute (非响应式对象)
console.log(context.attrs);

// 插槽 (非响应式对象)
console.log(context.slots);

// 触发事件 (方法)
console.log(context.emit);
```

执行 setup 时，组件实例尚未被创建。因此，你只能访问以下 property：

```js
props;
attrs;
slots;
emit;
```

换句话说，你将无法访问以下组件选项：

```js
data;
computed;
methods;
```
