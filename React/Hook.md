## hook

#### 使用规则
**1.只在最顶层使用 Hook**
**2.不要在循环，条件或嵌套函数中调用 Hook**
[学习文档](https://react.docschina.org/docs/hooks-intro.html)

#### 为什么不能在条件嵌套语句使用


**在 React 中，Hooks 是基于调用顺序的。这意味着 Hooks 的调用顺序必须保持稳定，不能在组件的渲染过程中发生变化。**
如果将 Hooks 放在条件语句（比如 if、for、while 等）中，就会导致 Hooks 的调用顺序在不同渲染周期之间发生变化，从而违反了 React Hooks 的规则。
具体来说，当组件重新渲染时，React 会根据 Hooks 的调用顺序来确定每个 Hook 对应的状态。
如果 Hooks 放在条件语句中，那么条件满足与否可能会导致不同的 Hooks 被调用，这样就会破坏 Hook 调用顺序的稳定性，可能会导致组件状态的混乱和不一致。



Hook 是 React 16.8 的新增特性。它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。
Hook 是什么？ Hook 是一个特殊的函数，它可以让你“钩入” React 的特性。例如，useState 是允许你在 React 函数组件中添加 state 的 Hook。稍后我们将学习其他 Hook。
什么时候我会用 Hook？ 如果你在编写函数组件并意识到需要向其添加一些 state，以前的做法是必须将其它转化为 class。现在你可以在现有的函数组件中使用 Hook。

```js
  import React, { useState } from 'react';

  function Example() {
    const [count, setCount] = useState(0);

      useEffect(() => {
          // Update the document title using the browser API
          document.title = `You clicked ${count} times`;
        });

   return (
     <div>
       <p>You clicked {count} times</p>
       <button onClick={() => setCount(count + 1)}>
        Click me
       </button>
      </div>
   );
 }
```
#### useState

调用 useState 方法的时候做了什么? 它定义一个 “state 变量”。我们的变量叫 count， 但是我们可以叫他任何名字，比如 banana。这是一种在函数调用时保存变量的方式 —— useState 是一种新方法，它与 class 里面的 this.state 提供的功能完全相同。一般来说，在函数退出后变量就会”消失”，而 state 中的变量会被 React 保留。

useState 需要哪些参数？ useState() 方法里面唯一的参数就是初始 state。不同于 class 的是，我们可以按照需要使用数字或字符串对其进行赋值，而不一定是对象。在示例中，只需使用数字来记录用户点击次数，所以我们传了 0 作为变量的初始 state。（如果我们想要在 state 中存储两个不同的变量，只需调用 useState() 两次即可。）

useState 方法的返回值是什么？ 返回值为：当前 state 以及更新 state 的函数。这就是我们写 const [count, setCount] = useState() 的原因。这与 class 里面 this.state.count 和 this.setState 类似，唯一区别就是你需要成对的获取它们。如果你不熟悉我们使用的语法，我们会在本章节的底部介绍它。


 **实现原理**

 在每个状态 Hook（如 useState）节点中，会通过 queue 属性上的循环链表记住所有的更新操作，并在 updade 阶段依次执行循环链表中的所有更新操作，最终拿到最新的 state 返回。
 

#### useEffect
**Effect Hook 可以让你在函数组件中执行副作用操作， react 组件需要在渲染后执行某些操作**

useEffect 做了什么？ 通过使用这个 Hook，你可以告诉 React 组件需要在渲染后执行某些操作。React 会保存你传递的函数（我们将它称之为 “effect”），并且在执行 DOM 更新之后调用它。在这个 effect 中，我们设置了 document 的 title 属性，不过我们也可以执行数据获取或调用其他命令式的 API。

为什么在组件内部调用 useEffect？ 将 useEffect 放在组件内部让我们可以在 effect 中直接访问 count state 变量（或其他 props）。我们不需要特殊的 API 来读取它 —— 它已经保存在函数作用域中。Hook 使用了 JavaScript 的闭包机制，而不用在 JavaScript 已经提供了解决方案的情况下，还引入特定的 React API。

useEffect 会在每次渲染后都执行吗？ 是的，默认情况下，它在第一次渲染之后和每次更新之后都会执行。（我们稍后会谈到如何控制它。）你可能会更容易接受 effect 发生在“渲染之后”这种概念，不用再去考虑“挂载”还是“更新”。React 保证了每次运行 effect 的同时，DOM 都已经更新完毕。


执行两次？
在开发模式下，使用严格模式下会触发。
之所以执行两次：**是因为模拟立即卸载组件和重新挂载组件，帮助开发者提前发现和重复挂载造成的bug.**
```js

import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

### useMemo

useMemo 是一个 React Hook，它在每次重新渲染的时候能够缓存计算的结果。记忆优化。
useMemo 缓存函数调用的结果。在这里，它缓存了调用 computeRequirements(product) 的结果。除非 product 发生改变，否则它将不会发生变化。这让你向下传递 requirements 时而无需不必要地重新渲染 ShippingForm。必要时，React 将会调用传入的函数重新计算结果。

### useCallback
useCallback 是一个允许你在多次渲染中缓存函数的 React Hook。
useCallback 缓存函数本身。不像 useMemo，它不会调用你传入的函数。相反，它缓存此函数。从而除非 productId 或 referrer 发生改变，handleSubmit 自己将不会发生改变。这让你向下传递 handleSubmit 函数而无需不必要地重新渲染 ShippingForm。直至用户提交表单，你的代码都将不会运行。


### useReducer
useReducer 是一个 React Hook，它允许你向组件里面添加一个 reducer。

通常用于替代 useState 进行复杂状态管理，尤其是在处理多个子状态或者更复杂的状态更新逻辑时，useReducer 比 useState 更具可读性和可维护性。它与 Redux 的工作原理类似，允许你使用 reducer 函数来管理状态更新。

```js
import React, { useReducer } from 'react';

// 定义初始状态
const initialState = { count: 0 };

// 定义 reducer 函数
function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    case 'reset':
      return { count: 0 };
    default:
      return state;
  }
}

function Counter() {
  // 使用 useReducer hook
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>Increment</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>Decrement</button>
      <button onClick={() => dispatch({ type: 'reset' })}>Reset</button>
    </div>
  );
}

export default Counter;

```
使用场景：
1. 复杂的状态逻辑：如果你的组件有多个状态字段或状态更新逻辑依赖于前一个状态，useReducer 是一个更好的选择
2. 状态更新逻辑需要解耦：useReducer 可以帮助你将多个状态更新逻辑放在一个函数中，这使得状态的管理更加清晰、易于维护。
3. 多个相关状态：当多个状态是相互依赖的，useReducer 可以让你将它们组合成一个状态对象，从而让更新逻辑更加易于控制。



### useContext

useContext 是一个 React Hook，可以让你读取和订阅组件中的 context。
1.向组件数深层传递数据。
2.通过context更新传递后的数据。
```js

// context.ts
const ThemeContext = createContext(null);

// app.ts
function App() {
  const [theme, setTheme] = useState('light');
  <ThemeContext.provider value={theme}>
    <Child />
  </ThemeContext.provider>
}
// child.ts
function Child() {
  const theme = useContext(ThemeContext);
  ////

}


```

### useRef
1. 使用用 ref 引用一个值
2. 通过 ref 操作 DOM
3. 避免重复创建 ref 的内容