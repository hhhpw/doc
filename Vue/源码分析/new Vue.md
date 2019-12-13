
```js
// /src/core/index
initGlobalAPI(Vue)
Object.defineProperty(Vue.prototype, '$isServer', {
  get: isServerRendering
})

Object.defineProperty(Vue.prototype, '$ssrContext', {
  get () {
    /* istanbul ignore next */
    return this.$vnode && this.$vnode.ssrContext
  }
})

// expose FunctionalRenderContext for ssr runtime helper installation
Object.defineProperty(Vue, 'FunctionalRenderContext', {
  value: FunctionalRenderContext
})

Vue.version = '__VERSION__'

export default Vue

```
在instance里引入Vue对象，调用initGlobalAPI方法。
在initGlobalAPI方法里,对Vue对象进行扩展,例如extend、nextTick、directive、filter、components等

```js
// /src/instance/index
// vue是一个函数一个类，往原型上添加方法
function Vue (options) {
  if (process.env.NODE_ENV !== 'production' &&
    !(this instanceof Vue)
  ) {
    warn('Vue is a constructor and should be called with the `new` keyword')
  }
  this._init(options)
}

  initMixin(Vue)
  // const vm: Component = this
  // vm指定为this
  // vm.$options = mergeOptions(
  //   resolveConstructorOptions(vm.constructor),
  //   options || {},
  //  vm
  // )
  // 这里将new Vue(options)进行合并挂载在$options里
  // initinitState(vm)
  stateMixin(Vue)
  eventsMixin(Vue)
  lifecycleMixin(Vue)
  renderMixin(Vue)

  export default Vue

```

## 为什么可以直接this[key]访问data里定义的属性

```js
// /src/instance/state
export function initState(vm: Component) {
  vm._watchers = []
  const opts = vm.$options
  if (opts.props) initProps(vm, opts.props)
  if (opts.methods) initMethods(vm, opts.methods)
  if (opts.data) {
    initData(vm)
  } else {
    observe(vm._data = {}, true /* asRootData */)
  }
  if (opts.computed) initComputed(vm, opts.computed)
  if (opts.watch && opts.watch !== nativeWatch) {
    initWatch(vm, opts.watch)
  }

  function initData(vm: Component) {
    // 在这里vm.$options.data赋值给data= vm_data
    // 也就是我们this.data可以访问vm..$options.data的原因
    let data = vm.$options.data
    data = vm._data = typeof data === 'function'
      ? getData(data, vm)
      : data || {}
    ...
    // 对vm_data进行getter setter
    // 其实内部都是在对_data做处理，外部暴露为data
    proxy(vm, `_data`, key)
  }

  export function proxy(target: Object, sourceKey: string, key: string) {
  // proxy(vm, `_data`, key)
  
  sharedPropertyDefinition.get = function proxyGetter() {
    return this[sourceKey][key]
  }
  sharedPropertyDefinition.set = function proxySetter(val) {
    this[sourceKey][key] = val
  }
  // 在这里对vm实例上data的key进行proxy this.key === this._data.key
  Object.defineProperty(target, key, sharedPropertyDefinition)
}


```


