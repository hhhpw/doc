link 属于 XHTML 标签，而@import 是 CSS 提供的。
页面被加载时，link 会同时被加载，而@import 引用的 CSS 会等到页面被加载完再加载。
import 只在 IE 5 以上才能识别，而 link 是 XHTML 标签，无兼容问题。
link 方式的样式权重高于@import 的权重。
使用 dom 控制样式时的差别。当使用 javascript 控制 dom 去改变样式的时候，只能使用 link 标签，因为@import 不是 dom 可以控制的。

1.从属关系区别
@import 是 CSS 提供的语法规则，只有导入样式表的作用；link 是 HTML 提供的标签，不仅可以加载 CSS 文件，还可以定义 RSS、rel 连接属性等。

2.加载顺序区别
加载页面时，link 标签引入的 CSS 被同时加载；@import 引入的 CSS 将在页面加载完毕后被加载。

3.兼容性区别
@import 是 CSS2.1 才有的语法，故只可在 IE5+ 才能识别；link 标签作为 HTML 元素，不存在兼容性问题。

4.DOM 可控性区别
可以通过 JS 操作 DOM ，插入 link 标签来改变样式；由于 DOM 方法是基于文档的，无法使用@import 的方式插入样式。

5.权重区别(该项有争议，下文将详解)
link 引入的样式权重大于@import 引入的样式。


  ```css
<style type=”text/css”>
    @import url（“a.css”）；
</style>
  ```