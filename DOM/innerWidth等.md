![demo](../images/FirefoxInnerVsOuterHeight2.png)

-window.outerHeight、window.outerWidth 为窗口的整个外层高度和宽度(px)，
包括浏览器的 tab 标签页等。

- window.innerHeight、window.innerWidth 为窗口展示区域的高度和宽度(px).

- el.clientWidth、 el.clientHeight

  1.内联元素以及没有 css 样式的元素的 clientWidth 属性值都为 0

  2. **该属性包括 padding！！**。但如果 box-sizing 是 border-box，该属性就为 width/height
