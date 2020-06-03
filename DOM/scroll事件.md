## scrollTo

Window.scroll()
Window.scrollBy()
Window.scrollTo()
Element.scroll()
Element.scrollBy()
Element.scrollTo()

[ScrollToOptions](https://developer.mozilla.org/en-US/docs/Web/API/ScrollToOptions)

```ts
{
  left: number,
  top: number,
  behavoir: "smooth" | "auto"
}

```
## scrollIntoView

[Options](https://blog.csdn.net/THINK_OF_/article/details/85338033)


```ts
{
  behavior: "auto" | "instant" | "smooth", // 默认 auto
  block: "start" | "center" | "end" | "nearest", // 默认 center
  inline: "start" | "center" | "end" | "nearest", // 默认 nearest
}

```
behavior 表示滚动方式。auto 表示使用当前元素的 scroll-behavior 样式。instant 和 smooth 表示 直接滚到底 和 使用平滑滚动。
block 表示块级元素排列方向要滚动到的位置。对于默认的 writing-mode: horizontal-tb 来说，就是竖直方向。start 表示将视口的顶部和元素顶部对齐；center 表示将视口的中间和元素的中间对齐；end 表示将视口的底部和元素底部对齐；nearest 表示就近对齐。
inline 表示行内元素排列方向要滚动到的位置。对于默认的 writing-mode: horizontal-tb 来说，就是水平方向。其值与 block 类似。