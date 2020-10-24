```js
// 记得一定要全局安装 -g
cnpm i webpack --save-dev
cnpm install --save-dev webpack-cli
```

#### loader 的执行顺序是从下往上，从右到左

loader 其实是一个函数，参数是匹配文件的源码，返回结果是处理后的源码。

```js
// source为匹配文件的源码 返回为string类型
// index.js
`<div>
  <h1>这是标题</h1>
  <p>这是文本哈哈！</p>
</div>`;

// loader.js
module.exports = function (source) {
  let content = source.replace(/<p>/g, "<p class='name'>");
  let s = `
    let style = document.createElement('style');
    style.innerHTML = '.name{color:red}'
    document.head.appendChild(style);
    document.body.innerHTML = ${content};
  `;
  return s;
};
```
