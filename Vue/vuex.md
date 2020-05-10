
#### 工作流程

　　1.在vue组件里面，通过dispatch来触发actions提交修改数据的操作。

　　2.然后再通过actions的commit来触发mutations来修改数据。

　　3.mutations接收到commit的请求，就会自动通过Mutate来修改state（数据中心里面的数据状态）里面的数据。

　　4.最后由store触发每一个调用它的组件的更新


　　Vuex的作用：项目数据状态的集中管理，复杂组件(如兄弟组件、远房亲戚组件)的数据通信问题。


#### 常见状态和属性

  1. state 存放数据的地方
  2. getter 获取数据，用作计算属性
  3. action  提交mutation,支持同步异步操作
  4. mutation  更改state,只能同步操作
  5. module 模块化vuex

#### 原理分析

下载vuex，Vue.use(vuex) 安装插件

调用Vuex中的install。 通过vue.mixin机制，借助vue的生命周期钩子函数beforeCreate完成。
即每个vue组件实例化过程中，会在beforeCreate里调用vuexInit。
其中VuexInit将store注册一个this.$store对象。
stroe在vuex为构造函数，因此必须使用new调用。
并在this.$store上注册actions等初始化对象。


```js
Vue.mixin({
  beforeCreate() {
    if (this.$options && this.$options.store) {
    //找到根组件 main 上面挂一个$store
      this.$store = this.$options.store
    // console.log(this.$store);

      } else {
    //非根组件指向其父组件的$store
      this.$store = this.$parent && this.$parent.$store
      }
    }
  })


getters和states变成响应式的原因？
    resetStoreVM(this, state)
原理就是将state作为一个vue实例的data属性传入。


```