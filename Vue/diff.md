### 核心观念

**两个相同的组件产生类似的 DOM 结构，不同的组件产生不同的 DOM 结构。**

**同一层级的一组节点，他们可以通过唯一的 id 进行区分。**

[参考资料](https://www.cnblogs.com/wind-lanyan/p/9061684.html)

Dep.notify
patch(oldVnode, Vnode)

```js
if (sameVnode) {
  patchVnode
  if (oldVnode有子节点,Vnode没有) {

  }
  if (oldVnode没有子节点,,Vnode有) {

  }
  if (都有文本节点) {

  }
  if (都有子节点) {

  }
} else {
  replace vnode
  return
}



```
