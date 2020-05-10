
[参考文档](https://juejin.im/post/5ea45801e51d4546d4399055#heading-21)
### BFC(Block formatting context)
[参考文档](https://blog.csdn.net/qq_43004614/article/details/90691509)

- 定义

块级格式化上下文。BFC就是一个css的一个布局概念，是一个独立的区域，是一个环境,拥有一套渲染规则，
决定其子元素将如何定位，以及和其他元素的关系和相互作用。

- 规则

1.内部的块级元素会在垂直方向，一个接一个的放置
2.块级垂直方向的距离由margin决定，属于同一个bfc的两个相邻块级元素的margin会发生重叠
3.每个元素的margin box的左边，与包含块border box的左边相接触（对于从做往右的格式化，否则相反）;
4.bfc的区域不会与浮动区域的box重叠
5.bfc是一个页面上的独立的容器，外面的元素不会影响bfc里的元素，反过来，里面的也不会影响外面的
6.计算bfc高度的时候，浮动元素也会参与计算

- BFC特性

（1）所有子元素（包含浮动元素）与容器（父元素）左边对齐
（2）属于同一个BFC的父元素和子元素，相邻的父子或者兄弟间margin垂直方向会重叠，若2个元素属于不同的BFC，则垂直方向不会重叠
（3）可以自动撑开容器（若子元素是float的，父元素设置overflow:hidden，父元素就形成一个BFC）

- 怎么取创建bfc

float属性不为none（脱离文档流）
position为absolute或fixed
display为inline-block,table-cell,table-caption,flex,inine-flex
overflow不为visible


- 应用场景:

自适应两栏布局
清除内部浮动 
防止垂直margin重叠
子浮动，父盒高度塌陷


### LFC(Inline Formatting Contexts)
[参考文档](https://blog.csdn.net/ixygj197875/article/details/79344472?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)
- 定义

只有行内级框参与的格式化上下文，称作行格式化上下文（Inline Formatting Contexts，简称IFC）行内格式化上下文。

- 怎么创建LFC
IFC的形成条件非常简单，块级元素中仅包含内联级别元素，需要注意的是当IFC中有块级元素插入时，会产生两个匿名块将父元素分割开来，产生两个IFC。


- 渲染规则

1.子元素水平方向横向排列，并且垂直方向为元素顶部
2.子元素只会计算横向样式空间(padding,border,margin),垂直方向上不会被计算
3.把包含了一行内所有行内级框的矩形区域，称作行框（line box）。行框是本行一个虚拟的框，是浏览器渲染模型中的一个概念，并没有实际显示出来。
4.IFC一般左右排列，但float元素会优先排列
5.当line-box超过父元素宽度时。如果子元素未设置强制换行的情况下，line-box将会溢出父元素


line box 的高度始终容下所有的 inline box ，并只与行高有关。
line box 的宽度受到父容器和浮动元素存在的影响(这就是文本环绕浮动元素)。如果 line box 的宽度小于容器， line box 的水平排布就取决于 text-align 。如果 line box 的宽度大于容器，则截断 line box 并换行。在新的 line box 中重新排布元素(截断处不应用 padding 和 margin 值)。如果 line box 无法截断，如单词过长或者指定不换行，则会溢出容器。

vertical-align 决定了垂直对齐的方式

宽度不够时会自动换行

宽度不够时如果文本是单词是不会自动换行 
需要手动设置word-wrap:break-word;
word-break:break-all;


在LFC内，子元素浮动仍在该盒子内