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


编写Loader时要遵循单一原则，每个Loader只做一种"转义"工作。
每个Loader的拿到的是源文件内容（source），可以通过返回值的方式将处理后的内容输出，
也可以调用this.callback()方法，将内容返回给webpack。
还可以通过 this.async()生成一个callback函数，再用这个callback将处理后的内容输出出去。 
此外webpack还为开发者准备了开发loader的工具函数集——loader-utils。