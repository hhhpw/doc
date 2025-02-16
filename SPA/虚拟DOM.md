### 真实 DOM 的渲染过程

1. 解析 HTML 文件，从上往下生成 DOM TREE。
2. 解析 CSS 文件，从右往左生成 RULE TREE。
3. DOM TREE 和 RULE TREE 结合，计算样式并生成渲染对象的过程为 attachment，
   每个 dom 节点有一个 attach 方法，attachment 的过程是同步的，调用新节点的 attach 方法插入到 dom 树中。生成 RENDER TREE。
4. 有了 RENDER TREE，浏览器开始布局（layout）,绘制(paint)

对真实 DOM 的操作，会导致回流和重绘，从而影响性能。

### Virtual DOM

Virtual DOM 可以理解是真实 DOM 的 JS 对象映射，用对象的形式去模拟和表达真实 DOM。

### 为什么是 Vitual DOM
1.  减少真实 DOM 的操作。 DOM 的增删改不会立即对真实 DOM 进行操作，而是将这些改动都保存在 JS 对象中，
    再一次性去 attachment 到 DOM 树。从而减少真实 DOM 操作频率。
2.  真实的 DOM 节点，有许多属性和方法。使用虚拟节点，能在数据存储上节省内存。
3. 在 React 或 Vue 中，状态变更不会立即导致 DOM 更新，而是 批量收集多次状态变更，等到事件循环结束后进行统一更新，减少了无意义的中间状态更新，提高了渲染效率。
4. 垮平台能力
5. 减少重排和重绘

### 相关库

snabbdom 核心函数:h
