[参考资料](http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html)

**采用网格布局的区域，称为"容器"（container）。容器内部采用网格定位的子元素，称为"项目"（item）。**

行和列的交叉区域，称为"单元格"（cell）。

正常情况下，n 行和 m 列会产生 n x m 个单元格。比如，3 行 3 列会产生 9 个单元格。

Grid 布局的属性分成两类。一类定义在容器上面，称为容器属性；另一类定义在项目上面，称为项目属性。

**注意，设为网格布局以后，容器子元素（项目）的 float、display: inline-block、display: table-cell、vertical-align 和 column-\*等设置都将失效。**

# 容器

- grid-template-columns 列宽
- grid-template-rows 行高
- repeat()

```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 33.33%);
  grid-template-rows: repeat(3, 33.33%);
}
```

```css
grid-template-columns: repeat(2, 100px 20px 80px);
```

- auto-fill 关键字

有时，单元格的大小是固定的，但是容器的大小不确定。如果希望每一行（或每一列）容纳尽可能多的单元格，这时可以使用 auto-fill 关键字表示自动填充。

```css
/* 每列宽度100px，然后自动填充，直到容器不能放置更多的列 */
.container {
  display: grid;
  grid-template-columns: repeat(auto-fill, 100px);
}
```

- fr 关键字

为了方便表示比例关系，网格布局提供了 fr 关键字（fraction 的缩写，意为"片段"）。如果两列的宽度分别为 1fr 和 2fr，就表示后者是前者的两倍。
第一列的宽度为 150 像素，第二列的宽度是第三列的一半。

```css
.container {
  display: grid;
  grid-template-columns: 150px 1fr 2fr;
}
``
```

- minmax()

minmax()函数产生一个长度范围，表示长度就在这个范围之中。它接受两个参数，分别为最小值和最大值。

```css
grid-template-columns: 1fr 1fr minmax(100px, 1fr);
```

- auto

auto 关键字表示由浏览器自己决定长度。

```css
grid-template-columns: 100px auto 100px;
```

第二列的宽度，**基本上等于该列单元格的最大宽度，除非单元格内容设置了 min-width，且这个值大于最大宽度。**

- 网格线名称
  在一个 3x3 格子里，存在 4x4 的网格线

```css
.container {
  display: grid;
  grid-template-columns: [c1] 100px [c2] 100px [c3] auto [c4];
  grid-template-rows: [r1] 100px [r2] 100px [r3] auto [r4];
}
```

### 间距

- grid-row-gap 行间距

- grid-column-gap 列间距

- grid-gap 缩写（row，col）

### area 区域

```css
grid-template-areas:
  "a a a"
  " . . ."
  "c c c";
```

将 9 个单元格分为三个区域，其中.标示该区域不需要利用

**注意，区域的命名会影响到网格线。每个区域的起始网格线，会自动命名为区域名-start，终止网格线自动命名为区域名-end**

### 顺序

- grid-auto-flow: row(默认)/column/row dense/ column dense

划分网格以后，容器的子元素会按照顺序，自动放置在每一个网格。**默认的放置顺序是"先行后列"，即先填满第一行，再开始放入第二。**
dense 表示尽量去占满空格

### 整个内容区域在容器位置

- justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
- align-content: start | end | center | stretch | space-around | space-between | space-evenly;

### 子元素在单元格内的位置

- justify-items: start | end | center | stretch(占满);
- align-items: start | end | center | stretch;
- place-items: [align-items] [justify-items];

### 自增列宽和行高

- grid-auto-columns
- grid-auto-rows

有时候，一些项目的指定位置，在现有网格的外部。比如网格只有 3 列，但是某一个项目指定在第 5 行。这时，浏览器会自动生成多余的网格，以便放置项目。

# 项目属性

- grid-column-start 属性：左边框所在的垂直网格线
- grid-column-end 属性：右边框所在的垂直网格线
- grid-row-start 属性：上边框所在的水平网格线
- grid-row-end 属性：下边框所在的水平网格线

这四个属性的值还可以使用 span 关键字，表示"跨越"，即左右边框（上下边框）之间跨越多少个网格。

```css
.item-1 {
  grid-column-start: span 2;
}
```

简写：

- grid-column: [start-line] / [end-line];
- grid-row: [start-line] / [end-line];

### 区域

- grid-area

```css
.item-1 {
  grid-area: e;
}
.item2 {
  grid-area: <row-start> / <column-start> / <row-end> / <column-end>;
}
```

### 单个元素位置

- justify-self: start | end | center | stretch;
- align-self: start | end | center | stretch;
- place-self: <justify-self> <align-self>;
