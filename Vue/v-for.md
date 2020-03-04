## 为什么要用key

  virtual DOM以及DIFF算法

  决定节点是否可以复用
  建立key-index的索引,主要是替代遍历，提升性能
  Leeesin/wheels

## 为什么不用索引作为Key

 在一个列表里，删除其中一项，由于key与index绑定，会导致后面的元素都重新渲染，
 影响性能。