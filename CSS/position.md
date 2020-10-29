- absolute

      绝对定位，相对于出了 static 定位以外的第一个父元素进行定位。

- fixed

      生成固定定位，相对于浏览器窗口进行定位。

- relative

      生成相对定位，相对于其正常位置进行定位。

- sticky

      粘性定位，该定位基于用户滚动的位置。
      粘性定位的元素是依赖于用户的滚动，在 position:relative 与 position:fixed 定位之间切换。
      它的行为就像 position:relative; 而当页面滚动超出目标区域时，它的表现就像 position:fixed;，它会固定在目标位置。

```css
.container {
  background: #eee;
  width: 600px;
  height: 1000px;
  margin: 0 auto;
}

.sticky-box {
  position: -webkit-sticky;
  position: sticky;
  height: 60px;
  margin-bottom: 30px;
  background: #ff7300;
  top: 0px;
}

h1 {
  font-size: 30px;
  text-align: center;
  color: #fff;
  line-height: 60px;
}
```

```html
<div class="container">
  <h1 class="sticky-box">标题一</h1>
  <h1 class="sticky-box">标题二</h1>
  <h1 class="sticky-box">标题三</h1>
  <h1 class="sticky-box">标题四</h1>
</div>
```

- inital 默认值

#### 拓展

translate 也可以改变定位，但不会引起重绘和回流。但是仍然占据其文档空间。
