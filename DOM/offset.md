# 偏移量

一共四个属性。 一个定位父级 offsetParent

- offsetLeft
- offsetTop
- offsetHeight
- offsetWidth
- offsetParent

## offsetParent

与当前元素最近的定位（即 position 属性不为 static！！！）父级元素

1. 当元素自身有 fixed 固定定位时，offsetParent 的结果为 null。
   原因在于固定定位的元素相对于视口进行定位，此时是没有父级的。

2. 元素自身没有 fixed 定位，且父元素都未经过定位，offsetParent 为 body。

3. 元素自身无 fixed 定位，且父元素存在定位的元素，offsetParent 为离自身元素经过的最近的父级元素。

4. body 元素的 parentNode 是 null。

## offsetWidth

offsetWidth = width + 左右 padding + 左右 border

## offsetHeight

同上

## offsetTop

offsetTop 表示元素的上外边框至 offsetParent 元素的上内边框之间的像素距离

## offsetLeft

offsetLeft 表示元素的左外边框至 offsetParent 元素的左内边框之间的像素距离
