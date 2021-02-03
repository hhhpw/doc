## 对象

Vue 无法检测 property 的添加或移除。由于 Vue 会在初始化实例时对 property 执行 getter/setter 转化，
所以 property 必须在 data 对象上存在才能让 Vue 将它转换为响应式的

```js
var vm = new Vue({
  data: {
    a: 1
  }
});

// `vm.a` 是响应式的

vm.b = 2;
// `vm.b` 是非响应式的
```

添加响应式 property

```js
Vue.set(vm.someObject, "b", 2);
this.$set(this.someObject, "b", 2);
// 添加多个
// 代替 `Object.assign(this.someObject, { a: 1, b: 2 })`
this.someObject = Object.assign({}, this.someObject, { a: 1, b: 2 });
```

## 对于数组

Vue 不能检测以下数组的变动：

当你利用索引直接设置一个数组项时，例如：vm.items[indexOfItem] = newValue
当你修改数组的长度时，例如：vm.items.length = newLength

```js
var vm = new Vue({
  data: {
    items: ["a", "b", "c"]
  }
});
vm.items[1] = "x"; // 不是响应性的
vm.items.length = 2; // 不是响应性的
```

解决

```js
// 第一类
// Vue.set
Vue.set(vm.items, indexOfItem, newValue);
// Array.prototype.splice
vm.items.splice(indexOfItem, 1, newValue);

// 第二类
vm.items.splice(newLength);
```


```js

export function set (target: Array<any> | Object, key: any, val: any): any {
  if (process.env.NODE_ENV !== 'production' &&
    (isUndef(target) || isPrimitive(target))
  ) {
    warn(`Cannot set reactive property on undefined, null, or primitive value: ${(target: any)}`)
  }　　// 判断目标值是否为数组，并且key值是否为有效的数组索引
  if (Array.isArray(target) && isValidArrayIndex(key)) {　　// 对比数组的key值和数组长度，取较大值设置为数组的长度
    target.length = Math.max(target.length, key)　　// 替换目标值
    target.splice(key, 1, val)
    return val
  }　　// 如果目标值是对象，并且key值是目标值存在的有效key值，并且不是原型上的key值
  if (key in target && !(key in Object.prototype)) {　　// 直接更改目标值
    target[key] = val
    return val
  }
  const ob = (target: any).__ob__ // 判断目标值是否为响应式的
  if (target._isVue || (ob && ob.vmCount)) { // 如果是vue根实例，就警告
    process.env.NODE_ENV !== 'production' && warn(
      'Avoid adding reactive properties to a Vue instance or its root $data ' +
      'at runtime - declare it upfront in the data option.'
    )
    return val
  }
  if (!ob) { // 如果目标值不是响应式的，那么值需要给对应的key赋值
    target[key] = val
    return val
  }　　// 其他情况，目标值是响应式的，就通过Object.defineProperty进行数据监听
  defineReactive(ob.value, key, val)　　// 通知更新dom操作
  ob.dep.notify()
  return val
}
```

大概流程就是：

　　1.判断目标值是否为有效值，不是有效值直接停止

　　2.判断是否为数组，并且key值是否为有效的key值

　　　　如果是数组，就选择数组的长度和key值取较大值作为数组的新的length值，并且替换目标值

　　　　splice方法，重写了，所以执行splice，会双向数据绑定

　　3.判断目标值是否为响应式的__ob__

　　　　如果是vue实例，直接不行

　　　　如果不是响应式的数据，就是普通的修改对象操作

　　　　如果是响应式数据，那就通过Object.defineProperty进行数据劫持

　　4.通知dom更新