## PureComponent 、Component

[参考资料](https://blog.csdn.net/ll18781132750/article/details/81206979)

    PureComponent即纯组件，用于展示组件，
    只需要把继承类从component转化为PureComponent，
    可以减少DOM不必要的render，提高西能。


- 当前问题，默认渲染行为


shouldComponentUpdate方法，默认值是true。即当我们更改组件中不需要刷新组件的Props或State，甚至没有改变组件的props或state，也会导致组件的重绘。极大的降低了React的渲染效率。


- 实现原理
**内部自己做了一层比较，会比较 Object.keys(state | props) 的长度是否一致，每一个 key 是否两者都有，并且是否是一个引用，也就是只比较了第一层的值，确实很浅，所以深层的嵌套数据是对比不出来的。**


- 使用注意点

确保数据类型是值类型
如果是引用类型，不应当有深层次的数据变化(解构).
