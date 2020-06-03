[参考资料](https://juejin.im/post/58e3a5a0a0bb9f0069fc16bb#heading-8)


- 基本概念

  主轴(main axis)、交叉轴(cross axis)、父容器(container)、子容器(item)

  主轴默认X方向（从左到右）、交叉轴默认Y方向（从下到上）


- 父容器属性

  - 设置子容器沿主轴排列: jusify-content: center/flex-start/flex-end/space-between(首尾两端的子容器到父容器的距离是子容器间距的一半)/space-around(首尾两端的字容器与父容器相切)

  - 设置子容器如何沿交叉轴排列: align-items: flex-start/flex-end/center/baseline(基线对齐,这里的 baseline 默认是指首行文字，即 first baseline，所有子容器向基线对齐，
  **交叉轴起点到元素基线距离最大的子容器将会与交叉轴起始端相切以确定基线。**)/stretch(子容器如果设置高度则不会起作用)



- 子容器属性

  - 设置子容器沿主轴排列：flex:flex-basis（相当于设置宽度，优先级高于width）/flex-grow(扩大,当父容器宽度大于所有子元素的宽度和时,子元素如何分配剩余空间，默认为0，值越大占据宽度越大)/flex-shrink(收缩,当父容器宽度小于所有子元素的宽度和时，子元素如何缩减自己的宽度，默认为1，0为不减，值越大减的越多)。
  


  - 设置子容器沿交叉轴排列方式：align-self:flex-start/flex-end/center/baseline/stretch


- 轴

  - flex-direction: row/column/row-reverse/column-reverse 改变主轴/交叉轴方向以及排列顺序

- 常用属性

  - 换行。flex-wrap: no-wrap/wrap/rap-reverse  不换行/换行/逆序换行
  - **多行沿交叉轴对齐**。align-content：flex-start/flex-end/center/space-between/space-around/stretch