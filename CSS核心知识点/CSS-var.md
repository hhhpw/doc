一直以来，不能在CSS里使用变量的问题被人诟病，只能借助于SCSS、LESS等编译语言。但在17年终有所该改善，CSS支持变量，强大的变量功能，为广大开发者提供了便利。

### 概述
 var()函数可以代替元素中任何属性中的值的任何部分。var()函数不能作为属性名、选择器或者其他除了属性值之外的值。[MDN释义](https://developer.mozilla.org/zh-CN/docs/Web/CSS/var)

### 变量声明
声明变量时候需要在变量前增加两根连词线*--*

### 作用域
**：root** 里的变量将作为全局的变量。局部作用域：作用于选择器内。同名变量将会被覆盖。

### var函数
```css
var( <custom-property-name> , <declaration-value>? )
```
方法的第一个参数是要替换的自定义属性的名称。函数的可选第二个参数用作回退值。如果第一个参数引用的自定义属性无效，则该函数将使用第二个值。[MDN释义](https://developer.mozilla.org/zh-CN/docs/Web/CSS/var)
- 注意
1. 变量值只能用作属性值，不能用作属性名
2. 自定义属性的回退值允许使用逗号。例如， var(--foo, red, blue) 将red, blue同时指定为回退值；即是说任何在第一个逗号之后到函数结尾前的值都会被考虑为回退值。
3. 带有数值单位时候，需要与calc结合使用

### 例子
```css 
.div {
  width: 100px;
  height: 100px;
  text-align: center;
  line-height: 100px;
}
```
- 例一
```css
    :root {
      --color: red;  /*全局变定义变量*/
    }

    .div {
      --color: yellow; /*重新定义color为yellow，.div将会是yellow,而不是全局定义的red*/
      background: var(--color); /*引用变量--color*/
    }
```

- 例二
```css
:root {
      --red: red;
      --yellow: yellow;
    }

.div {
    /* 普通回退值。并未定义--color变量，将使用blue作为背景色*/
    background: var(--color, blue);
	/* 变量回退值。并未定义--color变量,引用另一个变量--yellow作为背景色*/
    background: var(--color, var(--yellow));
  }
```

- 例三
```css
    :root {
      --pt: padding-top;
    }

    .div {
      /* 将变量作为属性名，样式不会应用*/
      var(--pt): 10px;
    }
```

- 例四
```css

    :root {
      --ml: 5;
    }
    .div {
      /* 该CSS不会作用 */
      margin-left: var(--ml);
      /* 结合calc一起使用 */
      margin-left: calc(var(--ml) * 1px);
    }
```

### js操作
- 读取
```js
// 注意，该方法只能读取通过JS设置的属性，在CSS里设置里的将不会读取
 document.querySelector('div').style.getPropertyValue('--color');
```
- 赋值
```js
document.querySelector('div').style.setProperty('--color', 'gold');
```

- 删除
```js
// 注意，该方法只能删除通过JS设置的属性，在CSS里设置里的将不会读取
document.querySelector('div').style.removeProperty('--color');
```

### 兼容性
目前基本上所有浏览器都支持该属性，也可以通过MDN取查询支持度。


