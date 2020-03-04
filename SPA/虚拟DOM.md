### 真实DOM的渲染过程


1. 解析HTML文件，从上往下生成DOM TREE。
2. 解析CSS文件，从右往左生成 RULE TREE。
3. DOM TREE和RULE TREE结合，计算样式并生成渲染对象的过程为attachment，
每个dom节点有一个attach方法，attachment的过程是同步的，调用新节点的attach方法插入到dom树中。生成RENDER TREE。
4. 有了RENDER TREE，浏览器开始布局（layout）,绘制(paint)

对真实DOM的操作，会导致回流和重绘，从而影响性能。


### Virtual DOM

  Virtual DOM可以理解是真实DOM的JS对象映射，用对象的形式去模拟和表达真实DOM。


### 为什么是Vitual DOM

 1. 减少真实DOM的操作。 DOM的增删改不会立即对真实DOM进行操作，而是将这些改动都保存在JS对象中，
 再一次性去attachment到DOM树。从而减少真实DOM操作频率。

 2. 真实的DOM节点，有许多属性和方法。使用虚拟节点，能在数据存储上节省内存。

