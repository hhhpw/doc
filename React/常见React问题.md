### 什么是高阶组件



高阶组件（HOC）是 React 中用于复用组件逻辑的一种高级技巧。HOC 自身不是 React API 的一部分，它是一种基于 React 的组合特性而形成的设计模式。

具体而言，高阶组件是参数为组件，返回值为新组件的函数。


高阶组件是个纯函数，没有副作用。

使用事项：
1.不要改变原始组件，使用组合。
2.将不相关的 props 传递给被包裹的组件
3.约定：最大化可组合性

### 组件的生命周期

    componentWillMount -- 多用于根组件中的应用程序配置
    componentDidMount -- 在这可以完成所有没有 DOM 就不能做的所有配置，并开始获取所有你需要的数据；如果需要设置事件监听，也可以在这完成
    componentWillReceiveProps -- 这个周期函数作用于特定的 prop 改变导致的 state 转换
    shouldComponentUpdate -- 如果你担心组件过度渲染，shouldComponentUpdate 是一个改善性能的地方，因为如果组件接收了新的 prop， 它可以阻止(组件)重新渲染。shouldComponentUpdate 应该返回一个布尔值来决定组件是否要重新渲染
    componentWillUpdate -- 很少使用。它可以用于代替组件的 componentWillReceiveProps 和 shouldComponentUpdate(但不能访问之前的 props)
    componentDidUpdate -- 常用于更新 DOM，响应 prop 或 state 的改变
    componentWillUnmount -- 在这你可以取消网络请求，或者移除所有与组件相关的事件监听器


### setState

  setState是异步的。每次调用setState都会触发更新，异步操作是为了提高性能，将多个状态合并一起更新，减少re-render调用。

  ```js
  // num将会只是1
    for ( let i = 0; i < 100; i++ ) {
        this.setState( { num: this.state.num + 1 } );
    }

  // 对象式
  this.setState(object, [callback])
  // 函数式
  this.setState(function, [callback])
  ```

  
  **如果对象式和函数式的setState混合使用，则对象式的会覆盖前面无论函数式还是对象式的任何setState，
  但是不会影响后面的setState。**
  
  1. setState在react生命周期和合成事件会批次覆盖执行。比如当多个setState调用的时候会提取传递setState对象，就像Object.assign的对象合并，相同最后的一个key会覆盖前面的key

  2. setState在原生事件，setTimeout，setInterval，Promise等异步操作中，state会同步更新。多次连续操作setState，每次都会re-render，state会同步更新。

  **setState的调用会引起React的更新生命周期的4个函数执行。**

    shouldComponentUpdate
    返回true,不更新state，进行下一步；返回false，更新state
    componentWillUpdate
    在此阶段也没有更新state
    render
    更新state
    componentDidUpdate

   **setState的缺点**

    1. **setState可能循环调用。**在shouldComponentUpdate没有返回false，且在其他三个会经历setState的周期函数里调用会形成循环调用，最终导致浏览器内存占满后崩溃

    2. 引发不必要的渲染。

    3. 不能总是有效管理组件中的所有所有状态。某些与视图无关的state用setState去管理不合适。

    





