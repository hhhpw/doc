![示意图](../images/render.png)

**DOM 解析和 CSS 解析是两个并行的进程**，这也是为什么 CSS 加载不会影响 DOM 解析的原因。
**RENDER TREE 依赖于 DOM TREE 和 CSS RULE TREE**，因此，页面的渲染要等 CSS RULE TREE（即使外部 CSS 依赖加载失败,也需等待）和 DOM TREE 解析完成, 从而 CSS 加载影响 DOM 的渲染。

## 渲染过程

1.HTML 解析文件，从上至下，顺序执行，生成 DOM Tree，解析 CSS 文件生成 CSS RULE Tree

2.将 Dom Tree 和 CSS DOM Tree 结合，生成 Render Tree(渲染树)

3.根据 Render Tree 渲染绘制，将像素渲染到屏幕上。 RENDER TREE -> LAYOUT（布局） -> PAINT(绘制)

## CSS

1.**CSS 的加载不会影响 DOM 树的解析,但是会影响 DOM 树的渲染**

2.**CSS 的加载会影响在之后面的 JS 文件执行**

3.CSS 不会阻塞外部 JS 的加载，但会阻塞执行

## JS

1.JS 顺序执行

2.JS 会阻塞 html 的解析和渲染

3.没有 defer 和 async 标签的 script 会立即加载并执行

4.有 async 标签的 js，js 的加载执行和 html 的解析和渲染并行，下载完成后立即执行，不依赖 CSS 和 HTML，**加载顺序不固定**

5.有 defer 标签的 js，js 的加载和 html 的解析和渲染并行，但会在 html 解析完成后执行,**在触发 DOMContentLoaded 事件执行**

如果同时使用 async 和 defer 属性，后者不起作用，浏览器行为由 async 属性决定。

---

     在head中如果遇到script标签，会暂停html的解析，将网页的渲染权交给JS引擎。
     如果<script>标签引用了外部脚本，就下载该脚本，否则就直接执行。
     执行完毕，控制权再交给渲染引擎，恢复下载解析HTML网页

## 重绘

重绘（repaint）: 当元素的一部分属性发生变化，如外观背景色不会引起布局变化而需要重新渲染的过程叫重绘

## 回流

回流（reflow）: 当 DOM 数中一部分或者大部分因为元素的大小、布局、隐藏等改变而需要重新构建。

- 减少 reflow 的方法：

  1. 尽可能限制 reflow 的影响范围，需要改变元素的样式。比如直接修改子元素，不要通过父元素
  2. 通过设置 class 去修改样式，减少通过 style 样式直接去修改
  3. 尽可能回流的范围，例如通过 marginLeft 去代替 left

**回流必然会引起重绘，重绘不一定引起回流。**

##### 导致回流的操作：

    页面首次渲染
    浏览器窗口大小发生改变
    元素尺寸或位置发生改变
    元素内容变化（文字数量或图片大小等等）
    元素字体大小变化
    添加或者删除可见的DOM元素
    激活CSS伪类（例如：:hover）
    查询某些属性或调用某些方法

## 监听加载完成

```js
document.addEventListener("DOMContentLoaded", function () {
  console.log("A");
  // DOM 渲染完即可执行，此时图片、视频还可能没有加载完
});
window.addEventListener("load", function () {
  console.log("B");
  // 页面的全部资源加载完才会执行，包括图片、视频等
});
```

## 优化建议

1.使用 CDN(因为 CDN 会根据你的网络状况，替你挑选最近的一个具有缓存内容的节点为你提供资源，因此可以减少加载时间)

2.对 css 进行压缩(可以用很多打包工具，比如 webpack,gulp 等，也可以通过开启 gzip 压缩)

3.合理的使用缓存(设置 cache-control,expires,以及 E-tag 都是不错的，不过要注意一个问题，就是文件更新后，你要避免缓存而带来的影响。其中一个解决防范是在文件名字后面加一个版本号)

4.减少 http 请求数，将多个 css 文件合并，或者是干脆直接写成内联样式(内联样式的一个缺点就是不能缓存)
